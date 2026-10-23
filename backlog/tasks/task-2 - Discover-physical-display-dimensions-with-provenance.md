---
id: TASK-2
title: Discover physical display dimensions with provenance
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies:
  - TASK-1
documentation:
  - docs/design.md
type: feature
ordinal: 2000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Pixel resolution and UI scaling cannot supply panel density. Obtain physical dimensions from available hardware metadata, preserving uncertainty so the user can override bad or absent data. Avoid adding multiple discovery subsystems without evidence that they are needed.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Inspect reports physical width/height with units and provenance, or an explicit unknown/invalid result.
- [ ] #2 EDID parsing validates supported blocks and handles missing, zero, truncated or corrupt data without panicking.
- [ ] #3 Dimensions account for display orientation without double-swapping; fixtures cover landscape and portrait.
- [ ] #4 Device-interface lookup does not manufacture registry paths by parsing symbolic strings; EDID override limitations and unsupported formats are documented.
- [ ] #5 Tests cover supported EDID fixtures and dimension failures; GetDeviceCaps physical size and effective Windows DPI are not treated as authoritative measurements.
<!-- AC:END -->
