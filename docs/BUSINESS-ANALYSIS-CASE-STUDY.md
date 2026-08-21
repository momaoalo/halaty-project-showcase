# Business Analysis Case Study — Nutrition Logging

This case study shows how a product problem in **حالتي** was translated into requirements, a redesigned flow, and an implemented experience.

## 1. Problem

Food logging is a high-frequency task. If the user has to repeat a long sequence every time they eat the same food or meal, the workflow becomes harder to sustain.

The product problem was therefore not simply **“add nutrition tracking.”** It was:

> How can nutrition logging remain detailed enough to be useful without making repeated daily entry unnecessarily slow?

## 2. User need

A frequent user should be able to:

- find a food quickly;
- understand the serving being logged;
- change serving size and quantity without doing manual calculations;
- reuse foods or meals they log often;
- confirm the entry with minimal repeated work;
- see updated daily calories and macros immediately after logging.

## 3. Requirements

| ID | Requirement | Priority |
| --- | --- | --- |
| NUT-01 | The user can search for a food and review its nutrition information before adding it. | Must |
| NUT-02 | The logging flow supports **Serving Size × Quantity** rather than requiring grams only. | Must |
| NUT-03 | Frequently used foods/meals can be reached through quick logging or recent/saved patterns. | Must |
| NUT-04 | Daily calories and macros update after a confirmed log. | Must |
| NUT-05 | Packaged foods can be entered through barcode lookup when source data is available. | Should |
| NUT-06 | The interface distinguishes the food/data source where that context matters. | Should |
| NUT-07 | Missing nutrition fields should remain unknown rather than being presented as reliable zero values. | Should |

## 4. User story

> **As a user who logs food several times a day, I want to repeat common foods and choose a serving and quantity quickly so that tracking does not become a repetitive chore.**

## 5. Acceptance criteria example

### Serving and quantity

**Given** a user has selected a food  
**When** the portion sheet opens  
**Then** the user can select a serving size, adjust quantity, and see the resulting nutrition amount before confirming.

### Quick repeat

**Given** a food or meal has been used frequently or saved  
**When** the user opens Nutrition  
**Then** the item can be reached without repeating the full search flow.

### Confirmation

**Given** a valid food, serving, and quantity have been selected  
**When** the user confirms the entry  
**Then** the meal is added and the daily calorie/macro totals update.

## 6. Flow redesign

### Longer repeated path

```text
Nutrition
  → Add meal
  → Search
  → Select result
  → Set amount
  → Confirm
```

### Optimized repeat path

```text
Nutrition
  → Quick logging / recent / saved
  → Review serving × quantity
  → Confirm
```

The goal is not to remove detail. It is to keep detail available while reducing the number of repeated decisions for common actions.

## 7. Prioritization logic

The most important distinction was between capabilities needed for a reliable logging transaction and features that improve convenience.

**Must-have** work focused on:

- correct food selection;
- serving and quantity;
- correct totals after confirmation;
- a fast repeat path.

**Should-have** work included:

- barcode workflows;
- richer source/context information;
- additional convenience and discovery paths.

This sequencing keeps the core transaction usable before adding more surface area.

## 8. Traceability

| Problem / observation | Requirement | Product response | Evidence |
| --- | --- | --- | --- |
| Repeated food logging can become slow | Reduce repeated actions | Quick logging and reusable items | Nutrition screenshots in the main README |
| Grams-only entry is not natural for every food | Support serving × quantity | Portion/serving workflow | Implemented nutrition flow |
| Users need immediate feedback after a log | Update daily totals | Calories and macros refresh after confirmation | Nutrition daily summary |
| Packaged foods are difficult to find manually | Support barcode lookup | Barcode scanner flow | Barcode screen in current build |

## 9. What this case demonstrates

This case demonstrates the part of business analysis I enjoy most:

1. identifying the practical user/business problem;
2. separating core requirements from convenience features;
3. translating needs into a clear flow;
4. defining expected behavior;
5. reviewing the implemented result and iterating when friction remains.

No performance improvement percentage is claimed here because the product is still pre-launch and does not yet have a production user cohort large enough to support that measurement.