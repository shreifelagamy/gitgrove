# Git Studio — VS Code extension

A VS Code extension that gives a full graphical Git client inside the editor, modeled on the
workflow and UX of the Fork desktop app (https://git-fork.com): commit graph, staging with
hunk/line selection, branches/tags/remotes/stashes sidebar, interactive rebase, merge-conflict
resolution, blame and file history.

"Inspired by Fork" means matching its layout, interactions and actions. Do not copy Fork's name,
logo, icons, or other bundled assets; use VS Code codicons and our own styling.

## How we work on this project

This is a **learning project**. The owner wants to learn how to build a high-performance VS Code
extension, step by step.

- **Agree before building.** Never start a feature without an agreed OpenSpec change. Propose
  first, wait for approval, then implement.
- **Teach while building.** For every step, explain *what* we are doing, *why* (the VS Code API or
  performance reason), and what the alternatives were. Point to the relevant VS Code API docs.
- **Small steps.** One concept per step (e.g. "activation events", "TreeDataProvider",
  "webview messaging"). Each step ends in something runnable (F5 Extension Development Host).
- Keep explanations in chat; keep durable decisions in OpenSpec `design.md` files.

## Spec-driven workflow (OpenSpec)

- `openspec/specs/` — current truth: one spec per capability.
- `openspec/changes/<change>/` — proposals in flight (`proposal.md`, `design.md`, `tasks.md`, delta specs).
- Flow: `/openspec-explore` (optional) → `/openspec-propose <name>` → user approval →
  `/openspec-apply-change` → `/openspec-archive-change`.
- Tasks in `tasks.md` are checked off as they are implemented.

## Researching Fork's behavior (rea)

The `reverse-engineer-anything` skill / `rea` MCP server is installed for studying how Fork
behaves (which git commands an action runs, UI states, edge cases). Fork is installed at
`/Applications/Fork.app` (native macOS app, so decompilation needs Hopper or Ghidra).

- Use it to understand **behavior** (git invocations, flows, options), then write our own
  implementation. Never copy decompiled code or extract Fork's assets into this repo.
- Prefer cheap evidence first: Fork's UI itself, its docs/release notes, and tracing the git
  commands it runs — before decompiling.
- Record findings that shape a feature in that change's `design.md`.

## Architecture principles (performance first)

- **Lazy activation.** Precise `activationEvents` (e.g. `workspaceContains:.git`, view/command
  events). Nothing heavy in `activate()`; defer work until a view is visible.
- **Bundle** the extension host code (esbuild) into one file; keep `.vscodeignore` tight.
- **Git access** through a single `GitService` layer that spawns the `git` CLI
  (`child_process.spawn`, streaming stdout, `--no-pager`, machine-readable formats like
  `-z` / `--format`). Never shell-interpolate user input; pass args as arrays.
  Reuse the built-in `vscode.git` extension API where it already gives what we need.
- **Never block the extension host.** Stream and parse large outputs incrementally; support
  cancellation (`CancellationToken`) for every long-running git call; debounce file-watcher refreshes.
- **Webviews** (commit graph, diff/rebase UIs): virtualized lists, paginate commits
  (load N at a time), message passing with small typed payloads, strict CSP with nonce,
  `retainContextWhenHidden` only when justified. Theme with VS Code CSS variables.
- **Native UI first.** Use TreeViews, QuickPicks, SCM API and diff editors where they fit;
  use webviews only where native UI cannot do the job (graph, interactive rebase).
- Dispose everything: push every disposable into `context.subscriptions`.

## Tech stack (to be confirmed in the first change)

TypeScript (strict), esbuild, `@vscode/test-cli` + `@vscode/test-electron` for integration tests,
`@vscode/vsce` for packaging. Webview UI framework is decided in its own change.

## Commands

Not scaffolded yet. Fill this in once the project is generated (build, watch, test, package).

## Skills available in this repo

`.claude/skills/`: `vscode-extension-guide` (broad guide: webviews, tree views, testing,
publishing), `vscode-extension-expert` (manifest, activation, CSP, performance),
`vscode-ext-commands`, `vscode-ext-localization`. Consult them when working in those areas.
