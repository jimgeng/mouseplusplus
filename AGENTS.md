# mouseplusplus

## Context on demand
- Tasks live in `backlog/`; research, scope, decisions and open gates live in
  `docs/design.md`. Read the relevant sections, not every task or document.
- Backlog.md CLI: `npx --yes backlog.md@1.55.2 <command>` (setup: `README.md`).
  Before managing tasks, run `instructions overview`; load its action-specific
  guides as needed. Use the CLI rather than hand-editing task Markdown.
- The backlog is a human-readable work queue, not agent orchestration. Dependencies
  represent real prerequisites, not a mandatory execution schedule.
- Update/split/add tasks as discoveries warrant; keep later work coarse. Routine
  backlog maintenance is authorized, but recording work does not authorize its
  implementation. Ask before expanding product scope or changing agreed architecture.
- Work on the requested task/slice. Record its current implementation plan before
  coding; stop for user decisions at unresolved design gates. Do not start the next
  task merely because it is ready. Record evidence, not session transcripts.

## Constraints that must survive refactoring
- Windows 11 only; unelevated per-user process. No service, driver or GUI in MVP.
- MVP exclusions are explicit foreground-executable matches, not game heuristics.
- Cross only shared physical active-area edges; block unmatched sections. No gaps,
  wraparound or within-monitor speed normalization. Preserve portrait/stacked support.
- Keep geometry pure and separate from Win32 effects. Prefer one Cargo package and
  ordinary modules; no workspace/framework/async runtime without a demonstrated need.
- Do not adapt upstream source until reuse/licensing is resolved. Never visit
  `littlebigmouse.com`; the user has flagged it as malicious. Use the GitHub source.

## Readable Rust and Windows boundaries
- Prefer small, named functions with one understandable responsibility. Split API
  discovery, validation, policy and side effects rather than writing long orchestration
  functions. No arbitrary line limits or tiny wrappers that obscure control flow.
- Keep hook callbacks as thin dispatch/policy adapters: no process inspection,
  filesystem I/O, blocking locks or per-movement logging. Never unwind across Win32 FFI.
- Localize `unsafe` and document each non-obvious safety precondition at the boundary.
  Make handle ownership and cleanup explicit; propagate API failures instead of
  pretending an operation succeeded. Do not introduce a generic Win32 wrapper layer.
- Prefer explicit coordinate units/conventions over clever math or implicit casts;
  use descriptive types/names without building an elaborate units framework.
- Test geometry and state transitions without a live cursor. For changed Rust, run
  `cargo fmt --check`, `cargo test`, and `cargo clippy --all-targets -- -D warnings`.
  Report unrun checks and Windows hardware/game tests separately; unchecked evidence
  stays open in the task. Explain important architectural choices in the design record.
