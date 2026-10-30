# Design docs

Adapted from [The Review Tax](http://www.abigstick.com/2026/09/24/review-tax/index.html).

Review, not build, is the bottleneck. Review at the wrong level is where important details get lost. Designs that are long, full of distracting detail, or not quite there make it worse, whether a person or an agent wrote them.

## Tenets

- **Align most often at the earliest, highest levels.**
- Produce clear, thoughtful designs to review and to implement.
- Reduce the review tax. Detail that wouldn't change a decision at the level being reviewed costs attention.
- Outsource doing, not thinking, to agents.
- Own your agent's output. Curate it before asking anyone to review it.

## Review in stages

Each stage stops for review before the next one is written. Earlier stages are cheap to review and set the context for everything after.

| Stage | Contents | Then |
| --- | --- | --- |
| 1. Problem | 3–5 bullets. If it doesn't fit, it may be a wish list. | |
| 2. North stars | A few bullets, linked to any north star docs. The proposal aligns with them or deviates on purpose. | |
| 3. System shape | Diagrams and high-level descriptions. No specific tools, method names, or code exploration. | **Stop. Review.** |
| 4. UX | Flows, scenarios, screens. | **Review.** Alignment here informs the architecture. |
| 5. Architecture | Deeper diagrams, data model, edge cases, error handling, performance, integration, rollout. | **Review**, with both human and agent reviewers. |

Stages 1–3 are **above the fold**. Stages 4–5 and everything else are **below the fold**. Small changes can skip stages that don't apply; say so in the doc.

Asking a reviewer to infer the system, its architecture, or its UX from a code PR is an anti-pattern. So is asking them to evaluate detail before they understand the problem and north stars.

## Keep it current

A design doc is context for every agent and person who works on it later. The body is the current agreed design. When a decision changes it, rewrite the body, and record the decision and its reason in the *Decision log* appendix.

## Files

`NNNN-short-title.md`, numbered in order. Start from [the template](0000-template.md).
