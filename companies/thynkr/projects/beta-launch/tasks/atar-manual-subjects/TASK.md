---
name: ATAR tracker manual subject entry for unsupported subjects
assignee: engineer
project: beta-launch
priority: p1
---

Allow students to manually add subjects that Thynkr doesn't have practice content for (e.g., Dance, Legal Studies) with an estimated score, so the ATAR formula can use them.

## Requirements

1. Add a "manual subjects" UI section on the ATAR tracker page
2. Students pick from the full QCE subject catalogue and enter their estimated score (1-100)
3. Store manual entries (either new `manual_atar_subjects` table or JSONB on student record)
4. Pass both enrolled (mastery-derived) and manual (self-reported) subjects to the ATAR prediction formula
5. Allow editing and removing manual subjects

## Context

ATAR needs 5+ subjects but Thynkr only supports 14 of 60+ QCE subjects. Students whose combination includes unsupported subjects can't use the tracker at all. The ATAR prediction API already accepts subject + score pairs — the change is adding the UI and storage for manual entries.
