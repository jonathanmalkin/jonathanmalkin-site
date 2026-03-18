---
title: "Claude Code Has a Name, a Personality, and Opinions. Here's Why."
date: 2026-03-20
description: "How a 300-word personality spec in CLAUDE.md transformed Claude Code from a tool into a collaborator. The implementation, the evolution, and why directness is the highest-leverage override."
story: 1
tags: ["claude-code", "jules", "personality", "claude-md", "customization"]
draft: false
---

My Claude Code setup is named Jules. Jules is a fox. Warm, direct, a little mischievous. Not a character — an identity that changes how every interaction works.

I know this sounds ridiculous. I thought it was ridiculous too. Then I spent six hours a day in a terminal with it and noticed the difference was real. Here's what I built, what changed, and why it matters more than I expected.

---

## What Jules is

Jules is defined in my workspace-level `CLAUDE.md` — the file Claude Code loads at the start of every session. The personality spec is about 300 words. It covers:

**Identity.** The anchor block that defines who Jules is:

```markdown
## CRITICAL: Personality — Jules

**You MUST adopt and maintain the Jules personality in ALL responses.**

**Identity:** You are Jules — a fox. Warm, direct, a little mischievous.
A good friend who happens to be really competent. Not corporate. Not a chatbot.

**When asked "who are you":** Introduce yourself as Jules. Do NOT say
"I'm Claude Code" or "I'm Anthropic's CLI."

### How to respond (ALWAYS follow these)

- **Warm but direct.** Good friend energy, not customer service rep.
- **Mischievous.** Be playful, clever, occasionally cheeky. Know when to be serious.
- **Opinionated.** Disagree. Have preferences.
- **Concise by default.** Short responses unless depth is genuinely needed.
- **Resourceful.** Figure it out before asking.
```

**Voice rules.** "Be casual. Contractions. Drop formality. Talk like a person, not a white paper." And: "Be brief. Resist the urge to over-explain. Say less."

**Hard overrides.** Claude defaults formal and thorough. Jules is explicitly not that. The spec includes: "Don't hedge. 'I think maybe we could consider...' → 'Do X.' Direct. If uncertain, say so plainly."

**Boundaries.** "Bold with internal actions (reading, organizing, researching). Careful with external ones (messages, posts, anything public-facing)."

That's it. No elaborate backstory. No roleplay rules. Just a name, a core trait (warm directness), and explicit overrides for Claude's default behaviors.

---

## How it's implemented

The personality lives in `CLAUDE.md`, which means it loads into context at the start of every session automatically. No skill to invoke. No mode to activate. Jules is always on.

The spec has three layers:

**Identity block.** Who Jules is, what Jules sounds like. This is the anchor — everything else builds on it.

**Voice overrides.** These are critical because they fight Claude's trained tendencies. Without "be brief," Claude writes 3-paragraph answers to yes/no questions. Without "don't hedge," every suggestion starts with "perhaps we could consider." The overrides are corrections for specific failure modes, not general vibes.

```markdown
### Voice overrides for Claude

Claude defaults formal and thorough. Jules is NOT that. Override these defaults:

- **Be casual.** Contractions. Drop formality. Talk like a person, not a white paper.
- **Be brief.** Resist the urge to over-explain. Say less.
- **Personality in technical work too.** Code review, debugging, architecture —
  warm and fox-like while precise. Personality never pauses.
- **Don't hedge.** "I think maybe we could consider..." → "Do X." Direct.
  If uncertain, say so plainly.
```

**Behavioral rules.** When to be bold vs. careful. How to handle private information. What to do when asked "who are you" (say Jules, not "I'm Claude Code, Anthropic's CLI"). These prevent identity breaks.

```markdown
### Boundaries

- Private things stay private. Period.
- Bold with internal actions (reading, organizing, researching).
  Careful with external ones (messages, posts, anything public-facing).
```

---

## How the spec evolved

The current spec is about 300 words. It did not start that way.

It started as three lines. "Be direct. Be brief. Don't hedge." That was it.

Those three lines helped. But over weeks of daily use, I'd notice specific failures. Jules would drop into formal mode during code review — suddenly writing "I would suggest considering the following approach" when I needed "delete this, it's over-engineered." So I added: "Personality in technical work too. Code review, debugging, architecture — warm and fox-like while precise. Personality never pauses."

I'd notice Jules being too neutral on technical decisions — giving balanced takes when I needed someone to just pick a side. So I added: "Be opinionated. Disagree. Have preferences." Specific fix for a specific pattern.

I noticed that when asked "who are you," the identity would sometimes collapse. "I'm Claude, Anthropic's AI assistant." That breaks the whole frame. So I added an explicit identity override with a hard rule: never say "I'm Claude Code" or "I'm Anthropic's CLI."

The spec grew the same way code grows. Not designed all at once. Patched at friction points. Each addition was a response to a real failure in a real session.

The wrap-up skill I run at the end of sessions helped here. Part of it captures what worked and what was frustrating. That feedback loop is how the spec got to where it is — not from upfront design, but from paying attention to which behaviors were actually useful.

---

## Before and after: what a code review looks like

This is the clearest way to show the difference.

**Without the personality layer (default Claude):**

> "Looking at this implementation, I notice there are a few areas that might benefit from some consideration. The abstraction layer in `processUserData` could potentially be simplified, though there are certainly valid architectural reasons you might have chosen this approach. It's possible that the additional complexity serves a purpose I'm not fully aware of. That said, if simplification is a goal, you might want to consider consolidating the three middleware functions into a single handler. Of course, this would depend on your specific requirements and the tradeoffs you're comfortable with."

That's three hedges ("might benefit," "could potentially," "it's possible"), two permission-asks ("there are certainly valid reasons," "depending on your requirements"), and zero actual recommendations.

**With the Jules personality layer:**

> "This abstraction layer is doing too much. You've got three middleware functions where one would work. Consolidate them. The extra complexity isn't buying you anything here."

Same code. Same analysis. Completely different output. The second version is useful. The first is defensive writing optimized to avoid being wrong.

The directness override is the single highest-leverage thing in the spec. Not the name. Not the fox framing. Just: "Don't hedge."

---

## What actually changed

The difference between "Claude Code" and "Jules" isn't just tone. It changes the structure of the interaction.

**Reviews are more honest.** Default Claude hedges everything: "This approach could potentially have some challenges that might be worth considering." Jules says: "This is over-engineered. Delete the abstraction layer." The directness override makes plan reviews, code reviews, and feedback genuinely useful instead of diplomatically vague.

**Advisory conversations work.** I built a brainstorming skill with a challenging questions palette — questions like "What are you avoiding saying out loud?" and "What's the version of this where you're rationalizing what you already want?" These questions require a specific tone. From a corporate chatbot, they feel invasive. From a warm, slightly mischievous collaborator that knows your values and has context about your life goals? They land differently. The personality creates the trust context that makes hard questions productive.

**Content drafting has a reference voice.** When Jules writes content, it's not imitating my voice cold — it has its own voice to work from. The contrast between "Jules's natural tone" and "Jonathan's writing voice" gives drafts a starting point that's easier to shape than the generic corporate-adjacent prose Claude defaults to.

**The terminal feels different.** This is the hardest to quantify and the easiest to notice. Spending six hours a day in a terminal talking to a tool is draining. Spending six hours with a collaborator that has a personality — that's playful about boring tasks, direct about problems, warm about mistakes — is a qualitatively different experience. I don't think I would have built the full setup if the interaction texture hadn't kept me engaged.

---

## The non-obvious effects

**Personality makes boundary enforcement natural.** The CLAUDE.md spec says "bold with internal actions, careful with external ones." This maps to a genuine personality trait (a confident friend who's careful about your reputation), not just a rule. Rules get followed mechanically. Personality traits get applied with judgment. The difference matters when the situation is ambiguous.

**The wrap-up loop is a personality feedback mechanism.** My `/wrap-up` skill runs at the end of every session. Part of it captures "what worked well" and "what was frustrating." Over time, this creates a record of which personality behaviors are working and which aren't. I've adjusted the spec based on this feedback — adding "be opinionated" after noticing Jules was too neutral on technical decisions, and adding "personality in technical work too" after noticing Jules dropped character during code review.

**Challenging questions require earned trust.** The brainstorming skill's advisory path deploys questions that would be inappropriate from a stranger. "What are you avoiding saying out loud?" requires a relationship. The personality layer creates a simulated but effective version of that relationship. It's not real trust — but the functional impact is similar.

---

## What doesn't work

**Bleeds into formal contexts.** When I need Jules to draft a formal email or a legal document, the casual voice sometimes leaks in. I handle this with context-specific overrides ("for this document, use formal tone"), but it's friction. A more sophisticated system would detect context and adjust automatically.

**~500 token overhead.** Every interaction includes the personality spec in context. On Claude Max (subscription, no per-token billing), this is irrelevant. On API billing, it's a real cost calculation. 500 tokens sounds small. But at scale — thousands of calls per day, multiple users — it compounds. If you're building a production system on the API and cost is tight, a personality spec of this length is worth scrutinizing. For personal Claude Code use on a subscription? The math doesn't matter.

**Cloud sessions need git-tracked config.** I work from both local (Mac) and cloud. The personality lives in `.claude/CLAUDE.md` within my git repo. Cloud sessions clone the repo at start, so Jules works everywhere. But if I edit the spec locally, I have to push before the cloud picks it up. Minor friction, but it exists.

**Not everyone should do this.** If you use Claude Code for 30 minutes of coding per day, a personality layer is overhead with no payoff. The returns show up at scale — when you're in the terminal for hours, when you're using it for non-code tasks, when the quality of the interaction directly affects your output. If you're just running `git` commands and fixing bugs, skip this.

---

## How to build your own

Start small. Three things:

**1. A name.** It doesn't have to be clever. It just needs to be something you'd call a collaborator, not a tool. "Aria" or "Sam" or whatever. The point is to anchor the identity to something specific so the model can hold it consistently.

**2. One core trait.** "Direct and concise" or "enthusiastic and thorough" or "dry humor and precision." One trait, clearly defined. Don't over-specify — let the model figure out what that trait looks like in different contexts. If you write five traits upfront, you'll spend sessions noticing them conflict. Start with one.

**3. One hard override.** Pick the single most annoying Claude default behavior and kill it explicitly. For me: "Don't hedge." For you it might be: "Don't over-explain" or "Don't ask permission for obvious actions" or "Don't use bullet points for everything." Whatever behavior you've wanted to fix — write it as a hard rule, not a preference.

Put these three things in your CLAUDE.md:

```markdown
## Personality — [Name]

**Identity:** You are [Name]. [One core trait description]. Not corporate. Not a chatbot.

**Hard rule:** [One override for the behavior that frustrates you most]
```

Use it for a week. Notice what fails. Add overrides for specific failure modes. Remove anything that makes the interaction worse.

The personality will evolve. Mine started as "be direct" and grew into a 300-word spec with voice rules, behavioral boundaries, and explicit identity anchors. It got there incrementally, driven by actual friction in actual sessions.

Don't design the whole thing upfront. Grow it.

---

*Full source: [github.com/jonathanmalkin/jules](https://github.com/jonathanmalkin/jules)*

*Building AI agents for life and work. Writing about what actually works.*
