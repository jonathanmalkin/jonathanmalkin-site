---
title: "Inside a 116-Configuration Claude Code Setup"
date: 2026-03-05
description: "A full audit of a production Claude Code workspace: 6 CLAUDE.md files, 29 skills, 5 agents, 22 rules, 8 hooks, 43 Makefile targets, and more. Every configuration explained."
story: 1
tags: ["claude-code", "skills", "hooks", "rules", "agents", "configuration"]
platforms:
  reddit: "https://www.reddit.com/r/ClaudeCode/comments/1rltiv7/inside_a_116configuration_claude_code_setup/"
  x: "https://x.com/builtwithjon/status/2029666421408436545"
draft: false
---

I run a small business with a custom web app, content pipeline, business operations, and the usual solopreneur overhead. But Claude Code isn't just my IDE. It's my thinking partner, decision advisor, and operational co-pilot. Equal weight goes to `Code/` and `Documents/`. Honestly, 80% of my time is in the Documents folder. Business strategy, legal research, content drafting, daily briefings. All through one terminal, one Claude session, one workspace.

After setting it up over a few months, I did a full audit. Here's what's actually in there.

---

## The Goal

Everything in this setup serves one objective: Jules operates autonomously by default. No hand-holding, no "what would you like me to do next?" It just does the work.

Three things stay human:

1. **Major decisions.** Strategy, money, anything hard to reverse. Jules presents options and a recommendation. I approve or push back.
2. **Deep thinking.** I drop a messy idea via voice dictation. Sometimes two or three rambling paragraphs. Jules extracts the intent, researches the current state, pulls information from the web, then walks me through an adversarial review process: different mental models, bias checks, pre-mortems, steelmanned counterarguments. But the thinking is still mine. Jules facilitates. I decide.
3. **Dangerous actions.** `sudo`, `rm`, force push, anything irreversible. The safety hook blocks these automatically.

Everything else? Fully enabled. Code, content, research, file organization, business operations. Jules just handles it and reports what happened at the end of the session.

That's the ideal, anyway. Still plenty of work to make that entire vision a reality. But the 116 configurations below are the foundation.

---

## The Total Count

| Category | Count |
|---|---|
| CLAUDE.md files (instruction hierarchy) | 6 |
| Skills | 29 |
| Agents | 5 |
| Rules files | 22 |
| Hooks | 8 |
| Makefile targets | 43 |
| LaunchAgent scheduled jobs | 2 |
| MCP servers | 1 |
| **Total** | **116** |

That's not counting the content inside each file. The bash-safety-guard hook alone is 90 lines of regex. The security-reviewer agent is a small novel.

---

## 1. The CLAUDE.md Hierarchy (6 files)

This is the foundation. Claude Code loads CLAUDE.md files at every level of your directory tree, and they stack. Mine go four levels deep:

**Global** (`~/.claude/CLAUDE.md`) is minimal. Points everything to the workspace-level file:

```markdown
# User Preferences

All preferences are in the project-level CLAUDE.md at ~/Active-Work/CLAUDE.md.
Always launch Claude from ~/Active-Work.
```

I keep this thin because I always launch from the same workspace. Everything lives one level down.

**Workspace root** (`~/Active-Work/CLAUDE.md`) is the real brain. Personality, decision authority, voice dictation parsing, agent behavior, content rules, and operational context. Here's the voice override section:

```markdown
### Voice overrides for Claude

Claude defaults formal and thorough. Jules is NOT that. Override these defaults:

- **Be casual.** Contractions. Drop formality. Talk like a person, not a white paper.
- **Be brief.** Resist the urge to over-explain. Say less.
- **Don't hedge.** "I think maybe we could consider..." → "Do X." Direct.
```

The persona is detailed enough that it changes how Claude handles everything from debugging to content feedback. Warm, direct, mischievous, no corporate-speak.

**Sub-workspace** (`Code/CLAUDE.md`) has the project inventory with stacks and statuses. `Documents/CLAUDE.md` has folder structure and naming conventions.

**Project-level** CLAUDE.md files live in each project with context specific to that codebase. My web app, my website, utility projects. Each gets stack info, deployment patterns, and domain-specific gotchas.

The hierarchy means you never paste context repeatedly. The web app CLAUDE.md only loads when you're working in that project folder. The document conventions only apply in the documents tree.

---

## 2. Skills (29)

Skills are invoked commands. Claude activates them when you ask, or you invoke them with `/skill-name`. Each one is a folder with a SKILL.md (description + instructions) and sometimes supporting reference files.

| Skill | What it does |
|---|---|
| `agent-browser` | Browser automation via Playwright |
| `brainstorming` | Structured pre-implementation exploration |
| `check-updates` | Display the latest Claude Code change monitor report |
| `content-marketing` | Read-only content tasks: backlog display, Reddit monitoring, calendar review |
| `content-marketing-draft` | Creative writing tasks: draft articles in my voice |
| `copy-for` | Format text for a target platform and copy to clipboard |
| `docx` | Create, read, edit Word documents |
| `engage` | Scan Reddit/LinkedIn/X for engagement opportunities |
| `executing-plans` | Follow a plan file step by step with review checkpoints |
| `generate-image-openai` | Generate images via OpenAI's GPT image models |
| `good-morning` | Present the daily operational briefing |
| `pdf` | PDF operations: read, merge, split, rotate, extract text |
| `pptx` | PowerPoint operations |
| `quiz-smoke-test` | Smoke tests for a custom web app |
| `retro-deep` | Full end-of-session forensic retrospective |
| `retro-quick` | Quick mid-session retrospective |
| `review-plan` | Pre-mortem review for plans and architecture decisions |
| `subagent-driven-development` | Fresh subagent per task with two-stage review |
| `systematic-debugging` | Structured approach to diagnosing hard bugs |
| `wrap-up` | End-of-session checklist: git commit, memory updates, self-improvement loop |
| `writing-plans` | Creates a structured plan file before multi-step implementation |
| `xlsx` | Spreadsheet operations |

The split between `content-marketing` (Haiku) and `content-marketing-draft` (Sonnet) is intentional. Displaying a backlog costs $0.001. Drafting a 1500-word article in someone's specific voice costs more and deserves a better model.

---

## 3. Agents (5)

Agents are specialized subagents with their own system prompts, tool access, and sometimes model assignments.

| Agent | Model | What it does |
|---|---|---|
| `content-marketing` | Haiku | Read/research content tasks |
| `content-marketing-draft` | Sonnet | Creative content work, drafting, voice checking |
| `codex-review` | Opus | External code review via OpenAI Codex |
| `quiz-app-tester` | Sonnet | Runs the right subset of tests based on what changed |
| `security-reviewer` | Opus | Reviews code changes for vulnerabilities |

The security reviewer exists because the web app handles personal data. That gets a dedicated review pass.

---

## 4. Rules Files (22)

Rules are always-on context files that load for every session. They're for domain knowledge Claude would otherwise get wrong or need to look up repeatedly.

| Rule | Domain |
|---|---|
| `1password.md` | How to pull secrets from 1Password CLI |
| `bash-prohibited-commands.md` | Documents what the bash-safety-guard hook blocks |
| `browser-testing.md` | Agent-browser installation fix, testing patterns |
| `claude-cli-scripting.md` | Running `claude -p` from shell scripts |
| `context-handoff.md` | Protocol for saving state when context window gets heavy |
| `dotfiles.md` | Config architecture, multi-machine support |
| `editing-claude-config.md` | How to modify hooks, agents, skills safely |
| `mcp-servers.md` | MCP server paths and discovery conventions |
| `proactive-research.md` | Decision tree for when to research vs. when to ask |
| `siteground.md` | SSH patterns and WP-CLI usage |
| `skills.md` | Skill file conventions and testing checklist |
| `token-efficiency.md` | Context window hygiene, model selection guidance |
| `wordpress-elementor.md` | Elementor stores content in `_elementor_data` postmeta, not `post_content` |

The Elementor rule exists because I got burned. Spent two hours "updating" a page that never changed because Elementor completely ignores `post_content`. Now that knowledge is always in context.

---

## 5. Hooks (8)

Hooks are shell scripts that fire on specific Claude Code events. They're the guardrails and automation layer. Here's the core of my bash safety guard:

```bash
PATTERNS=(
  '(^|[;&|])\s*rm\b'                    # rm in command position
  '\bfind\b.*(-delete|-exec\s+rm)'       # find -delete or find -exec rm
  '^\s*>\s*/|;\s*>\s*/|\|\s*>\s*/'       # file truncation via redirect
  '\bsudo\b|\bdoas\b'                    # privilege escalation
  '\b(mkfs|dd\b.*of=|fdisk|parted|diskutil\s+erase)'  # disk ops
  '(curl|wget|fetch)\s.*\|\s*(bash|sh|zsh|source)'    # pipe-to-shell
  '(curl|wget)\s.*(-d\s*@|-F\s.*=@|--upload-file)'    # upload local files
  '>\s*.*\.env\b'                        # .env overwrite
  '\bgit\b.*\bpush\b.*(-f\b|--force-with-lease)'      # force push
)
```

Each pattern has a corresponding error message. When Claude tries `rm -rf /tmp/old-stuff`, it gets: "BLOCKED: rm is not permitted. Use `mv <target> ~/.Trash/` instead."

| Hook | Event | What it does |
|---|---|---|
| `bash-safety-guard.sh` | PreToolUse: Bash | Blocks rm, sudo, pipe-to-shell, force push, and 12 other destructive patterns |
| `clipboard-validate.sh` | PreToolUse: Bash | Validates content before clipboard operations |
| `cloud-bootstrap.sh` | SessionStart | Installs missing system packages on cloud containers |
| `notify-input.sh` | Notification | macOS notification when Claude needs input |
| `pdf-to-text.sh` | PreToolUse: Read | Intercepts PDF reads and converts to text |
| `plan-review-enforcer.sh` | PostToolUse: Write/Edit | Injects mandatory review directive after writing a plan file |
| `plan-review-gate.sh` | PreToolUse: ExitPlanMode | Blocks exiting plan mode if review notes are missing |
| `pre-commit-verify.sh` | PreToolUse: Bash | Advisory reminder before git commits |

The PDF hook is probably my favorite. A 33-page PDF read as images chews through ~50,000 tokens that stay in context for every subsequent API call. The hook transparently swaps it to extracted text before Claude ever sees it. 95% smaller, no behavior change.

---

## 6. Makefile (43 targets)

The Makefile is the workspace CLI. `make help` prints everything. Grouped by domain:

**Quiz app** (12): dev, build, lint, test, test-all, db-seed, db-reset, report, report-send, validate, kill, analytics

**Claude monitor** (4): monitor-claude, monitor-claude-force, monitor-claude-report, monitor-claude-ack

**Morning briefing** (5): good-morning, good-morning-test, good-morning-weekly, morning-install, morning-uninstall

**Workspace health** (4): push-all, verify, status, setup

**Disaster recovery** (4): disaster-recovery, disaster-recovery-repos, disaster-recovery-mcp, disaster-recovery-brew

The disaster recovery stack does a full workspace restore from GitHub and 1Password: clones all repos, reinstalls MCP servers, reinstalls Homebrew packages. One command from a blank machine to fully operational.

---

## The Part That Actually Matters

The count is impressive on paper, but the reason this setup works isn't the volume. It's the layering.

The hooks enforce behavior I'd otherwise skip under deadline pressure (plan review, safety checks). The rules load domain knowledge that would take three searches every time I need it. The skills route work to the right model at the right cost. The agents isolate context so the main session doesn't become a 100K-token mess after two hours.

Nothing here is clever for its own sake. Every piece traces back to something that broke, slowed me down, or cost money.

The most unexpected thing I learned: the personality layer (Jules) changes the texture of the work in ways that are hard to quantify but easy to feel. Claude Code without a persona is a tool. Claude Code with a coherent personality is closer to a collaborator. The difference matters when you're spending 6-10 hours a day in the terminal.

---

*Full source: [github.com/jonathanmalkin/jules](https://github.com/jonathanmalkin/jules)*
