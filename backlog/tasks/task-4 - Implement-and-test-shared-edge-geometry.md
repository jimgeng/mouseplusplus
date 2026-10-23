---
id: TASK-4
title: Implement and test shared-edge geometry
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies: []
documentation:
  - docs/design.md
type: feature
ordinal: 4000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Cursor correction must preserve physical edge position across panels of different density without changing motion inside a panel. Keep this policy independent of Win32 so a human can inspect and test the mathematics. Geometry can be developed with synthetic rectangles before real discovery is complete.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Pixel-centre, rounding, half-open bounds and coordinate-unit conventions are explicit and tested.
- [ ] #2 All four shared-edge crossings preserve the tangential physical coordinate and land on valid destination pixels.
- [ ] #3 Unmatched edge spans, nonadjoining gaps and corner-only contact do not produce crossings; portrait and stacked arrangements work.
- [ ] #4 Corner ties, floating-point adjacency tolerance and fast-movement overshoot have deterministic documented rules.
- [ ] #5 Tests cover negative desktop coordinates, mixed density, partial spans, rapid reversals and no reverse-crossing from a landing pixel during tangential motion.
- [ ] #6 Geometry tests require neither Windows APIs nor a live cursor and include round-trip or bounded-error checks.
<!-- AC:END -->
