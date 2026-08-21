# Core Algorithms & Scoring Logic

This document exposes selected **real scoring logic** from Halaty at a level that a technical reviewer can inspect without publishing the full private source tree.

The full implementations live in the private project under `src/core/scoring/*`. The formulas below reflect the current implementation and intentionally separate **evidence-backed choices** from **product/design choices**.

## Design rule: personalization before population comparison

Several physiological inputs are interpreted against the user's own historical baseline rather than a fixed population threshold.

For recovery, HRV and resting heart rate are evaluated relative to the user's baseline. The private implementation uses a smallest-worthwhile-change style normalization for baseline deviation where the data supports it.

That means the question is closer to:

> **How different is this user from their normal state?**

rather than:

> **Is this user's HRV better than another person's HRV?**

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

Deep/REM stage duration is intentionally **not given direct score weight**. Stage information can still be displayed as context.

### Key behaviors

- Duration and consistency have equal top-level weight.
- Naps contribute to total sleep duration/debt repayment.
- Consistency uses variation in sleep timing over recent history.
- Sleep debt is accumulated against the lower bound of the target range.
- The duration curve is asymmetric: short sleep is penalized much more strongly than long sleep.
- Long sleep used to repay existing debt is not treated like severe undersleep.
- If the main sleep record is missing, the engine does not fabricate a score.

### Sufficiency gate

A plain weighted average creates a problem: four hours of sleep can look artificially acceptable if the remaining components are strong.

Halaty therefore applies a duration sufficiency cap:

```ts
const SUFFICIENCY_MARGIN = 25;

function capBySufficiency(score: number, durationScore?: number) {
  if (durationScore === undefined) return score;
  return Math.min(score, durationScore + SUFFICIENCY_MARGIN);
}
```

This is a deliberate product/modeling decision: good consistency should not fully compensate for severe sleep restriction.

### Missing data

Each component may be unavailable independently. Missing values are excluded rather than silently converted to zero. But the sleep score itself is anchored to an actual sleep record.

Private source: `src/core/scoring/sleep.ts`

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

A second internal recovery variant removes sleep and renormalizes the remaining physiological/load components for contexts where double-counting sleep would be undesirable.

### Baseline-centered scoring

A value exactly at the personal baseline is not mapped to 100. The current neutral anchor is 75, leaving headroom for an unusually favorable day.

Selected curves from the real engine:

```ts
const NEUTRAL_AT_BASELINE = 75;

// HRV deviation in SWC units
score = clamp(75 + 25 * min(deltaSWC, 1.5), 0, 100);

// Resting HR deviation in bpm
score = clamp(75 - 7.5 * deltaBpm, 0, 100);

// Recent load deviation in SWC units
score = clamp(75 - 15 * deltaSWC, 0, 100);
```

### Wake-anchored load

Recovery is meant to describe what the user is recovering **from**. The load component therefore excludes the current day's still-accumulating activity and uses prior-day history. Today's activity belongs to the live strain/load story; it should influence tomorrow's recovery rather than causing the recovery score to drift downward during the same day.

### Deload signal

The engine also contains a conservative deload flag based on a multi-day combination of suppressed HRV and elevated resting heart rate relative to baseline.

Private source: `src/core/scoring/recovery.ts`

---

## 3. Strain / accumulated fatigue

Strain is intentionally separate from readiness/recovery.

```text
0 = fresh
100 = high accumulated fatigue
```

Current equation:

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

The engine requires enough available component weight before returning a value. This prevents a single weak input from masquerading as a complete fatigue estimate.

```ts
if (availableWeight < 0.5) return noScore;
```

Private source: `src/core/scoring/strain.ts`

---

## 4. Missing data is a first-class state

One of the most important engineering rules in Halaty is:

> **Missing is not zero, and uncertainty is not certainty.**

Examples:

- no sleep record → do not invent a sleep score;
- insufficient baseline history → keep the component unavailable;
- absent HRV/RHR → show a missing-signal explanation rather than synthesizing the value;
- incomplete nutrition micronutrients → unknown, not `0`;
- Apple Watch workout without strength detail → do not invent exercises, sets, reps, or weights.

This rule exists at both domain and UI-contract level.

---

## 5. Why formulas are exposed in the product

The private application contains methodology surfaces that are intended to display the same curves/anchors used by the engines rather than maintaining a separate marketing explanation that can drift from the code.

Several scoring modules export their curves/weights specifically so methodology UI and tests can reference the implementation itself.

This reduces a common product risk:

```text
marketing copy says one thing
        ≠
actual production formula
```

---

## 6. Other algorithmic areas

The private project also contains domain logic for areas such as:

- dynamic nutrition targets;
- sleep debt and regularity;
- workload / training-load interpretation;
- workout-plan matching;
- progressive overload suggestions;
- deload suggestions;
- muscle fatigue / recovery context;
- body-composition trends;
- biological-age estimation;
- personal trend/correlation discovery;
- data confidence and maturity gating.

These are kept out of this public document unless they can be explained accurately and safely without exposing private implementation details.

## What this document is — and is not

This is a **technical showcase of the implemented system**, not a claim that every constant is a universally validated medical threshold. Where research provides a framework but not an exact production constant, the implementation treats the constant as a design/modeling choice and documents it as such.

Halaty is a wellness/product system, not a diagnostic medical device.
