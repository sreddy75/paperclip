---
name: Create parental consent and compliance documents
assignee: pilot-lead
project: beta-launch
priority: p1
---

Create the legal and compliance documents needed for under-18 students to use Thynkr in a school pilot context.

## Deliverables

1. **Parental consent form** — covers: AI-powered learning, data collection (learning data, email, name), email notifications, spaced repetition reminders. Must be clear, non-technical, and reassuring.

2. **School data-sharing agreement** — covers: what data Thynkr collects, how it's stored (Railway-hosted PostgreSQL in AU region), retention policy, who has access, deletion rights.

3. **Privacy policy update** — add specific sections for:
   - Under-18 data handling
   - AI usage disclosure (Claude API for tutoring)
   - Student data rights (access, correction, deletion)
   - School pilot data sharing scope

## Context

- Platform collects learning data, sends emails, and uses AI with minors
- Australian Privacy Act applies (APP 3 — collection from children requires parental consent)
- Could block pilot if not handled
- The school will likely have their own consent process — these docs need to integrate with that

## Acceptance Criteria

- Consent form is written in plain English (not legalese)
- Data-sharing agreement covers all data Thynkr collects
- Privacy policy update is specific and accurate
- All documents reference Australian privacy law correctly
