---
id: TASK-8
title: Harden Windows lifecycle and recovery
status: To Do
assignee: []
created_date: '2026-10-23T08:17:00Z'
labels: []
dependencies:
  - TASK-7
documentation:
  - docs/design.md
type: feature
ordinal: 8000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
A background input utility must stop safely when Windows changes the desktop underneath it. Refine and split this task before implementation if lifecycle responsibilities no longer fit one reviewable change. Event handler presence alone is not recovery evidence.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Display hotplug/mode/rotation changes pause correction, discard stale state and resume only after successful rediscovery and configuration validation.
- [ ] #2 Suspend/resume, lock/unlock and relevant session/desktop transitions have documented tested state transitions; secure desktops are not manipulated.
- [ ] #3 Single-instance enforcement, emergency disable and normal shutdown are verified, including registration or permission failures.
- [ ] #4 Hook timeout/removal limitations and an evidence-backed recovery policy are documented and tested without a busy loop or a claim of perfect detection.
- [ ] #5 If an approved backend uses clipping, ownership-aware restoration and crash/termination limitations are verified; otherwise no clipping machinery is added.
- [ ] #6 State-machine/API-failure tests and a reproducible manual lifecycle checklist cover recovery paths.
<!-- AC:END -->
