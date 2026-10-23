---
id: TASK-9
title: Validate the MVP on Windows hardware and games
status: To Do
assignee: []
created_date: '2026-10-23T08:17:00Z'
labels: []
dependencies:
  - TASK-8
documentation:
  - docs/design.md
type: task
ordinal: 9000
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Source review and synthetic input cannot establish physical crossing quality, game compatibility or resource use. Collect acceptance evidence before describing the utility as reliable. Missing hardware evidence is a remaining limitation, not a passing result.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 A manual checklist records topology, dimensions, scaling, orientation and tested scenarios without committing unnecessary personal identifiers.
- [ ] #2 Mixed-density shared edges, stacked/portrait layouts, partial spans, slow/fast/diagonal motion and dragging are exercised; unavailable scenarios are named.
- [ ] #3 Excluded game startup, menus, Alt-Tab and available overlays/elevated-window cases confirm bypass behavior and limitations.
- [ ] #4 Hotplug, rotation/resolution changes, sleep, lock/unlock, emergency disable and termination are exercised.
- [ ] #5 Idle and active CPU/memory are measured with recorded methods; high-rate mouse behavior is checked where hardware is available.
- [ ] #6 Operating documentation describes configuration, recovery, exclusions and known limitations; unresolved failures become backlog items rather than overstated completion.
<!-- AC:END -->
