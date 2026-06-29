---
title: Owner-Operator Model — On-call, Operations, and Incentives
date: 2026-06-29
source: https://www.scarletink.com/the-5-impacts-of-the-owner-operator-model-regarding-on-call-operations-and-incentives/
tags: [on-call, operations, ownership, engineering, amazon]
---

# Owner-Operator Model — On-call, Operations, and Incentives

## Summary
Dave Anderson (Scarlet Ink, ex-Amazon Tech Director/GM) contrasts two models for software
support: the **owner-operator** ("end-to-end ownership") model used by Amazon, where the team
that builds software also runs it, versus the dedicated **operations team** model used by many
large tech companies. His thesis: owning software end-to-end — at the cost of occasional
annoying operational work — creates natural pressure to build and maintain software the right
way. The article promises 5 impacts; only the first is visible (the rest are paywalled).

## Decisions & insights
- **Two support models.** Owner-operator (you build it, you run it: hardware, alarms, health
  checks, escalation thresholds, customer comms) vs. a separate operations team that takes over
  in production after handoff. Neither is objectively better, but it shapes daily work life —
  worth knowing before joining a team.
- **On-call defined.** In owner-operator teams, support is a rotating role (the "on-call"). At
  Amazon, typically 24/7 for one week every 7–10 weeks (varies by team). The on-call triages
  emergencies, applies short-term patches to remove customer impact, and absorbs random
  interruptions so the rest of the team keeps building. Between fires: maintenance like rotating
  security keys, upgrading prod dependencies, updating alarms to new SLAs.
- **Impact #1 — Natural consequences and incentives (author's favorite).** Most operational
  events aren't random — they follow code changes. Evidence: during Q4 peak code freezes (e.g.
  Black Friday), Amazon-wide operational events dropped by an *order of magnitude* (orgs with
  40–50 major events/week fell to 2–3).
- **Consequences land on whoever wrote the code.** Bad code usually comes from rushing,
  laziness, inexperience, or misjudging scale — not incompetence. When the author is the one
  paged at 3am, quality pressure is intrinsic; a "stop writing bad software" lecture can't
  compete with the fear of waking a teammate. (Anecdote: junior calls an edge case one that
  "will never happen"; senior replies that at scale edge cases *always* happen — and it'll page
  someone at 3am, so fix it.)
- **Counter-example (operations-team model).** An AWS customer ran for *years* with a memory
  leak their engineers never knew about, because the ops team had quietly automated 3-hour
  instance restarts to mask it. It "worked" but inflated hosting costs — the pain of operating
  never fed back to the people who could fix the root cause.

## Open question
Where do my teams sit on the owner-operator ↔ ops-team spectrum, and does the feedback loop
from operating actually reach the people writing the code?

## References
- [Original article (partial — paywalled)](https://www.scarletink.com/the-5-impacts-of-the-owner-operator-model-regarding-on-call-operations-and-incentives/)
- Author: Dave Anderson, Scarlet Ink — ex-Amazon GM/Tech Director
- Related: [[perception-at-work]] (high-ops vs. low-ops team perception)
