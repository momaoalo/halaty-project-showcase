# Halaty (حالتي)

**Arabic-first personal health application for iOS**

Halaty brings **daily health, sleep, recovery, nutrition, training, and progress** into one connected experience. The product is built around a simple question:

> **How am I today, why, and what should I do next?**

This repository is a public product showcase. The production source code is intentionally kept private.

---

## Product at a glance

| Area | Product direction |
| --- | --- |
| Daily health | Turn wearable signals into an understandable daily state |
| Sleep & recovery | Show the score, the reasons behind it, and data-confidence context |
| Nutrition | Make Arabic-first food logging fast and practical |
| Training | Connect workout planning and logging with recovery context |
| Reports | Help users understand weekly and monthly trends |
| Body & progress | Keep physical progress connected to training and health |
| Friends & coaching | Enable controlled sharing and contextual guidance |

---

## The problem

People who actively track their health often rely on multiple separate products: a wearable app for sleep and recovery, another app for food, another for workouts, and separate tools for trends or body progress.

The result is a lot of data but no single answer to what that information means for the user's day.

**Halaty explores a more integrated model:** health signals, food, and training should contribute to one clear and understandable picture instead of behaving like unrelated apps placed under the same navigation.

---

## Who Halaty is for

The primary product direction is **Arabic-speaking users, particularly in Saudi Arabia and the Gulf**, who care about health, fitness, sleep, or nutrition and want a product designed around Arabic from the beginning rather than translated later.

The experience is designed for two usage styles:

- **Quick-view users** who want a simple summary and recommendation.
- **Detail-oriented users** who want metrics, trends, explanations, and historical context.

That tension between simplicity and depth is one of the main UX problems the product is designed around.

---

## Core product areas

### 1. Daily health, sleep & recovery

The daily experience brings together signals such as:

- Sleep
- Recovery
- Activity
- Readiness / daily state
- Physical strain / fatigue context
- Longer-term trends

The goal is not only to show a number. The user should be able to understand **why it changed**, what signals contributed to it, and whether the available data is strong enough to trust the interpretation.

### 2. Nutrition

Nutrition is treated as an important contributor to the user's overall health state rather than as a separate logging-only product.

Product concepts include:

- Arabic and English food search
- Gram-first portions and serving quantities
- Barcode-based logging
- Saved and recent meals
- Energy and macro targets
- Meal-photo identification concepts
- Daily nutrition context connected to the broader health experience

The UX direction emphasizes **logging speed, clear portions, and fewer unnecessary steps**.

### 3. Training

Training is designed as a broader physical-performance experience rather than an isolated workout logger.

Product areas include:

- Workout planning
- Exercise library
- Live session logging
- Sets, repetitions, rest, and training details
- Exercise substitutions
- Progression and deload guidance
- Imported workout context from Apple Health / Apple Watch
- Body and performance progress

A central product goal is to make recovery relevant to training decisions instead of keeping both areas disconnected.

---

## Supporting areas

### Reports & trends

Weekly and monthly views help users move beyond one-day scores and understand longer-term direction.

### Body & progress

Weight, body-composition context, measurements, and related progress views are treated as part of the user's longer-term physical story.

### Friends & controlled sharing

The social concept is intentionally different from a public feed. The direction is **permission-based sharing** of selected health or training information.

### Coaching

The product explores both AI-assisted guidance and permission-based human-coach workflows, with an emphasis on contextual guidance rather than generic responses.

### Health-data integration

The iOS experience integrates with Apple health data so that automatic health information can form the foundation of the daily experience.

---

## UX architecture

One of the product's main design rules is:

> **Depth lives behind drill-down, never on the surface.**

The experience is structured in three levels:

1. **Understand today** — the first level communicates the current state quickly.
2. **Understand why** — detail screens explain the most relevant drivers and context.
3. **Explore deeply** — trends, history, advanced detail, and longer reports live deeper in the experience.

This structure is used whenever a screen becomes too dense: useful information is moved deeper rather than competing for attention on the first screen.

---

## Product principles

- **Simple first, detailed when requested.**
- **Do not invent certainty.** Missing or insufficient data should be communicated clearly.
- **Explain, not only score.** Numbers should have understandable context.
- **Connect product areas.** Sleep, recovery, nutrition, and training should tell one story.
- **Arabic-first product design.** Arabic influences layout, terminology, search, hierarchy, and content—not just translation.
- **Privacy by design.** Health sharing should be explicit and permission-controlled.

---

## Examples of product decisions

### Information hierarchy

A recurring product decision is what belongs on the first screen versus what should live behind a detail view. This keeps the daily experience quick to scan without removing depth for advanced users.

### Reducing logging friction

Nutrition and training contain many possible inputs. The product repeatedly evaluates what should be required, what should be optional, and which interactions can be shortened.

### Missing-data behavior

Health products can appear precise even when the underlying information is incomplete. Halaty deliberately includes learning states, missing-signal states, and confidence-aware presentation rather than filling gaps with confident-looking values.

### Friend viewing versus editing

Friend experiences are treated as read-only tracking by default. Sharing permissions are separated by information category so social features do not weaken privacy.

### Connecting recovery and training

Training should not live in isolation. Recovery state is intended to influence how training information and guidance are presented.

---

## My role

My main hands-on contribution to Halaty is in **product direction, product analysis, and user experience**, including:

- Defining and refining features
- Breaking large ideas into user flows and smaller product decisions
- Reviewing screens and identifying usability problems
- Simplifying flows and removing unnecessary steps
- Deciding what information belongs at summary versus detail level
- Comparing competitor experiences and alternative approaches
- Prioritizing feature and UX improvements
- Reviewing product behavior and identifying functional gaps
- Testing iterations and refining requirements based on observed problems
- Maintaining consistency across health, nutrition, training, and social areas

I can discuss the product rationale, user flows, feature decisions, trade-offs, and front-end experience in detail. This portfolio intentionally presents my contribution around the areas I personally own and can explain; it does **not** position me as a backend or infrastructure specialist.

---

## Technology context

The private application is an iOS-focused mobile product built with a modern **React Native / Expo** stack, integrates with **Apple Health / HealthKit**, and uses **Supabase** for cloud capabilities.

These technologies are included to explain the product environment, not as a claim of expert-level engineering proficiency in every part of the stack.

The public showcase intentionally does not expose:

- Production source code
- Credentials or secrets
- Private database or infrastructure details
- Internal implementation material that could reveal sensitive application logic

---

## Current state

Halaty is an **active pre-launch product**.

The private codebase contains working and partially working capabilities across daily health scoring, sleep, recovery, nutrition, training, reports, body progress, coaching, social sharing, onboarding, and health-data integration.

The product is still being iterated. Some areas are more mature than others, and the current focus is on closing UX gaps and turning the broad feature set into one coherent daily experience.

This repository presents Halaty as a real evolving product—not as a finished commercial application.

---

## What this project demonstrates

### Product / Business Analysis

- Problem decomposition
- Feature definition
- Workflow and requirements thinking
- Prioritization
- Product-gap identification
- Competitor comparison
- Iterative decision-making

### Product / UX

- Information architecture
- Progressive disclosure
- Interaction simplification
- Consistency across a large product
- Balancing beginner and advanced-user needs

### Data-oriented thinking

- Thinking about how health data becomes understandable user-facing information
- Distinguishing source data from derived metrics
- Designing trends, summaries, and confidence-aware presentation

---

## Why the source code is private

The purpose of this repository is to let recruiters, hiring managers, and collaborators understand **the product, the problems being solved, the structure of the experience, and the work I personally own and can explain** without publishing the private production codebase.
