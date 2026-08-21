# Quality, Testing & Release Engineering

A portfolio for a real product should show more than screenshots and feature claims. This document summarizes the quality controls present in the private Halaty codebase without publishing the private test suite itself.

## Automated quality commands

The current project exposes dedicated commands for:

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

These are separate because they answer different release questions.

| Gate | Purpose |
| --- | --- |
| Lint | static code-quality checks |
| Typecheck | TypeScript contract validation |
| Tests | domain and feature behavior |
| Timezone tests | date/day-boundary correctness |
| Runtime performance baseline | catch major hot-path regressions |
| Route verification | detect broken/missing application routes |
| Supabase schema verification | ensure app assumptions match backend schema |
| Production RLS smoke | verify access-control behavior against a production-like backend |
| Release gate | combine release-critical checks |
| App Store check | validate packaging/release expectations |

## Why domain tests matter here

Health scoring logic is separated from UI, which makes it possible to test behavior such as:

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

A screenshot cannot prove those rules. Tests can.

## Example: methodology drift protection

Several scoring modules expose the same curves and weights used by the production engine so methodology screens/tests can consume the implementation rather than retyping a second set of constants.

The objective is to prevent this failure mode:

```text
UI explanation = old formula
production engine = new formula
```

## Missing-data tests

Missing data is a core product rule, not just an edge case.

Tests cover the expectation that an absent signal should not automatically become:

- `0`;
- a fake score;
- a fabricated workout detail;
- a fake nutrition micronutrient;
- a crash caused by an invalid persisted record.

## Runtime validation

High-value persisted records are structurally validated before they are trusted by the domain layer. Invalid records can be skipped/treated as missing instead of being allowed to poison later calculations.

Migration logic is intentionally conservative: it may repair safe structural metadata, but it should not invent health measurements.

## Backend authorization checks

The current cloud stack uses Supabase and includes dedicated RLS verification/smoke tooling.

This matters because a mobile app's UI can look perfectly correct while the backend accidentally allows one user to read or mutate another user's data.

Access control therefore has its own release checks rather than being treated as a manual afterthought.

## Privacy checks

Sensitive health applications need checks at data boundaries, especially for:

- exports;
- friend/shared snapshots;
- cloud synchronization;
- local file URIs;
- raw HealthKit sample arrays;
- progress photos.

The private implementation distinguishes derived summaries from raw data and is designed to reject unsafe payloads before they become public/shareable surfaces.

## Local protection / fail-closed behavior

The native local store attempts to use a device-protected key. The intended security behavior is not "if encryption fails, quietly save plaintext." The safer fallback is non-persistent memory until the issue is resolved.

That is an example of a release decision where preserving confidentiality is more important than silently preserving persistence.

## Performance

The project includes a runtime performance baseline command because the Today/Home experience can combine multiple derived domains and historical queries.

The objective is to keep heavy domain calculations out of unnecessary re-render loops and make regressions measurable rather than subjective.

## Release philosophy

A feature is not considered production-ready simply because the screen renders.

A release-quality path asks:

1. Does it typecheck?
2. Does domain behavior still pass tests?
3. Does the route exist and open?
4. Does backend authorization still hold?
5. Are privacy boundaries respected?
6. Does it behave across timezone/day changes?
7. Has runtime performance materially regressed?
8. Does the release configuration pass the final gate?

## Public-vs-private boundary

The test names, architecture, and release approach can be demonstrated publicly. The full private test suite and production environment configuration stay in the private repository.
