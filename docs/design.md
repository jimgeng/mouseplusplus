# Design and research

Status: agreed MVP direction; runtime architecture remains a proposal until the
Windows feasibility experiment. This record is not evidence of working software.
Research inspected source and Microsoft documentation; no upstream binaries, hook
runtime tests or performance benchmarks were run. Local project was a Rust starter.

## Agreed product scope

- Windows 11 only, normal unelevated per-user background process; minimal idle work.
- Correct monitor crossings using physical dimensions/positions, including portrait
  and stacked displays. Leave normal within-monitor Windows pointer behavior alone.
- Discover available display information automatically. An editable configuration
  supplies physical positions and overrides missing/incorrect dimensions. No MVP GUI.
- Rectangles represent physical active display areas. Cross only shared edges with
  overlapping spans; block unmatched sections. Omit bezel gaps in the MVP configuration.
  Physical gaps, wraparound and corner-only connections are not supported crossings.
- While an explicitly excluded executable owns the foreground window, disable all
  correction, including game menus. Resume with fresh tracking when focus leaves it.
  Being merely running is not sufficient. No capture heuristics are required for MVP.
- A later investigation may resume correction when a still-focused game releases
  cursor control. A later GUI may allow dragging monitors. Neither is MVP scope.
- Reliability, readability and reviewable increments outrank upstream feature parity.

## Verified upstream findings

Source: [mgth/LittleBigMouse](https://github.com/mgth/LittleBigMouse), pinned at
[`2b8946aec72c4030bf6d5dcb76a35fc038bcbf10`](https://github.com/mgth/LittleBigMouse/commit/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10).
Never visit `littlebigmouse.com`: the user flags it as malicious.

Links below use paths relative to that pinned GitHub tree:

- [Rust workspace](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/Cargo.toml):
  substantial implemented geometry, traversal, hook, discovery and agent crates.
  It is not just a planned rewrite; a C# frontend still coexists and the newer Rust
  resident-agent integration is transitional. The engine identifies its historical
  runtime lineage as C++, not C#.
- [Zone transforms](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-zones/src/zone.rs):
  separate desktop pixel and physical millimetre rectangles; `+0.5` pixel-centre
  convention and independent horizontal/vertical scales.
- [Engine](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-engine/src/lib.rs):
  straight crossings map the tangential coordinate through precomputed side links,
  landing on a valid destination edge pixel. A second algorithm intersects a physical
  movement line with candidate monitor interiors. Missing targets confine the cursor.
  Both horizontal and vertical crossings exist. Exact landing thresholds matter:
  treating the landing column itself as an exit causes reverse-crossing/oscillation.
- [Layout links](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-layout/src/zoning/mod.rs):
  selects destinations within configurable travel distance (default 200 mm), so
  upstream can traverse gaps. Our shared-edge-only policy is a deliberate simplification
  confirmed with the user after research, not a claim of identical upstream behavior.
- [Windows hook](https://github.com/mgth/LittleBigMouse/tree/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-hook/src/hook/windows):
  `WH_MOUSE_LL` and dedicated message pump; handles movement, not buttons/wheel;
  suppresses handled moves and catches panics at callback boundaries.
- [Cursor backend](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-hook/src/platform/windows/cursor.rs):
  `SetCursorPos` plus managed `ClipCursor` confinement, including travel rectangles.
  Therefore a reposition-only alternative is not already proven by upstream.
- [Hook policy](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-hook/src/hook/mod.rs)
  and [matching](https://github.com/mgth/LittleBigMouse/blob/2b8946aec72c4030bf6d5dcb76a35fc038bcbf10/rust/crates/lbm-hook/src/shared.rs):
  exclusions pause hooking and are checked at startup. Upstream matching is
  case-sensitive substring/wildcard matching, not our proposed exact matching.
- Engine capture/freelook heuristics use hidden cursor or external clipping, ignoring
  LBM's own clip. They do not establish universal Raw Input/DirectInput detection.
- Safety includes nonblocking hot-path locking, ownership-aware clip restoration,
  rescue shortcut and hook watchdog. Display changes stop hooking; a controller must
  supply a valid layout to resume. Some cursor API errors are ignored upstream;
  we should not inherit assumed success after a failed call.

## Proposed minimal architecture

One executable/Cargo package, ordinary modules: `geometry`, `display`, `config`,
`runtime`, plus CLI entry point. Do not create empty modules just to match this list.
Use the existing `windows` crate with necessary features, plus `serde`/`toml` if the
configuration task validates that choice. No async runtime or cross-process protocol.

A dedicated hook/message-pump thread owns cursor state. Slow discovery/configuration
and process resolution stay off its callbacks; a small worker may return validated
results tagged to the current layout/focus generation. No file I/O, process opening,
blocking locks, allocation-heavy work or per-move logging in the hot path.

Proposed commands: `inspect`, `validate`, `run`, `stop`. Restart-to-reload is enough
initially. An emergency-disable hotkey is required before interactive hook experiments.
Exact command/configuration shape, shutdown transport and hotkey remain task decisions.

### Discovery and configuration

- `QueryDisplayConfig(QDC_ONLY_ACTIVE_PATHS)` gives active paths/modes and target
  rotation; retry the size/query sequence on `ERROR_INSUFFICIENT_BUFFER`.
- `DisplayConfigGetDeviceInfo` gives source/view names and target monitor device paths.
  Join GDI view names to `MONITORINFOEX`; do not equate an HMONITOR with persistent identity.
- `EnumDisplayMonitors` / `GetMonitorInfoW` supply desktop pixel rectangles.
- Follow the target monitor device interface through SetupAPI; validate EDID-derived
  dimensions/identifiers. Do not parse a symbolic path into a guessed registry path.
  A raw registry EDID value is not guaranteed on every device and may not represent
  the complete effective EDID when overrides exist. WMI is an alternative, not an
  additional mandatory runtime subsystem.
- Report size provenance and missing/invalid data, support millimetre overrides, and
  account for rotation without rotating dimensions twice. Windows scaling is not
  panel density; `GetDeviceCaps(HORZSIZE)` is not authoritative physical measurement.
- Use device information and EDID identifiers where available. Identical/serial-less
  displays and changing dock paths can be ambiguous; require explicit mapping rather
  than silently rebinding physical positions. Do not promise universally stable IDs.
- Validate positive finite sizes, distinct identities, nonoverlapping physical
  rectangles and unambiguous shared spans. Disable correction for an unvalidated
  topology. Initially reject mirror/clone layouts rather than choosing a panel.
- Unknown physical desk positions must be configured; EDID and Windows logical
  layout do not establish real physical offsets. Scaling metadata is diagnostic,
  not a multiplier in the correction algorithm.

### Crossing algorithm

Interior moves pass through unchanged. At an attempted exit, identify the crossed
edge, convert its tangential coordinate to millimetres, select the shared physical
span, convert to destination pixels and land inside that edge. No target means block
at the source edge. Use explicit pixel-centre and half-open-boundary conventions;
settle corner ties, adjacency tolerance and overshoot handling in geometry tests.

Do not modify button/wheel events. Preserve drag behavior. Avoid recursively processing
our own repositioning; do not assume every injected event came from this application.
Reset tracking after bypass, topology change or unexpected relocation.

**Feasibility gate:** verify `WH_MOUSE_LL` + `SetCursorPos` + movement suppression
against Windows edge clamping, disconnected logical edge spans, stacked/portrait
layouts and Windows cross-display pointer assistance. Prefer no persistent clipping.
If clipping is necessary, present evidence and a revised cleanup/recovery design for
approval before adopting it. Do not smuggle Raw Input or a driver into this scope.

### Foreground exclusions and later capture detection

Propose case-insensitive exact executable basenames, optionally exact full paths;
no substring/glob matching. Use `EVENT_SYSTEM_FOREGROUND`, initial
`GetForegroundWindow`, window-to-PID resolution and
`OpenProcess(PROCESS_QUERY_LIMITED_INFORMATION)` / `QueryFullProcessImageNameW`.
Check initial focus before correction starts. Reject stale async resolution results.

Proposal to validate: briefly defer correction on unresolved focus transitions rather
than reuse stale process identity; report persistent access failures. Resume with
fresh cursor history. An event notification plus cheap foreground-identity checking
must be evaluated for races; no claim that notifications eliminate every transition.

Later signals have limitations:
- `GetClipCursor` exposes confinement, not its owner or intent.
- `GetCursorInfo` exposes visibility; hidden does not necessarily mean a game.
- `GetCapture` only covers the calling thread; `GetGUIThreadInfo` can inspect foreground
  GUI capture, but capture is not equivalent to relative-input ownership.
- Reposition patterns are heuristics. No verified universal game-capture API exists
  for this design. Fullscreen state alone is insufficient; anti-cheat compatibility
  needs actual game testing. Do not infer compatibility from source inspection.

### Safety, lifecycle and performance

Pass through or disable correction on errors/invalid state; never keep swallowing
input after a failed reposition. Check Win32 results and contain panics at FFI boundaries.
Set per-monitor awareness before display-dependent setup; hook positions are already
per-monitor-aware screen coordinates, not values to multiply by scaling percentage.

Handle display changes, suspend/resume, lock/unlock, session/desktop transitions and
shutdown. Pause first, discard stale history, rediscover/validate, then resume only on
an eligible desktop. No secure-desktop support or mandatory elevation. Single instance
and emergency disable are required. Registration/permission failures must be visible.

Windows may silently remove a slow low-level hook. Keep callbacks small and investigate
a low-frequency health check; polling is not proof of perfect removal detection.
If clipping is ever added, restore only an identifiable application-owned clip; never
blindly clear a newer game clip. Panic catching does not guarantee abort/crash cleanup.

Prefer event-driven idle behavior, bounded diagnostics and no per-event logging. Measure
CPU/memory rather than declaring arbitrary budgets already met. Protect personal paths,
process lists, monitor serials and diagnostics from accidental commits.

## Testing and delivery

Automated: pure geometry/rounding properties, all four edges, negative coordinates,
mixed densities, rotations, partial spans, blocked sections, corners, fast/diagonal
moves, landing stability and reversals. Test configuration/EDID validation, ambiguous
identity, exclusion transitions, stale results and API failures with fakes; no live
cursor movement in ordinary tests. Run fmt, tests and clippy on changed Rust.

Manual Windows: mixed resolution/scaling; stacked/portrait arrangements; slow/fast
crossings and dragging; high-rate mice where available; game startup already focused,
Alt-Tab/overlays/elevated windows; hotplug, mode/rotation changes, sleep, lock/unlock,
emergency disable and process termination. Record actual topology, outcomes and
CPU/memory measurements. Automated/synthetic checks do not replace this evidence.

Milestones (Backlog.md owns task status and implementation plans):
1. Read-only discovery, dimensions/provenance and inspect output.
2. Config validation and pure tested geometry.
3. Bounded Windows hook feasibility experiment and backend decision.
4. MVP integration, foreground exclusions and safety/lifecycle handling.
5. Hardware/game acceptance, performance and operating documentation.

Later tasks may need splitting as interfaces become concrete. No custom DAG machinery
or agent orchestration is required; dependencies simply record real prerequisites.

## Open decisions and excluded features

Before incorporation of upstream implementation, resolve source reuse and licensing:
repository LICENSE is GPLv3 and Rust workspace declares `GPL-3.0-only`. User permits
upstream-compatible reuse, but no project license or copied source is selected yet.
Independent implementation and adapting upstream have different obligations.

Still to validate: cursor backend/clamping, monitor identity fallbacks, EDID coverage,
config/command shape, hook health recovery, focus-resolution failures and edge ties.
Material departures from agreed scope require user approval.

Excluded from MVP: GUI/tray, physical-gap traversal, wraparound, resistance, speed
normalization, layout solvers, automatic physical placement, profiles, Linux, services,
drivers, automatic elevation, automatic updates and same-game capture-release heuristics.

## Microsoft references

- [QueryDisplayConfig](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-querydisplayconfig)
- [Target device name](https://learn.microsoft.com/en-us/windows/win32/api/wingdi/ns-wingdi-displayconfig_target_device_name)
- [SetupAPI device interface](https://learn.microsoft.com/en-us/windows/win32/api/setupapi/nf-setupapi-setupdiopendeviceinterfacew)
  and [device registry key](https://learn.microsoft.com/en-us/windows/win32/api/setupapi/nf-setupapi-setupdiopendevregkey)
- [EDID overrides](https://learn.microsoft.com/en-us/windows-hardware/drivers/display/overriding-monitor-edids)
  and [reported WMI sizes](https://learn.microsoft.com/en-us/windows/win32/wmicoreprov/wmimonitorbasicdisplayparams)
- [GetDeviceCaps caveats](https://learn.microsoft.com/en-us/windows/win32/api/wingdi/nf-wingdi-getdevicecaps)
- [LowLevelMouseProc and timeout](https://learn.microsoft.com/en-us/windows/win32/winmsg/lowlevelmouseproc)
  and [hook coordinates/flags](https://learn.microsoft.com/en-us/windows/win32/api/winuser/ns-winuser-msllhookstruct)
- [SetCursorPos desktop/confinement requirements](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setcursorpos)
- [Foreground event hook](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwineventhook)
  and [process image query](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-queryfullprocessimagenamew)
- [GetCapture limitations](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getcapture),
  [GetGUIThreadInfo](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getguithreadinfo),
  [GetClipCursor](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getclipcursor)
