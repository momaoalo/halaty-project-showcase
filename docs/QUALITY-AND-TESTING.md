# Quality, Testing & Release

A working screen is not enough to prove that a health-related product behaves correctly. This document summarizes the quality controls used in حالتي.

## Automated quality commands

The project exposes dedicated commands for:

```text
npm run lint
npm run lint:budget
npm run typecheck
npm test
npm run test:timezones
npm run perf:runtime
npm run routes:check
npm run rules:check
npm run rls:production-smoke
npm run release:gate
npm run appstore:check
```

| Gate | Purpose |
| --- | --- |
| Lint | static code-quality checks |
| Typecheck | TypeScript contract validation |
| Tests | domain and feature behavior |
| Timezone tests | date/day-boundary correctness |
| Runtime performance baseline | detect major hot-path regressions |
| Route verification | detect broken or missing routes |
| Supabase schema checks | verify application/backend assumptions |
| RLS smoke checks | verify access-control behavior |
| Release gate | combine release-critical checks |
| App Store check | validate packaging/release expectations |

## Why domain tests matter

Health scoring and other domain rules are separated from UI so behavior can be checked independently of screen rendering.

Examples include:

- missing data;
- baseline changes;
- HRV source stability;
- sleep scoring anchors;
- recovery stability;
- fatigue thresholds;
- timezone/day-boundary behavior;
- nutrition target calculations;
- workout progression/deload rules;
- privacy-safe export behavior.

## Missing-data behavior

Missing data is treated as a product state, not only an edge case.

Checks are intended to prevent an unavailable signal from becoming:

- `0` when zero has a different meaning;
- a fabricated score;
- invented workout details;
- fake nutrition micronutrients;
- a crash caused by an invalid persisted record.

This supports the product principle **missing is not zero**.

## Runtime validation

High-value persisted records are structurally validated before domain logic relies on them. Invalid records can be skipped or treated as missing rather than silently contaminating later calculations.

## Authorization checks

The Supabase stack includes RLS verification because UI correctness does not prove backend authorization correctness.

Access control is therefore treated as a release concern rather than only a development-time assumption.

## Privacy checks

Relevant data boundaries include:

- exports;
- friend/shared snapshots;
- cloud synchronization;
- local file references;
- raw HealthKit sample arrays;
- progress photos.

The application distinguishes derived summaries from raw data so sharing/export surfaces can use the minimum information needed for their purpose.

## Performance

The project includes a runtime performance baseline because daily-summary screens can combine multiple domains and historical calculations.

The goal is to make major regressions measurable instead of relying only on visual inspection.

## Release review

A release-quality review asks:

1. Does the project typecheck?
2. Do domain behaviors still pass tests?
3. Do routes open correctly?
4. Does backend authorization still behave as intended?
5. Are privacy boundaries respected?
6. Does behavior remain correct across timezone/day changes?
7. Has runtime performance materially regressed?
8. Does the release configuration pass final checks?

## What this demonstrates

From a portfolio perspective, the value of this section is not the number of test files. It demonstrates a **quality mindset**: defining expected behavior, checking edge cases, separating functional correctness from visual completion, and including release risks in the delivery process.
