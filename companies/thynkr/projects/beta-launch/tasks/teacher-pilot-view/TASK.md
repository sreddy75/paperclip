---
name: Verify and polish teacher dashboard for pilot
assignee: engineer
project: beta-launch
priority: p1
---

The teacher dashboard infrastructure already exists at `/teacher/students`, `/teacher/conversations`, `/teacher/performance`. Verify it works correctly and polish for the school pilot.

## Requirements

1. Verify all teacher dashboard pages load without errors
2. Check that student data displays correctly (enrollment, sessions, mastery, ATAR)
3. Ensure the teacher can see which students are active and their engagement metrics
4. Fix any UI issues (broken layouts, missing data, error states)
5. Verify teacher auth flow works (teacher accounts, school enrollment)

## Acceptance Criteria

- Teacher can log in and see their class roster
- Teacher can view individual student progress (mastery per subject, sessions completed)
- Teacher can see ATAR trend for each student
- No console errors or broken UI on teacher pages
- Pages follow the Thynkr design system (dark shell + light content)
