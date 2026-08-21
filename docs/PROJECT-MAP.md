# Project Map

This map helps a technical reviewer understand **where responsibilities live** in the حالتي application.

## Selected top-level source structure

```text
src/
├── app/                 Expo Router routes
├── features/            feature/screen composition
├── core/                domain logic and services
├── intelligence/        higher-level interpretation and guidance
├── data/                models, local persistence and cloud adapters
├── state/               Zustand stores
├── design2/ + ui2/      tokens and shared UI primitives
├── i18n/                Arabic/English resources
├── billing/             premium/entitlement behavior
├── server/              server-facing application helpers
└── utils/               dates, formatting and shared helpers

supabase/
├── migrations/          database schema / RLS / RPC evolution
└── functions/           server-side Edge Functions

targets/
├── watch/               Apple Watch application target
├── watch-widget/        watch/widget shared surfaces
└── rest-widget/         rest-timer / Live Activity surface
```

## Product area → implementation backbone

| Product area | Main implementation areas |
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
| AI / coaching | coach/AI domain modules + server-side AI function |
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

The score engines are kept separate from screen components so UI can consume their result contracts instead of redefining formulas.

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

Nutrition implementation areas include Arabic/English search, USDA lookup, Open Food Facts barcode lookup, caching, custom foods, saved/recent meals, serving/portion handling, daily summaries, and target logic.

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
progression / fatigue / deload analysis
  ↓
reports + recovery context
```

## Cloud architecture map

```text
src/data/repositories.ts
        ↓
local persistence
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

## Questions this map helps answer

A technical reviewer can use this structure to discuss questions such as:

- Why keep scoring outside screens?
- How does local-first synchronization behave?
- How are stale writes handled?
- Which data is derived versus raw?
- Why are server-side functions used for some integrations?
- How does a missing health signal reach the UI?

The purpose of this document is orientation: it connects **product areas to system responsibilities** without turning the portfolio into a large code dump.
