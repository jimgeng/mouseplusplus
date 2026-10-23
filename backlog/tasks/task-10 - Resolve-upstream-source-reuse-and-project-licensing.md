---
id: TASK-10
title: Resolve upstream source reuse and project licensing
status: To Do
assignee: []
created_date: '2026-10-23T08:17:00Z'
labels: []
dependencies: []
documentation:
  - docs/design.md
type: task
ordinal: 10000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The user permits upstream-compatible reuse, but that does not select a project license. LittleBigMouse has a GPLv3 license and its Rust workspace declares GPL-3.0-only. Resolve obligations before adapting source, rather than silently copying implementation into an unlicensed project.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 An explicit user-approved decision identifies whether implementation will adapt upstream source and which project license applies.
- [ ] #2 If adapting source, required license text, attribution and source provenance are included and compatible dependency obligations are checked.
- [ ] #3 If implementing independently, the source boundary is documented without claiming that research alone guarantees a particular legal outcome.
<!-- AC:END -->
