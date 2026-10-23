---
id: TASK-6
title: Implement explicit foreground-executable exclusions
status: To Do
assignee: []
created_date: '2026-10-23T08:16:00Z'
labels: []
dependencies: []
documentation:
  - docs/design.md
type: feature
ordinal: 6000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
The MVP does not need to infer game capture. It must stop correcting for an explicitly excluded foreground executable, including when that executable is already focused at startup, and recover without replaying stale motion. This policy can be tested without live cursor correction.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Matching uses case-insensitive exact executable basenames and, if supported, exact full paths; no substring/glob matches or merely-running-process bypass.
- [ ] #2 Initial foreground identity is resolved before correction eligibility; changing focus into an exclusion disables correction and leaving it resets cursor history before resumption.
- [ ] #3 Process resolution stays outside the mouse callback, handles are released, and stale resolution results cannot override newer focus.
- [ ] #4 Foreground notification races, unknown identities and permission failures have explicit tested behavior; sustained unresolved identity is diagnosable.
- [ ] #5 Tests cover startup in an excluded game, rapid focus changes, identical basenames with path rules and resolution failures; no capture/visibility heuristics are introduced.
<!-- AC:END -->
