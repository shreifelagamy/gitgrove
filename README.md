# GitGrove

A fast, full-featured graphical Git client for VS Code: commit graph, line-level staging,
branches/tags/remotes/stashes sidebar, interactive rebase, conflict resolution, blame and history —
all inside the editor.

> 🚧 Early development. Built step by step as a learning project in high-performance VS Code
> extension development.

## Roadmap

| Stage | Scope |
|---|---|
| M0 Foundations | Project setup, lazy activation, `GitService` (git CLI, streaming, cancellation), sidebar container |
| M1 Sidebar | Branches, remotes, tags, stashes tree; checkout/create/delete/rename; fetch/pull/push |
| M2 Commit graph | Paginated, virtualized commit graph webview with search and commit details |
| M3 Changes & commit | Stage/unstage files, hunks and lines; commit, amend, discard |
| M4 History & blame | File history, inline blame |
| M5 Power tools | Interactive rebase, merge conflicts, cherry-pick, revert, reset, stashes |

## Development

Specs and design decisions live in [`openspec/`](openspec/) (spec-driven development with
[OpenSpec](https://github.com/Fission-AI/OpenSpec)).

Build instructions will be added once the project is scaffolded (M0).

## License

[MIT](LICENSE) © Shreif Ashraf. Third-party content below keeps its own license.

## Third-party content

`.claude/skills/vscode-extension-guide` is © aktsmm, licensed CC BY-NC-SA 4.0 (see its
`LICENSE.txt`). Other skills in `.claude/skills/` come from their respective upstream repositories.
