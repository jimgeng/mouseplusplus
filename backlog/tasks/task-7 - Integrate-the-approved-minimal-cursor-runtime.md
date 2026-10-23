---
id: TASK-7
title: Integrate the approved minimal cursor runtime
status: To Do
assignee: []
created_date: '2026-10-23T08:17:00Z'
labels: []
dependencies:
  - TASK-3
  - TASK-5
  - TASK-6
documentation:
  - docs/design.md
type: feature
ordinal: 7000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Join validated layout, pure crossing policy, the experimentally approved backend and foreground exclusions into one small executable. Focus on ordinary runtime operation; lifecycle hardening and hardware acceptance remain separate work.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Correction starts only with a valid current topology and allowed foreground state, following the approved backend decision without unresolved feasibility assumptions.
- [ ] #2 Interior movement and button/wheel events pass through; crossing and blocked-edge behavior match geometry tests while normal Windows acceleration stays unchanged.
- [ ] #3 Excluded foreground executables receive no correction; resume initializes fresh tracking and handles unexpected cursor relocations safely.
- [ ] #4 API failure or invalid state cannot leave the process indefinitely swallowing movement; no Rust panic unwinds through a Win32 callback.
- [ ] #5 Explicit start/stop behavior and bounded diagnostics work without per-move logging, blocking locks or process/file inspection in the callback.
- [ ] #6 Integration uses fakes for ordinary automated tests; interactive smoke-test evidence and remaining limitations are recorded separately.
<!-- AC:END -->
