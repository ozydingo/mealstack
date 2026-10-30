# 0001: Core model

**Status:** Shape review
**Issue:** AND-17

## Problem

- Every feature depends on a shared model of how food flows from one batch cook into several different meals. We don't have one yet.
- Recipe tools are one-shot: ingredients in, one dish out. They can't say "this roast feeds three meals" or "this meal uses the roast lamb you already made."
- Planning starts from loose requests, not a recipe list: "let's have lamb this week," "eggplant was on sale," "inspire me."
- A batch cook makes a finite amount that keeps for a limited time. Meals draw it down and must use it before it turns.
- Shopping should cover what's missing without making anyone keep an inventory of their pantry.

## North stars

None yet.

## System shape

**Library: what you know how to cook.**

- **Ingredients form a network.** Raw ingredients (lamb shoulder, eggplant) and cooked components (roast lamb, shredded lamb) are both ingredients. Links say what can stand in for what: lamb can stand in for other red meat, but shredded lamb can't stand in for steak.
- A **batch cook recipe** turns ingredients into components: a lamb shoulder roast yields roast lamb and jus.
- A **meal recipe** calls for ingredients. Some come from a batch cook; the rest are bought or already in the pantry.
- **Pantry habits** replace pantry tracking: how reliably you stock each ingredient (always, usually, sometimes, never). Habits shape suggestions and the shopping list. Nothing is counted.

**Week: what you're actually doing.**

- You start with zero or more **requests**. You build the week by picking batch cooks and meals from the library.
- Each **batch cook** produces real components, with an amount and a cooked-on date. Each **meal** draws on them.
- The **shopping list** is worked out: what the week needs, minus what you always stock.

```mermaid
erDiagram
    ING["Ingredient (raw or cooked)"] }o--o{ ING : "stands in for"
    BCR["Batch cook recipe"] }o--|{ ING : "uses and yields"
    MR["Meal recipe"] }o--|{ ING : "calls for"
    HABIT["Pantry habit"] }o--|| ING : "how often stocked"

    PLAN["Plan (week)"] |o--o{ REQ["Request"] : "starts from"
    PLAN ||--|{ BC["Batch cook"] : schedules
    PLAN ||--|{ MEAL["Meal"] : schedules
    BC }o--|| BCR : follows
    MEAL }o--|| MR : follows
    BC ||--|{ COMP["Component (amount, date)"] : produces
    MEAL }o--o{ COMP : "draws from"
```

### Milestones

| | You do | The app does |
| --- | --- | --- |
| **M1: one week's plan** | Make requests, pick batch cooks and meals, and say what gets reused where. | Track components drawing down, flag spoilage risk, and build the shopping list. |
| **M2: suggestions** | Make requests. | Suggest batch cooks, reuse, and substitutions using the ingredient network and pantry habits. |
| Later | | Possibly pantry tracking. Out of scope until then. |

M1 needs only the parts of the network that you state directly. M2 is where the network starts doing the work.

<!-- Stop here and get review before writing below the fold. -->

---

## Example

| Day | Event | Roast lamb left |
| --- | --- | --- |
| Sun | Batch cook: lamb shoulder roast → 2 kg roast lamb, jus | 2 kg |
| Sun | Meal: roast lamb plate (−0.8 kg) | 1.2 kg |
| Tue | Meal: lamb birria tacos (−0.7 kg, uses jus) | 0.5 kg |
| Thu | Meal: lamb salad, goat cheese, tarragon (−0.4 kg) | 0.1 kg |

## UX

Not yet. Written after shape review.

## Architecture

Not yet.

## Open questions

- **Where does the M1 library come from?** You enter it, it's seeded, or it's generated when you ask.
- **How precise are amounts?** Grams, servings, or rough sizes.
- **Shelf life:** a default per component, or set per batch cook?
- **Transformations:** roast lamb → shredded lamb → birria filling. Is each step a node in the network, with the meal recipe doing the transforming?
- **What do the links in the network carry?** Plain "stands in for", or context too (works in tacos, not in a salad)?

## Appendix: Decision log

- 2026-10-30: No north stars yet (reviewer's call).
- 2026-10-30: No pantry tracking. Pantry *habits* (always/usually/sometimes/never) are in scope instead. Users buy what they lack and substitute what they can.
- 2026-10-30: Ingredient specificity is a network of stand-in links, not a fixed hierarchy. Superseded the "component kind" concept from the first draft: cooked components are ingredients in the same network.
- 2026-10-30: Milestones. M1 is one week's plan, with reuse stated by the user. M2 adds suggestions from the network.
- 2026-10-30: Planning starts from zero or more requests.
