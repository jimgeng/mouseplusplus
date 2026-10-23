---
id: DRAFT-1
title: Investigate capture release within a focused excluded game
status: Draft
assignee: []
created_date: '2026-10-23T08:17:00Z'
labels: []
dependencies: []
documentation:
  - docs/design.md
type: spike
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Post-MVP possibility only: resume physical correction when a still-focused game genuinely releases cursor control. Explicit foreground exclusion remains the approved baseline. Clip, visibility and warp signals are imperfect; investigate concrete games and false positives before proposing a policy.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Evidence distinguishes capture/confinement/hidden-cursor signals from actual relative-input ownership and records false-positive/negative cases.
- [ ] #2 Any proposed policy is reviewed with the user before implementation and retains explicit exclusions as a predictable fallback.
<!-- AC:END -->
