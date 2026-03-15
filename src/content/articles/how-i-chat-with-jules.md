---
title: "How I Chat With Jules From My Phone (Full Code)"
date: 2026-03-03
description: "A file-based async messaging system for Claude Code. Drop a request in a markdown file from your phone, get results back in 20 seconds. No terminal needed."
story: 1
tags: ["claude-code", "async", "claude-p", "launchd", "automation", "syncthing"]
platforms:
  reddit: "https://www.reddit.com/r/ClaudeCode/comments/1rjpcgn/how_i_chat_with_jules_my_personal_assistant_build/"
draft: false
---

The problem with Claude Code is it's session-based. You sit down, open a terminal, do work. Great when you're at your desk.

This is inspired by [OpenClaw](https://openclaw.ai), which uses a similar async processing model. They do it over chat. I wanted file-based so it works with my existing sync setup.

Here's what I built.

---

## The experience

Open a note app on my phone. Add a line to `## Requests` in a file called `async-inbox.md`:

```
- Check if express has security patches since 4.18. Summarize what changed and whether we should upgrade.
```

Save. Within about 20 seconds the `## Reports` section in that same file updates with what the assistant did. Pull to refresh. The answer is there.

No terminal. No interactive session. Just a note and a result.

---

## The architecture

```
Phone (any notes app) → async-inbox.md → Syncthing → Mac
                                                      ↓
                                               launchd WatchPaths
                                                      ↓
                                          claude -p (read-only tools)
                                                      ↓
                                       Project files read, answer drafted
                                                      ↓
                                       Results written back to async-inbox.md
                                                      ↓
                                       Syncthing → Phone sees update
```

The file has two sections: `## Requests` where you drop items, `## Reports` where results come back. Syncthing keeps both devices in sync.

The full `async-inbox.md` format:

```markdown
# Async Inbox
<!-- Drop items in Requests. Auto-processed. Results in Reports. -->

## Requests
- Check if express has security patches since 4.18

## Reports
### 2026-03-03 10:14
- ✅ Express 4.19.2 patches 2 CVEs vs 4.18.x. Upgrade recommended. Details in project notes.
```

---

## The bash script

Here's the core of `inbox-process.sh`, sanitized with generic paths:

```bash
#!/usr/bin/env bash
# inbox-process.sh — Process async inbox requests via claude -p

set -euo pipefail

WORKSPACE="$HOME/projects"
INBOX="$WORKSPACE/async-inbox.md"
LOG_DIR="$HOME/.local/share/inbox-processor"
LOG_FILE="$LOG_DIR/inbox-process.log"
LOCKFILE="/tmp/inbox-process.lock"

# Ensure PATH includes homebrew and local bins
# (launchd runs with minimal environment)
for dir in /opt/homebrew/bin /usr/local/bin "$HOME/.local/bin"; do
    [ -d "$dir" ] && PATH="$dir:$PATH"
done
export PATH

mkdir -p "$LOG_DIR"
log() { echo "[inbox] $(date '+%Y-%m-%d %H:%M:%S') $*" | tee -a "$LOG_FILE"; }
```

**The self-trigger guard, this is critical:**

When the script writes results back to `async-inbox.md`, launchd fires *again*. Without a guard, you get an infinite loop.

```bash
# Our own writes to async-inbox.md trigger WatchPaths. Skip if we just wrote.
REENTRY_GUARD="/tmp/inbox-reentry-guard"
if [ -f "$REENTRY_GUARD" ]; then
    guard_age=$(( $(date +%s) - $(stat -f %m "$REENTRY_GUARD") ))
    if [ "$guard_age" -lt 5 ]; then
        exit 0
    fi
fi
```

**Checking for actual requests:**

```bash
# Extract content between ## Requests and ## Reports
REQUESTS=$(awk '/^## Requests$/{found=1;next}/^## Reports$/{exit}found' "$INBOX")
REQUESTS_TRIMMED=$(echo "$REQUESTS" | sed '/^[[:space:]]*$/d; /^[[:space:]]*-[[:space:]]*$/d')

if [ -z "$REQUESTS_TRIMMED" ]; then
    exit 0  # Nothing to do
fi
```

**The claude -p call:**

```bash
OUTPUT=$(timeout -k 15 180 claude -p \
    --model sonnet \
    --system-prompt "You process async inbox requests. Use Read, Glob, and Grep to research questions. Write answers clearly — the user will read these in a notes app on their phone. Keep responses brief and actionable." \
    --tools "Read,Glob,Grep" \
    --strict-mcp-config \
    --max-turns 3 \
    --output-format text \
    < "$INPUT_FILE" 2>"$LOG_DIR/claude-stderr.log") || true
```

The `--strict-mcp-config` flag is **not optional**. Without it, MCP servers from your project config start up, their children survive SIGTERM, and they hold stdout open. The `$()` substitution blocks forever waiting for output that never comes.

**Race detection, new items added while processing:**

```bash
# Re-read requests right before writing results
CURRENT_REQUESTS=$(awk '/^## Requests$/{found=1;next}/^## Reports$/{exit}found' "$INBOX")
CURRENT_TRIMMED=$(echo "$CURRENT_REQUESTS" | sed '/^[[:space:]]*$/d')

NEW_ITEMS=""
if [ "$CURRENT_TRIMMED" != "$REQUESTS_TRIMMED" ]; then
    # Items were added during processing — preserve them
    NEW_ITEMS=$(diff <(echo "$REQUESTS_TRIMMED") <(echo "$CURRENT_TRIMMED") \
        | grep '^>' | sed 's/^> //' || true)
    [ -n "$NEW_ITEMS" ] && log "New items arrived during processing — preserving"
fi
```

**Writing results back atomically:**

```bash
TIMESTAMP=$(date '+%Y-%m-%d %H:%M')
TMP_FILE=$(mktemp)

EXISTING_REPORTS=$(awk '/^## Reports$/{found=1;next}found' "$INBOX")

cat > "$TMP_FILE" << OUTEOF
# Async Inbox
<!-- Drop items in Requests. Auto-processed. Results in Reports. -->

## Requests
$NEW_ITEMS

## Reports
### $TIMESTAMP
$REPORT

$EXISTING_REPORTS
OUTEOF

# Set reentry guard BEFORE writing (launchd fires on write)
touch "$REENTRY_GUARD"
mv "$TMP_FILE" "$INBOX"
```

The `touch` before `mv` is intentional. WatchPaths can fire as soon as the move completes. If you set the guard after the write, there's a race window.

---

## The launchd plist

Save this as `~/Library/LaunchAgents/com.yourname.inbox-processor.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.yourname.inbox-processor</string>

    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>-l</string>
        <string>-c</string>
        <string>/path/to/inbox-process.sh</string>
    </array>

    <key>WatchPaths</key>
    <array>
        <string>/Users/yourname/projects/async-inbox.md</string>
    </array>

    <key>StandardOutPath</key>
    <string>/tmp/inbox-launchd.log</string>

    <key>StandardErrorPath</key>
    <string>/tmp/inbox-launchd.log</string>

    <key>EnvironmentVariables</key>
    <dict>
        <key>PATH</key>
        <string>/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin</string>
    </dict>
</dict>
</plist>
```

Note: `launchctl start` has an undocumented ~10 second cooldown between invocations. Don't spam it during testing and wonder why it's not firing.

---

## What works well

Anything read-only and research-oriented. "Does this library have any breaking changes in the latest major?" "What does our package.json say our Node version is?" "Summarize the last 5 commits to the auth module."

Claude gets `Read`, `Glob`, and `Grep`. Enough to navigate a codebase and give a real answer, not enough to write files while you're not watching.

## What doesn't

Anything that needs a live tool (web search, API calls). Anything complex enough that you'd want to iterate. For those, the report just says "needs a live session" with enough context to pick it up quickly.

---

The realization that made this click: `claude -p` isn't just a scripting tool. It's a background service when you pair it with a file queue and a WatchPaths trigger. The markdown file is the message queue. launchd is the event loop. The lockfile is the mutex. No daemon process required.

The whole script is about 300 lines. The `claude -p` call is 10 of them. The rest is guards and validation so it doesn't eat itself.

---

*Full source: [github.com/jonathanmalkin/jules](https://github.com/jonathanmalkin/jules)*
