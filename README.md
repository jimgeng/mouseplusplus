# mouseplusplus

A planned minimal Windows 11 alternative to LittleBigMouse: physical monitor-edge
correction with explicit foreground-process exclusions. Currently a Rust starter;
**cursor correction is not implemented**.

## Project map

- [Design and research](docs/design.md): scope, source evidence and feasibility gates.
- [Agent instructions](AGENTS.md): short project-specific working rules.
- `backlog/tasks/`: shared Backlog.md work items and acceptance criteria.
- `backlog/drafts/`: tentative post-MVP ideas, not committed implementation scope.

## Backlog

Use Backlog.md **1.55.2**, verified during bootstrap. With Node.js/npm available,
these commands need no global install or project `package.json`:

```text
npx --yes backlog.md@1.55.2 task list --ready --plain
npx --yes backlog.md@1.55.2 task view TASK-1 --plain
npx --yes backlog.md@1.55.2 board
npx --yes backlog.md@1.55.2 instructions overview
```

The first invocation downloads the tool into npm's cache. Alternatively install
`npm install --global backlog.md@1.55.2` and use `backlog` for the same commands.
Do not use `npx backlog`: that names an unrelated package. Tool updates should be
intentional; update these instructions and `AGENTS.md` together after validation.

Tasks track outcomes, prerequisites and verification, not agent assignments or
orchestration. Add or revise work as evidence changes; keep near-term tasks detailed
and future work coarse. Record implementation plans when starting a task, not months
in advance. Use the CLI to preserve task metadata. Routine backlog maintenance does
not authorize new product scope or resolve open architectural decisions.

Shared tasks and `backlog/config.yml` belong in Git. Automatic commits, remote Git
operations and cross-branch scanning are disabled. No MCP connector, agent plugin,
status-triggered automation or background task server is required. Changes remain
uncommitted for review unless a commit is requested.

## Rust development

Use a Windows Rust toolchain; the package currently uses edition 2024. Backlog.md is
a development tool, not an application/runtime dependency.

```text
cargo fmt --check
cargo test
cargo clippy --all-targets -- -D warnings
```

Passing these checks does not establish cursor behavior on actual displays or games.
Hardware acceptance evidence will be recorded separately. Personal monitor geometry,
process lists and raw machine diagnostics should not be committed; their file location
will be settled with the configuration task. No application license has been selected
yet; see the upstream reuse/licensing gate in the design record.
