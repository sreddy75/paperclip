---
name: thynkr-qce-content
description: QCE curriculum knowledge, subject structure, and content seeding format for Thynkr
---

# Thynkr QCE Content Guide

## QCE Overview

Queensland Certificate of Education (QCE) is the senior secondary qualification for Queensland, Australia. Students typically study 5-6 subjects in Years 11-12, and their results determine their ATAR (Australian Tertiary Admission Rank) for university entry.

## Subjects Currently Live in Thynkr (14)

Chemistry, Biology, Physics, Specialist Maths, General Maths, Mathematical Methods, English, PE, Psychology, Design + Technologies, UCAT, Engineering, Business, and more being added.

## Content Structure

Each subject in Thynkr contains:
- **Concepts** — individual learning topics within the subject
- **Questions** — practice questions linked to concepts, with difficulty calibration
- **Misconceptions** — common student errors with diagnostic questions

## Seed File Format

Seed files live at `apps/api/src/seed/` (107 files). Each follows this pattern:

### Subject Seed
```typescript
// seed-subject-name.ts
import { db } from '../models/db';
import { subjects, concepts, questions, misconceptions } from '../models/schema';

const subjectData = {
  name: 'Chemistry',
  slug: 'chemistry',
  description: '...',
  qceCode: 'CHM',
  // ...
};
```

### Question Format
Questions include:
- `text` — the question stem
- `format` — mcq | short_response | extended_response | calculation
- `cognitiveLevel` — retrieval | comprehension | analysis | knowledge_utilisation
- `difficulty` — 1-5 scale
- `options` — for MCQ: array of { text, isCorrect }
- `explanation` — detailed worked solution
- `conceptId` — linked to a specific concept
- `hints` — array of progressive hints

### Misconception Format
Misconceptions include:
- `description` — what the student typically gets wrong
- `conceptId` — which concept this relates to
- `diagnosticQuestion` — a question that reveals whether the student holds this misconception
- `remediation` — explanation of why the misconception is wrong and what's correct

## QCAA Cognitive Verbs

QCE assessments use specific cognitive verbs that map to difficulty levels:
- **Retrieval**: recall, identify, describe, list
- **Comprehension**: explain, compare, discuss, interpret
- **Analysis**: analyse, evaluate, examine, investigate
- **Knowledge Utilisation**: create, design, justify, synthesise

Questions should be tagged with the appropriate cognitive level.

## ATAR Scaling

ATAR predictions use subject scaling parameters. Each subject has:
- Mean and standard deviation from QCAA published data
- Scaling coefficients that adjust raw scores
- These are seeded in the database — accuracy is critical

## Content Quality Standards

- All questions must be aligned to the QCAA syllabus
- Use cognitive verb tagging from QCAA frameworks
- Explanations must be detailed enough for self-study
- MCQ distractors should represent real misconceptions, not random wrong answers
- Questions should cover the full cognitive level spectrum (not just retrieval)
