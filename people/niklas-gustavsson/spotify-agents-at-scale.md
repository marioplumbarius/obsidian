---
title: Niklas Gustavsson — How Spotify Runs Agents Across 20M Lines of Code
date: 2026-06-30
tags: [engineering, ai-agents, spotify, fleet-management, developer-productivity, claude-code]
---

# Niklas Gustavsson — How Spotify Runs Agents Across 20M Lines of Code

## Summary

Niklas Gustavsson is a long-tenured engineering leader at Spotify who built "Honk" — Spotify's internal agentic infrastructure for automated code changes across their codebase. This note distills his key insights from a Pragmatic Engineer podcast interview. The story is about how investing in foundational engineering practices (test automation, standardization, fleet management) years before AI unlocks massive leverage when agents arrive.

Source: Pragmatic Engineer Podcast — *How Spotify runs agents across 20M lines of code*

---

## Decisions & insights

### 1. The Problem That Started Everything — Code Growing 7x Faster Than Engineers

Five to six years ago, Spotify identified that their codebase was growing **seven times faster** than the engineering headcount to support it. They were drowning in maintenance: Java version upgrades, library updates, API migrations — all done manually by hundreds of teams across thousands of components. Each migration took months and they could barely do 10 per year.

The response: build **fleet management** infrastructure to automate mutations across the entire codebase instead of asking every team to do it manually.

> *"We've merged millions and millions of those types of PRs."*

---

### 2. The Ceiling of Deterministic Scripts — Why LLMs Became Necessary

Their automated migration scripts hit a hard ceiling. Code has an enormous API surface — even a simple method swap becomes thousands of lines of edge-case handling when you account for every calling pattern (variable assignment, chaining, etc.).

> *"Each script that we had to migrate code turned into thousands of lines of taking care of every edge case."*

This is what pushed them to experiment with LLMs early — even before Claude, in the early GPT days. The initial attempts failed (models weren't good enough, they naively tried to one-shot everything), but it showed them where the industry was heading.

---

### 3. Honk — The Architecture

Honk is Spotify's internal agent infrastructure, now on V2 (really ~V8 by their own count). Architecture is intentionally simple:

- **Claude Agent SDK** running in a **Kubernetes pod**
- Access to a set of **tools** (V1: predefined allowlist; V2: users can add their own)
- Key tool: **CI builds** — can run verification on both Linux and macOS (critical for iOS development)
- Previously had an **LLM-as-judge** step; removed it once Claude 3 Opus/Sonnet got good enough

The judge was hugely important early on — it improved PR success rate from ~20-30% to ~80%. But model quality eventually made it redundant.

> *"It used to be that those tools were a predefined allow-listed set of tools that we trusted. Now in V2, users can add their own tools."*

---

### 4. Verification Is the Single Most Important Thing

For closed-loop agentic development (no human in the loop), the verification step is what makes or breaks it. The common mistake is underinvesting in how well that loop works.

Spotify's leverage here came from a pre-existing investment: because they were already auto-merging PRs via fleet management, they had **already forced teams to build strong test automation**. When agents arrived, they could reuse the same verification infrastructure.

> *"Now we can throw agents at that and use the same verification that we had in place before."*

---

### 5. Standardization Helps Agents As Much As It Helps Humans

Spotify has been driving codebase consistency for years — standardizing frameworks, tools, patterns. Originally for human productivity. It turned out to be equally (maybe more) valuable for agents.

> *"If they look in 10 different ways, Claude is going to be more confused. The more consistency we have, the better our agents work."*

Claude is good at finding patterns in the existing codebase and using them as "inspiration" for solving new problems. A consistent codebase means better signal; a fragmented one means noise.

---

### 6. The Metrics — 75% PR Frequency Improvement, 73% AI-Authored PRs

Spotify's ROI has been easy to see at a headline level:
- **75%+ improvement** in PR frequency directly attributable to AI tooling
- **~73% of all PRs** are now AI-authored

They're now trying to connect this to downstream user value — linking PRs → deployments → work items → A/B tests → user outcomes. The ROI conversation was easy early on because improvements were so large; now expectations for precision are rising as the practice matures.

---

### 7. Prototyping as a New Unlock — Including for Non-Engineers

Spotify built a simple internal infrastructure for end-to-end prototyping in their mobile apps and backend, plus an **internal app store** for sharing prototypes. Non-engineers (designers, PMs, even co-CEOs) are now building and sharing prototypes.

> *"Ideas that they always had in the back of their head... they have an entire engineering team that could build that out but that team is focused on other things. So for them to then be able to try something out more quickly..."*

This is a qualitative shift: previously motivating engineers to validate an idea took weeks. Now anyone can have a working prototype with real data in an hour or two.

---

### 8. Personal Take — What Actually Changed

Niklas came from a molecular biology PhD, has done competitive programming for fun, and was genuinely worried that AI would strip out the part of coding he loved (problem-solving). His conclusion:

> *"The thing that I like to do is solving problems and the way that I solve those problems turn out to not be the most critical piece for me."*

He now runs 5–10 Claude Code sessions in tmux in parallel, with a matrix of Claude sessions and matching terminals in git worktrees. His 20M+ line monorepo works surprisingly well — Claude is good at searching large codebases for patterns.

---

## Key takeaways

1. **Invest in test automation and standardization regardless of AI** — it pays off doubly when agents arrive
2. **The verification loop is everything** for autonomous agents — underinvesting here is the most common mistake
3. **Consistency in your codebase is a force multiplier** for agent quality
4. **Fleet management thinking** (mutate the whole codebase, not ask each team) is the right frame for large-scale agentic changes
5. **Speed and quality aren't a tradeoff** — automating quality practices is what enables speed
