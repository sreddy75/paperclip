---
name: Implement sharing opt-in toggle for proof moments
assignee: engineer
project: beta-launch
priority: p1
---

Add a `sharingEnabled` boolean to the student profile (default false). Students must toggle this on in settings before proof moment cards are publicly viewable at `/s/[shortcode]`.

## Requirements

- Add `sharingEnabled` boolean column to students table (default false)
- Add toggle in student settings page
- Public proof moment cards (`/s/[shortcode]`) check `sharingEnabled` before rendering
- Public cards show first name only, never email or full name
- When student taps "Share" on a proof moment for the first time and sharing is off, show a one-time prompt: "To share achievements, turn on sharing in your profile." with a link to settings

## Context

Part of the referral system (Spec 057). Australian privacy compliance for under-18 students. Public proof moment cards expose student names and achievement data.
