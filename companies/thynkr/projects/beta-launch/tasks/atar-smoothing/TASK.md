---
name: ATAR prediction smoothing to prevent noisy swings
assignee: engineer
project: beta-launch
priority: p1
---

Implement smoothing for per-session ATAR recalculation to prevent demotivating 1-2 point swings from a single bad session.

## Options (pick the simplest that works)

- (a) Exponential moving average on mastery scores before feeding to ATAR formula
- (b) 3-session buffer: only recalculate ATAR after 3+ sessions in a subject
- (c) Calculate per-session but hide the update until 3+ data points exist

## Context

Recalculating ATAR after every practice session causes volatile predictions. A single bad session can drop the predicted ATAR by 1-2 points, which erodes student trust in the tracker. The tracker should show trends, not noise.

## Acceptance Criteria

- ATAR prediction doesn't swing more than 0.5 points from a single session
- Students still see their ATAR update regularly (not frozen for days)
- The smoothing approach is documented in a code comment
