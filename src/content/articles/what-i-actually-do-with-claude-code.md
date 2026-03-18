---
title: "What I Actually Do With Claude Code (Real Examples, Not Theory)"
date: 2026-03-22
description: "A walkthrough of how one solo founder uses Claude Code for everything: morning briefings, voice dictation, software development, business decisions, content pipeline, analytics, and self-improvement loops."
story: 1
tags: ["claude-code", "jules", "workflow", "solo-founder", "real-examples"]
draft: false
---

Someone asked on my last post: "But what do you actually *do* with it? Walk me through a real example."

Fair. The previous article covered what's in the box. This one covers what happens when I open it.

I run a small business. Solo founder, one live web app, content pipeline, legal overhead, tax filing, insurance research. Claude Code handles all of it. Not "assists with." Handles. I talk, review the important stuff, and approve what matters. Here's what that actually looks like.

---

## The Setup in One Number

20 skills, 17 rules, 12 hooks, 5 agents. That's the v2 configuration. Skills are prompt-driven workflows (think: runbooks the LLM executes). Rules are persistent constraints loaded into every session. Hooks are automated triggers that fire on specific events. Agents are specialized sub-processes for parallel work.

None of those numbers are the point. The point is that they add up to something that feels less like a tool and more like a colleague who remembers everything.

---

## Morning Operations

Every day starts the same way. I type `good morning`.

The `/good-morning` skill kicks off a 990-line orchestrator script that pulls from 5 data sources: Google Calendar (service account), live app analytics, Reddit and X engagement links, an AI reading feed (Substack + Simon Willison), and YouTube transcripts. It reads my live status doc (Terrain.md), yesterday's session report, and memory files. Synthesizes everything into a briefing.

What that actually looks like:

> *3 items in Now: deploy the survey changes, write the hooks article, respond to Reddit engagement. Decision queue has 1 item: whether to add email capture to the quiz. Yesterday you committed the analytics dashboard fix but didn't deploy. Quiz pulse: 243 starts, 186 completions, 76.6% completion rate. No calendar conflicts today.*

Takes about 30 seconds. I skim it, react out loud, and we're moving.

The briefing also flags stale items: drafts sitting for 7+ days, memory sections older than 90 days, missed wrap-ups. It's not just "what's on the plate." It's "what's slipping through the cracks."

That framing matters. A morning brief that only tells me today's to-do list isn't very useful. One that also tells me what I've been ignoring is how I stay honest.

---

## Voice Dictation to Action

I use Wispr Flow (voice-to-text) for most input. That means my instructions look like this:

> *"OK let's deploy the survey changes first, actually wait, let me look at that Reddit thing, I had a comment on the hooks post, let's do that and then deploy, also I want to change the survey question about experience level because the drop-off data showed people bail there"*

That's three requests, one contradiction, and a mid-thought direction change. The intent-extraction rule parses it:

> *"Hearing three things: (1) Reply to Reddit comment, (2) deploy survey changes, (3) revise the experience-level question based on drop-off data. In that order. That right?"*

I say "yeah" and each task routes to the right depth automatically. Quick lookup, advisory dialogue, or full implementation pipeline. No manual mode-switching.

Later statements win. When dictation contradicts itself, the more recent statement takes precedence. That single rule makes voice input actually usable.

---

## Building Software

The live product is a web app: React + TypeScript frontend, PHP + MySQL backend. Here's what "building software" actually looks like in practice.

**Email conversion optimization.** Built a blur/reveal gating system on the results page with a sticky floating CTA. Wrote 30 new tests (993 total passing). Then ran 7 sub-agent persona reviews: a newbie user, experienced user, CRO specialist, privacy advocate, accessibility reviewer, mobile QA, and mobile UX. Each came back with specific findings. Deployed to staging, smoke tested, pushed to production with a 7-day monitoring baseline (4.6% conversion, targeting 10-15%, rollback trigger at less than 3%).

**Security audit remediation.** After requesting a full codebase audit, 14 fixes deployed in one session: CSRF flipped to opt-out (was off by default), CORS error responses stopped leaking the allowlist, plaintext admin password fallback removed, 6 runtime introspection queries deleted, 458 lines of dead auth code removed, admin routes locked out on staging and production. 85 insertions, 2,748 deletions across 18 files.

**Survey interstitial.** Built and deployed 3 post-quiz questions. 573 responses in the first few days, 85% completion rate. Then analyzed the responses: 45% first-year explorers, "figuring out where to start" at 43%, one archetype converting at 2x the average.

**Explorer archetype scoring fix.** The "Curious Cat" archetype was triggering for around 75% of users. Replaced a dual catch-all scoring logic with a spread-based check (`max - min < 35`). Tuned it down to 32%. One rule change, verified with analytics.

**Content migration.** When evaluating a domain change, the research included a full SEO impact analysis, a 12-month 301 redirect traffic model, and discovery that ChatGPT was recommending the app as one of 5 in its category. That last finding was hiding in "direct" traffic. The analysis surfaced it.

The deployment flow for each of these is the same: local validation (lint, build, tests), GitHub Actions CI, staging deploy, automated smoke test via Playwright through an agent-browser (mobile viewport included), my approval, production deploy, analytics pull 10 minutes later to verify the change landed.

That last step is non-negotiable. I don't report success without confirming it.

---

## Making Decisions

This is honestly where I spend the most time. Not code. Decisions.

**Advisory mode.** When I say "should I..." or "help me think about...", the `/advisory` skill activates. Socratic dialogue with 18 mental models organized in 5 categories. It challenges assumptions, runs pre-mortems, steelmans the opposite position, scans for cognitive biases (anchoring, sunk cost, status quo, loss aversion, confirmation bias). Then logs the decision with full rationale.

Real example: I spent three days stress-testing a business direction decision. Feb 28 brainstorming, Mar 1 initial decision, Mar 2 adversarial stress test, Mar 3 finalization. Jules facilitated each round. The advisory retrospective afterward evaluated around 25 decisions over 12 days across 8 lenses and flagged 3 tensions I'd missed.

In practice, advisory mode works like having a business partner who's done the research, doesn't have a stake in any particular outcome, and won't let you rationalize your way past a bad decision. The adversarial stress test is the most useful part. You present your decision. It argues against it as hard as it can. Then you respond to those arguments. If you can't answer them, you've found a problem before you've committed.

**Decision cards.** For quick decisions that don't need a full dialogue:

> **[DECISION]** Add email capture to quiz results | **Rec:** Yes, tests privacy assumption with real data | **Risk:** May reduce completion rate if placed before results | **Reversible?** Yes -> Approve / Reject / Discuss

These queue up in my status doc and I batch-process them when I'm ready. Usually at the start of a session, the morning brief surfaces what's pending.

**Builder's trap check.** Before every implementation task, the system classifies it: is this CUSTOMER-SIGNAL (generates data from outside) or INFRASTRUCTURE (internal tooling)? If I've done 3+ infrastructure tasks in a row without touching customer-signal items, it flags the pattern. One escalation, no nagging.

This matters because infrastructure work is genuinely fun. Writing scripts, building automations, tuning configurations. All of it produces flow state. None of it produces customer data. The classification doesn't stop me from doing infrastructure work. It just makes sure I'm choosing it, not drifting into it.

---

## What a Typical Day Actually Looks Like

Here's how the pieces fit together in practice.

**7 AM.** The container has already run the morning orchestrator at 5 AM. The brief is waiting. I type `good morning` and get the synthesized version: what's urgent, what's stale, what's in the decision queue, what the analytics show. I react out loud, and that reaction becomes the day's direction.

**Morning work block.** Usually 2-3 hours of focused work. Might be software development, might be content writing, might be business research. The morning brief determines which. Voice dictation handles most input. Task routing is automatic.

**Mid-session.** If I've been heads-down for a while, the engagement scan surfaces Reddit and X activity worth responding to. Content seeds get saved for later. If I've been doing infrastructure work for too long, the builder's trap check fires.

**Afternoon work block.** The afternoon orchestrator (4 PM) runs a lighter version of the morning brief: what moved, what's pending, anything new that surfaced. I pick up from there or wrap if the session is done.

**Session end.** `/wrap-up` commits code, updates memory, updates the status doc, and runs a retro scan. What happened, what moved, what's queued for tomorrow. Takes about 5 minutes. The report feeds tomorrow's morning brief.

A typical day touches 4-5 of the categories in this article. Monday I might deploy a feature, analyze survey data, draft a LinkedIn post, and prep for a legal consultation. All in one session. No context-switching between tools. The same conversation, continuous context.

---

## Content Pipeline

Not just "write a post." The full pipeline:

**Draft.** The content-marketing-draft agent (runs on Sonnet for voice fidelity) writes against a 950-word voice profile mined from my published posts. Specific patterns: short sentences for rhythm, self-deprecating honesty as setup, "works, but..." concession structure, insider knowledge drops.

**Voice check.** Anti-pattern scan: no em-dashes, no AI preamble ("In today's rapidly evolving..."), no hedge words, no lecture mode. If the draft uses comma-heavy asides or feature-bloat paragraphs, it gets flagged.

**Platform adaptation.** Each platform gets its own version. Reddit: long-form, code examples, technical depth. LinkedIn: punchy fragments, professional angle, links in comments not body. X: 280 characters, 1-2 hashtags.

**Post.** The `/post-article` skill handles cross-platform posting via browser automation. Updates tracking docs, moves files from Approved to Published.

**Engage.** The `/engage` skill scans Reddit, LinkedIn, and X for conversations about topics I've written about. Scores opportunities, drafts reply angles.

The voice profile is what makes this work. Without it, AI-assisted writing sounds like AI-assisted writing. The profile encodes specific stylistic patterns from authenticated work. The voice check catches drift.

---

## Business Operations

This is the part most people don't expect from a CLI tool.

**Legal.** Organized case documents, extracted text from PDFs (a hook converts 50K tokens of PDF images into 2K tokens of text automatically), researched state laws affecting the business, prepared consultation briefs with specific questions and context, analyzed risk across multiple legal strategies.

**Tax.** Compared 4 CPA options against specific criteria (crypto complexity, LLC structure, investment income). Organized uploaded documents. Tracked deadlines.

**Insurance.** Researched carrier options after one rejected the business. Compared coverage types, estimated premium ranges for the new business model, identified specific policy exclusions to negotiate on. Prepared questions for the broker.

**Brand and domain research.** When considering a domain change, the research covered SEO and GEO implications, modeled traffic impact over 12 months, and discovered that ChatGPT was recommending the app as one of 5 in its category. That last finding would have been easy to miss. It wasn't.

None of this is code. It's research, synthesis, document management, and decision support. The same terminal, the same personality, the same workflow.

Most coding-focused AI tools treat everything outside code as out of scope. The reality of running a small business is that code is maybe 30% of the actual work. The rest is decisions, research, communications, and operations. Having one tool that handles the whole surface is the actual advantage.

---

## Data and Analytics

**Local analytics replica.** 125K rows synced from the production database into a local SQLCipher encrypted copy in 11 seconds. Python query library with methods for funnel analysis, archetype distribution, traffic sources, daily summaries. Ad-hoc SQL via `make quiz-analytics-query SQL="..."`.

**Traffic forensics.** Investigated a traffic spike: traced 46% to a 9-month-old Reddit post in r/SampleSize, discovered ChatGPT referrals were hiding in "direct" traffic (45%). One Reddit post was responsible for 551 sessions.

**Survey analysis.** 573 responses from a 3-question post-quiz survey. Cross-tabulated motivation vs. experience level vs. biggest challenge. Found that one archetype name was attracting the wrong audience — the name sounded casual but the archetype was for deep exploration.

Analytics without a query layer is just data you can't act on. The local replica plus query library means I can go from question to answer in one terminal command.

---

## Self-Improvement Loop

This is the part that compounds.

**Session wrap-up.** Every session ends with `/wrap-up`: commit code, update memory, update status docs, run a retro scan. The retro checks for repeated issues, compliance failures, and patterns. If it finds something mechanical being handled with prose instructions, it flags it: "This should be a script, not more guidance."

**Deep retrospective.** Periodically run `/retro-deep` for forensic analysis of an entire session. Every issue, compliance gap, workaround. Saves a report, auto-applies fixes.

**Memory management.** Patterns confirmed across multiple sessions get saved. Patterns that turn out wrong get removed. The memory file stays under 200 lines. Concise, not comprehensive.

**Rules from pain.** Every rule in the system traces back to something that broke. The plan-execution pre-check exists because I re-applied a plan that was already committed. The bash safety guard exists because Claude tried to delete something it shouldn't have. The PDF hook exists because a 33-page PDF ate 50K tokens. Pain, then rule, then never again.

The self-improvement loop is what makes the system compound over time. Each session leaves the setup slightly better than it found it. After six months of daily production use, the v1 configuration of 35 skills and 24 rules had trimmed itself down to 20 skills and 17 rules. Not because things were removed arbitrarily. Because the unused ones revealed themselves.

---

## The Thing That's Hard to Convey in a Feature List

All of this happens in one terminal, in one conversation, with one personality that has context on everything.

There's no context-switching between "coding tool" and "business advisor" and "content writer." One conversation. Jules knows the codebase, the business context, the content voice, the pending decisions, and yesterday's session.

The 20 skills and 17 rules and 12 hooks aren't things I interact with individually. They're the substrate that makes it feel like working with a really competent colleague who never forgets anything. The morning briefing tells me what needs attention, voice dictation routes work to the right depth, and wrap-up captures what happened so tomorrow's briefing is accurate.

That's what I actually do with it.

---

*Full source: [github.com/jonathanmalkin/jules](https://github.com/jonathanmalkin/jules)*
