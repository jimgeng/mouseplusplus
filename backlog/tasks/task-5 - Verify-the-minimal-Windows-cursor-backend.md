---
id: TASK-5
title: Verify the minimal Windows cursor backend
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies:
  - TASK-1
  - TASK-4
documentation:
  - docs/design.md
priority: high
type: spike
ordinal: 5000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Upstream uses ClipCursor as well as SetCursorPos, so a simpler reposition-only backend is unproven. Run a bounded, explicitly enabled Windows experiment; this is a decision gate, not permission to ship persistent confinement or a production runtime.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Before live hook testing, an independent emergency-disable path works and hook registration or hotkey failures are visible.
- [ ] #2 The experiment records WH_MOUSE_LL candidate coordinates, suppression and SetCursorPos outcomes for ordinary, stacked/portrait and logically blocked edge spans.
- [ ] #3 Tests examine Windows edge clamping, cross-display pointer assistance and fast/diagonal movements; unavailable hardware cases remain explicitly unverified.
- [ ] #4 Own repositioning does not recurse, callbacks do not block, and button/wheel input is untouched.
- [ ] #5 Termination and failure tests show the experiment leaves no application-owned confinement behind.
- [ ] #6 A concise evidence-backed backend recommendation updates the design record; adding clipping or changing the input approach requires user approval before implementation.
<!-- AC:END -->
