# Project Map

This is a public orientation map for the private Halaty source tree. It helps a technical reviewer understand **where responsibilities live** without exposing the complete commercial repository.

## Top-level source structure

```text
src/
├── app/                 Expo Router routes
├── features/            feature/screen composition
├── core/                domain logic and services
├── intelligence/        higher-level interpretation and guidance
├── data/                models, local persistence and cloud adapters
├── state/               Zustand stores
├── design/ + ui/        tokens and shared primitives
├── i18n/                Arabic/English resources
├── billing/             premium/entitlement behavior
└── utils/               dates, formatting and shared helpers

supabase/
├── migrations/          database schema / RLS / RPC evolution
└── functions/           server-side Edge Functions

targets/
├── watch/               Apple Watch application target
└── watch-widget/        watch/widget shared surfaces
```

## Product area → implementation backbone

| Product area | Primary private modules |
| --- | --- |
| Today / daily state | `features/home2/*`, `core/scoring/*`, intelligence layer |
| Sleep | `core/scoring/sleep.ts`, sleep/recovery helpers, score detail UI |
| Recovery | `core/scoring/recovery.ts`, baseline + explanation modules |
| Strain / fatigue | `core/scoring/strain.ts`, load/recovery inputs |
| Nutrition | `core/nutrition/*`, nutrition feature screens, food-source adapters |
| Training | `core/workouts/*`, training feature screens, workout sync/import |
| Body / progress | `core/body/*`, body feature screens, composition/bio-age logic |
| Reports | `core/reports/*`, report/trend feature screens |
| Social / friends | social core + Supabase social backend + permission-aware snapshots |
| AI / coaching | coach/AI domain modules + server-side AI Edge Function |
| Apple Health | `core/health/*`, sync/extraction modules |
| Apple Watch | `targets/watch/*`, phone/watch delivery bridge |
| Cloud data | `data/supabase/*`, `supabase/migrations/*` |

## Scoring core

```text
src/core/scoring/
├── sleep.ts
├── recovery.ts
├── strain.ts
├── activity / performance logic
├── baselines.ts
├── hrvSource.ts
├── maturity / confidence logic
├── stats.ts
└── engine / snapshot orchestration
```

The score engines are intended to be framework-light. React Native UI should consume their result contracts rather than reimplement formulas in components.

## Health pipeline

```text
HealthKit
  ↓
source classification / feature extraction
  ↓
DailyRecord + workout records
  ↓
local repositories
  ↓
baselines + scoring core
  ↓
intelligence / explanations
  ↓
feature hooks
  ↓
UI
```

## Nutrition pipeline

```text
Search / barcode / saved food / custom food
  ↓
source adapter
  ↓
normalized FoodItem
  ↓
portion × quantity
  ↓
meal entry
  ↓
daily totals / targets / trend context
```

Relevant private modules cover:

- internal food database;
- Arabic/English search;
- USDA lookup;
- Open Food Facts barcode lookup;
- cache;
- custom foods;
- saved/recent meals;
- serving/portion handling;
- daily macro/micronutrient summary;
- target logic.

## Training pipeline

```text
weekly plan / routine
  ↓
today's session
  ↓
live set logging
  ↓
WorkoutRecord
  ↓
PR / progression / fatigue / deload analysis
  ↓
reports + recovery context
```

Training domain modules include exercise definitions, plan generation, session templates, progressive overload, deload suggestions, muscle-fatigue context, and workout import/linking.

## Cloud architecture map

```text
src/data/repositories.ts
        ↓
src/data/local*         ← local-first persistence
        ↓
src/data/supabase/
  client
  dataBackend
  tableRegistry
  mappers
  database.types
  authBridge
  socialBackend
  photoStorage
        ↓
Supabase Postgres / Auth / Storage
        ↓
supabase/functions/*
```

## Why show paths if the repository is private?

A technical hiring manager can see that the application has intentional boundaries and can ask precise questions such as:

- Why keep scoring outside screens?
- How does local-first sync behave?
- How are stale multi-device writes handled?
- Which data is derived versus raw?
- Why are Edge Functions used for some integrations?
- How does a missing HRV measurement flow to the UI?

That is more useful than publishing thousands of lines of code with no architecture narrative.

## Public source boundary

This repository intentionally exposes:

- system diagrams;
- domain formulas;
- selected implementation excerpts;
- module paths;
- design decisions;
- backend model;
- quality gates.

It intentionally does not expose the entire private application source or secrets.
