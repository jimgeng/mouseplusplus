---
id: TASK-1
title: Inspect active Windows display topology
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies: []
documentation:
  - docs/design.md
priority: high
type: feature
ordinal: 1000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The application needs a trustworthy read-only picture of the active desktop before it can correct input. Establish display discovery and an inspect command without installing hooks or moving the cursor. Automatic discovery is required; Windows logical positions are not physical desk positions.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Inspect reports each active source/target relationship, desktop pixel rectangle, orientation and available display names/device identifiers.
- [ ] #2 Topology changes between sizing and querying are retried safely; errors are reported rather than replaced with plausible values.
- [ ] #3 Mirrored or otherwise ambiguous source/target mappings are identified explicitly, not silently treated as independent cursor surfaces.
- [ ] #4 DPI awareness is established before display-dependent initialization; reported coordinates are not multiplied by Windows scaling.
- [ ] #5 Pure parsing/joining logic has tests, and a Windows inspect smoke-test procedure is documented with actual execution evidence or clearly marked pending.
<!-- AC:END -->
