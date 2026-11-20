<!-- Points Claude at AGENTS.md via import. AGENTS.md is upstream's; the fork's own notes go below so upstream syncs stay clean. -->

@AGENTS.md

## This fork

This checkout is the owner's fork of pingdotgg/t3code: `origin` is JoshuaHayesWasHere/t3code and `upstream` is pingdotgg. `main` is Maria's redesign stack (`upstream/maria/redesign/6-workspace-panes`) with the fork's own commits on top, and the `fork-base` branch marks the upstream commit it sits on. [fork/README.md](fork/README.md) explains the `t3-fork` tool, the themes and the package.

- Leave AGENTS.md as upstream has it. Every upstream sync rebases the fork's commits onto upstream, so an edit there conflicts each time. Fork-only guidance goes in this section.
- PRs go to the fork: pass `--repo JoshuaHayesWasHere/t3code` to every gh command and base on `main`. gh has no default repo in this checkout and the fork's parent is pingdotgg, so without `--repo` a PR can land upstream. Open nothing on pingdotgg/t3code unless the owner asks for an upstream PR by name.
- Branch new work from a fresh `origin/main`. `t3-fork sync` rebases `main` onto upstream and force-pushes it, so hashes change; rebase an open PR branch onto the new `origin/main` instead of merging.
- Work in a worktree and leave `~/repos/t3-fork` on `main` with nothing uncommitted. `t3-fork sync`, `build` and `apply` refuse to run otherwise.
- A merged PR does not reach the running app on its own. The installed T3 Code is the `t3code-fork` pacman package, rebuilt only when the owner runs `t3-fork apply`: it fast-forwards `main` to the merged PRs, rebases onto upstream, runs the fork's tests, builds, installs with sudo, and then T3 Code needs a restart. You are running inside that app, so do not run `apply`, `build`, `sync`, `pacman` or anything that restarts T3 Code, and do not ask whether to. Once your PR merges, say it ships with the owner's next `t3-fork apply` and restart.
- To see where things stand, run `uv run --project ~/repos/t3-fork/fork t3-fork status` (read-only apart from a fetch): upstream's head, the fork's commit count and base, and the installed package version. The `t3-fork` alias lives in the owner's zshrc, which agent shells skip.
- `themes/brave-kitty` is a submodule of JoshuaHayesWasHere/t3-themes. Change the theme in that repo; `t3-fork apply` picks up its latest `main`.
- On the fork, GitHub Actions runs Agent Context Budget and Windows Tests but not CI, so `vp run check:changed` before you push is the main gate.
