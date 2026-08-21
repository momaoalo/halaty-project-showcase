# Core Algorithms & Scoring Logic

This document shows selected scoring logic from the current حالتي implementation so a technical reviewer can understand how product rules translate into calculations.

The important distinction is between:

- **product requirements** — what the score should communicate;
- **modeling choices** — how inputs are weighted or normalized;
- **implementation behavior** — how missing data, baselines, and edge cases are handled.

## Design rule: personalization before population comparison

Several physiological inputs are interpreted against the user’s own historical baseline rather than a fixed population threshold.

For recovery, HRV and resting heart rate are evaluated relative to the user’s recent normal state.

The product question is closer to:

> **How different is this user from their usual state?**

rather than:

> **Is this user’s absolute value better than someone else’s?**

---

## 1. Sleep score

Current high-level equation:

```text
Sleep =
  0.30 × Duration
+ 0.30 × Consistency
+ 0.20 × Efficiency
+ 0.15 × Sleep Debt
+ 0.05 × Sleep Latency
```

Deep/REM stage duration is displayed as context rather than being given direct score weight.

### Key behaviors

- duration and consistency have the highest top-level weight;
- naps can contribute to total sleep duration/debt repayment;
- consistency uses recent sleep-timing variation;
- short sleep is penalized more strongly than modest oversleep;
- if the main sleep record is missing, the system does not fabricate a score.

### Sufficiency gate

A plain weighted average can make severe undersleep look too acceptable when secondary components are strong.

The implementation therefore caps the combined result relative to the duration component:

```ts
const SUFFICIENCY_MARGIN = 25;

function capBySufficiency(score: number, durationScore?: number) {
  if (durationScore === undefined) return score;
  return Math.min(score, durationScore + SUFFICIENCY_MARGIN);
}
```

This is a modeling/product choice intended to prevent strong consistency or efficiency from completely masking severe sleep restriction.

---

## 2. Recovery score

Current display equation:

```text
Recovery =
  0.40 × HRV
+ 0.25 × Resting Heart Rate
+ 0.20 × Sleep Recovery
+ 0.15 × Recent Load
```

### Baseline-centered scoring

A value at the user’s baseline is treated as a neutral-good state rather than automatically receiving the maximum score.

Selected implementation behavior:

```ts
const NEUTRAL_AT_BASELINE = 75;

// HRV deviation in normalized baseline units
score = clamp(75 + 25 * min(deltaSWC, 1.5), 0, 100);

// Resting HR deviation in bpm
score = clamp(75 - 7.5 * deltaBpm, 0, 100);

// Recent load deviation in normalized baseline units
score = clamp(75 - 15 * deltaSWC, 0, 100);
```

### Wake-anchored load

Recovery is intended to describe what the user is recovering **from**. The load component therefore uses prior activity context rather than allowing the current day’s still-accumulating activity to continually rewrite the same morning recovery state.

---

## 3. Strain / accumulated fatigue

Strain is intentionally separate from recovery.

```text
0 = fresh
100 = high accumulated fatigue
```

Current high-level equation:

```text
Strain =
  0.30 × Acute:Chronic Load component
+ 0.35 × Sleep Debt component
+ 0.35 × HRV Suppression component
```

Selected implementation behavior:

```ts
function acwrStrain(acwr: number) {
  return clamp((acwr - 1) / (1.8 - 1), 0, 1);
}

function sleepDebtStrain(debtMinutes: number) {
  return clamp(debtMinutes / 300, 0, 1);
}

function hrvSuppressionStrain(deltaSWC: number) {
  return clamp(-deltaSWC / 2, 0, 1);
}
```

The engine also requires enough available input weight before returning a value:

```ts
if (availableWeight < 0.5) return noScore;
```

This prevents one weak input from being presented as a complete fatigue estimate.

---

## 4. Missing data is a first-class state

One of the most important rules across the product is:

> **Missing is not zero, and uncertainty is not certainty.**

Examples:

- no sleep record → no invented sleep score;
- insufficient baseline history → keep the component unavailable;
- absent HRV/RHR → show a missing-signal state;
- incomplete nutrition micronutrients → unknown rather than automatic zero;
- incomplete workout data → do not invent exercises, sets, reps, or weights.

This rule exists in both domain behavior and the UI contract.

---

## 5. Why expose methodology

Methodology surfaces are intended to reflect the same rules used by the application rather than maintaining a separate explanation that can drift from implementation.

That reduces a simple but important product risk:

```text
what the interface explains
        ≠
what the calculation does
```

---

## 6. Other algorithmic areas

The application also contains logic for areas such as:

- dynamic nutrition targets;
- sleep debt and regularity;
- workload interpretation;
- workout-plan matching;
- progressive overload suggestions;
- deload suggestions;
- muscle fatigue/recovery context;
- body-composition trends;
- biological-age estimation;
- personal trend/correlation discovery;
- data-confidence and maturity gating.

## What this document demonstrates

This is a **technical case study of the implemented product**, not a claim that every constant is a universal medical threshold.

Its portfolio value is showing how a product requirement becomes an explicit model with:

- defined inputs;
- weighting choices;
- baseline logic;
- missing-data behavior;
- edge-case handling;
- an explanation that can be reviewed alongside the user experience.

حالتي is a wellness/product application, not a diagnostic medical device.
