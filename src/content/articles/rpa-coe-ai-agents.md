---
title: "The RPA COE Pattern Is Repeating in AI Agents"
date: 2026-04-24
description: "The Center of Excellence arc from RPA is repeating in AI agents: point solutions, organic spread, centralized governance, then federated execution. If you lived through RPA, you already have the map."
story: 3
tags: ["ai-agents", "rpa", "automation", "coe", "governance"]
platforms: {}
image: "/articles/rpa-coe-ai-agents/rpa-coe-maturity-arc.png"
draft: false
---

The Center of Excellence arc from RPA is repeating in AI agents: point solutions, organic spread, centralized governance, then federated execution. If you lived through RPA, you already have the map — only the unit of automation changed.

I watched this sequence play out dozens of times.

A Director of Finance would pull us in to automate an invoicing process. It would go well. A few months later, he'd come back with two more. Then he'd mention it to HR. HR had a use case. IT heard about it. Now IT had three. Within a year you had multiple departments running automations, none of them talking to each other, nobody sure who owned what.

So the company centralized. They stood up a Center of Excellence. IT took ownership. Governance, standards, a shared services model. Requests went through a queue. Priorities got set. Quality improved.

Then the COE became the bottleneck. Business units knew their own processes better than IT did. The model shifted again: a central COE for oversight, standards, and infrastructure — with development pushed back out to the teams that actually had the work. Federated. Distributed. Governed but not gatekept.

I saw that arc repeat at company after company during my years at Automation Anywhere. Different industries, different sizes, same pattern. Every time.

I didn't expect to watch it repeat in my own living room.

---

## The Four Stages — If You Were There, You Know Them

Before we get to agents, it helps to name the stages clearly. If you spent time in the RPA space, you probably lived through at least two of these.

![The four-stage maturity arc from point solution to organic spread, centralized shared services, and federated model.](/articles/rpa-coe-ai-agents/rpa-coe-maturity-arc.png)

**Stage 1: Point solution.** One team. One problem. One win. The technology proves itself.

**Stage 2: Organic spread.** Word gets around. Two departments become four. Each team is doing its own thing. No shared infrastructure, no shared standards, no visibility into what anyone else is running.

**Stage 3: Centralized shared services.** IT takes it over. A formal COE is established. Requests come in through a process. Development is centralized. Governance is real — but velocity slows. The people who know the work best are now waiting in a queue managed by people who don't.

**Stage 4: Federated model.** The COE matures into oversight. Development moves back to the business units. IT holds the guardrails. Teams hold the builds. The COE becomes infrastructure, not a gateway.

Fortune 500 companies ran this full loop over five to ten years. Mid-market companies ran it faster, messier. The pattern doesn't care about company size. It cares about how much automation you're running and how many people are involved.

---

## The Same Arc, One Person

Here's what my own journey with AI agents looked like.

It started with chat. Ask a question, get an answer. One task, one interaction. Point solution.

Then I started using it more — drafting, research, code, decisions. I was repeating the same setup over and over. Explaining context I'd already explained. Getting inconsistent outputs because the framing kept shifting. Different tools for different tasks, nothing connected.

Sound familiar?

So I formalized it. I built skills: _Think_ for working through decisions, _Write_ for drafting and editing. Named workflows with specific behaviors and consistent inputs. The organic spread had turned into a shared services layer, except I was both the business unit and the IT team.

Then I started connecting things. A research workflow feeding into a writing workflow. An orchestration layer handling the handoffs. Guidelines for how agents should behave and what they should escalate. Explicit boundaries on what runs autonomously versus what needs my review.

I had built a COE. One person. A team of specialized agents. An orchestrator managing the handoffs. Governance I'd written myself.

The arc was identical. The scale was completely different.

---

## The Pattern, Mapped

The COE arc holds across every scale of organization. What changes is the vocabulary and the headcount.

| COE Concept | Fortune 500 | Mid-Market | Solo Operator |
|---|---|---|---|
| Executive Sponsor | VP / C-Suite champion | Owner or Department Head | You |
| Center of Excellence | Formal IT COE team | IT lead + power users | Your agent orchestration layer |
| Business Unit Teams | Department-level developers | Individual power users | Your specialized agents |
| Governance & Standards | Formal policies, change control | Informal rules, one IT owner | Your system prompts and guidelines |
| Work Queue | Ticketing system | Shared inbox or spreadsheet | Your task routing |
| Quality Control | COE review gates | IT sign-off | Your review step before anything ships |
| The Federated Model | COE + distributed dev | Owner oversight + team execution | Orchestrator + specialized agents running autonomously |

The Fortune 500 version of this took a team of ten and five years to reach stage four. The solo operator version can get there in months. The leverage is the same. The overhead is almost zero.

![The COE pattern mapped from Fortune 500 organizations to solo operators using specialized AI agents.](/articles/rpa-coe-ai-agents/rpa-coe-pattern-map.png)

---

## What This Means If You Were in RPA

Most people discovering AI agents are working through this pattern for the first time. They'll do it the slow way — tool sprawl, eventual recognition that they need structure, overcorrection into rigid process, gradual federated maturity. Five years, compressed into two.

You already have the map.

The pain points you remember — shadow IT, the governance fights, the COE that couldn't keep up with demand, business units going around IT because the queue was too slow — all of it is happening again right now. In every company without a clear AI governance model. In every knowledge worker running five AI tools that don't talk to each other.

The skills that made you valuable in the RPA space translate directly. You know how to scope automation work. You know what "it'll only take a week" actually costs. You've seen what happens when you scale without governance and what happens when you govern without flexibility.

What changed is the unit of automation. RPA automated tasks. Agents automate judgment. The governance questions are harder, the failure modes are less visible, and the velocity ceiling is much higher. But the arc — from point solution to federated model — is the same one you already know.

---

## The Question Worth Asking

When the Finance Director first asked us to automate his invoicing process, he had no idea he was starting something that would eventually restructure how his entire company thought about operations.

He was just trying to save his team some time on one annoying task.

That's how all of these things start.

If you've been using AI for anything, you've already entered the arc. The question isn't whether to build this — it's what stage you're actually at, and whether you're building toward stage four or letting it stall somewhere in the organic spread.

The pattern is in motion. You've seen it before. This time you don't have to wait five years to see where it goes.
