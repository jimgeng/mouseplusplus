---
id: TASK-3
title: Validate a physical-layout configuration
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies:
  - TASK-1
  - TASK-2
documentation:
  - docs/design.md
type: feature
ordinal: 3000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Actual desk offsets cannot be discovered from EDID or the Windows desktop arrangement. Provide an editable layout with explicit identity binding and physical positions, allowing reliable startup only when it matches the active topology. Settle the file location and exact schema when this task starts.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 A documented minimal example supports millimetre positions, optional dimension overrides, portrait/stacked layouts and executable exclusions.
- [ ] #2 Monitor bindings are deterministic; indistinguishable devices, missing bindings and stale topologies cause actionable errors rather than guessed assignments.
- [ ] #3 Nonfinite/nonpositive sizes, physical overlaps, ambiguous shared spans and unsupported mirror layouts are rejected.
- [ ] #4 The validate command makes no cursor changes; startup correction remains disabled for invalid configuration.
- [ ] #5 Tests cover valid layouts, malformed input, unknown devices and overrides; personal machine configuration is excluded from version control.
<!-- AC:END -->
