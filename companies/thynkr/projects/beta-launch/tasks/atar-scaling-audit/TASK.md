---
name: Audit ATAR scaling parameters for all subjects
assignee: engineer
project: beta-launch
priority: p1
---

Verify that ATAR scaling parameters are correctly seeded for all subjects, not just Chemistry.

## Requirements

1. Read the ATAR scaling logic in the codebase (likely in `apps/api/src/services/atar/`)
2. Check the seed data for each of the 14 live subjects
3. Verify scaling parameters (mean, std dev, coefficients) against QCAA published data
4. Document any subjects with missing or incorrect parameters
5. Fix any incorrect values

## Acceptance Criteria

- All 14 live subjects have verified scaling parameters
- Parameters match QCAA published data (cite the source)
- Any subjects that can't be verified are flagged with a comment
