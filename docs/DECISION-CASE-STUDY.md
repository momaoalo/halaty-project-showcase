# Decision Case Study — Progressive Disclosure

This case study shows one of the recurring product decisions in **حالتي**: how to keep a broad health product understandable without removing useful detail.

## Decision

> **Keep the daily surface simple and place depth behind drill-down.**

## Problem

Health products can accumulate useful information quickly:

- daily scores;
- contributing metrics;
- trends;
- methodology;
- historical context;
- recommendations;
- confidence and missing-data states.

Putting all of that on the first screen creates a different problem: the user has access to more information but has a harder time knowing what matters now.

## Options considered

| Option | Advantage | Cost / risk |
| --- | --- | --- |
| **A. Put most information on the first screen** | Maximum immediate visibility | Dense, difficult to scan, weak hierarchy |
| **B. Remove advanced detail** | Very simple surface | Power users lose useful context and trust |
| **C. Progressive disclosure** | Simple first view with depth available on demand | Requires stronger navigation and consistent drill-down structure |

## Decision criteria

The preferred approach needed to satisfy four goals:

1. **Fast daily comprehension** — the user can understand the main state quickly.
2. **Explainability** — important scores are not black boxes.
3. **Depth when needed** — detailed users can explore measurements and history.
4. **Consistency** — sleep, recovery, strain, nutrition, and training should follow a recognizable information hierarchy.

Option C provided the best balance.

## Resulting information hierarchy

```text
Level 1 — How am I?
Headline state + most relevant action

        ↓

Level 2 — Why?
Drivers + measurements + context

        ↓

Level 3 — What changed?
History + trends + deeper analysis / methodology
```

## Example application

A sleep experience can contain duration, sleep need, consistency, stages, interruptions, trends, and methodology.

Instead of placing every metric on the first screen:

- the first view communicates the sleep state and the most useful next step;
- the next layer explains the key drivers;
- deeper views expose stages, history, trends, and detailed context.

The same principle is then reused in recovery and strain so the user does not have to learn a different hierarchy for every health domain.

## Trade-off

Progressive disclosure does not reduce product complexity internally. It moves the complexity to a structure where the user asks for it.

That creates additional design work:

- navigation needs to be predictable;
- summaries and drill-down pages must not contradict each other;
- the same metric should keep consistent terminology and units;
- important warnings or missing-data states cannot be hidden simply because they are detailed.

The trade-off was accepted because it produces a clearer everyday experience without removing transparency.

## How the decision was reviewed

The decision is evaluated against real screens using questions such as:

- Can the main state be understood in a few seconds?
- Is the first screen answering one clear question?
- Is important information duplicated unnecessarily?
- Can a user understand why a score changed?
- Is deeper context still reachable without excessive navigation?
- Does the same hierarchy work across other product areas?

## What this case demonstrates

This decision demonstrates:

- problem framing;
- alternatives analysis;
- explicit decision criteria;
- trade-off evaluation;
- information architecture;
- consistency across multiple workstreams;
- iterative review of the implemented result.

It is a representative example of how product decisions in حالتي are approached: not only by asking **“Can this feature be built?”**, but also **“What is the clearest way for the user to understand it?”**