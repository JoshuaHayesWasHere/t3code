<!-- Points Claude at AGENTS.md via import. AGENTS.md is upstream's; the fork's own notes go below so upstream syncs stay clean. -->

@AGENTS.md

## This fork

This checkout is the owner's fork of pingdotgg/t3code: `origin` is JoshuaHayesWasHere/t3code and `upstream` is pingdotgg. `main` is Maria's redesign stack (`upstream/maria/redesign/6-workspace-panes`) with the fork's own commits on top, and the `fork-base` branch marks the upstream commit it sits on. [fork/README.md](fork/README.md) explains the `t3-fork` tool, the themes and the package.

- Leave AGENTS.md as upstream has it. Every upstream sync rebases the fork's commits onto upstream, so an edit there conflicts each time. Fork-only guidance goes in this section.
- PRs go to the fork: pass `--repo JoshuaHayesWasHere/t3code` to every gh command and base on `main`. gh has no default repo in this checkout and the fork's parent is pingdotgg, so without `--repo` a PR can land upstream. Open nothing on pingdotgg/t3code unless the owner asks for an upstream PR by name.
- Branch new work from a fresh `origin/main`. `t3-fork sync` rebases `main` onto upstream and force-pushes it, so hashes change; rebase an open PR branch onto the new `origin/main` instead of merging.
- Work in a worktree and leave `~/repos/t3-fork` on `main` with nothing uncommitted. `t3-fork sync`, `build` and `apply` refuse to run otherwise.
- Done means installed, not merged. The running T3 Code is the `t3code-fork` pacman package, and a merged PR reaches it only through a rebuild, so carry every merge through to an install:
  1. Run `uv run --project ~/repos/t3-fork/fork t3-fork build` in the background. It fast-forwards `main` to the merged PRs, rebases onto upstream, force-pushes, runs the fork's tests, builds, and prints the package path.
  2. Get the owner's sudo password with `request_secret` (`delivery: "file"`), then `sudo -S -p '' pacman -U --noconfirm <package> < "$file"`. Do not run `t3-fork apply`: it calls bare `sudo`, which cannot read a password in an agent shell.
  3. Confirm the new version with `t3-fork status`, then tell the owner to restart T3 Code. Never restart it yourself: you are running inside it.
- To see where things stand, run `uv run --project ~/repos/t3-fork/fork t3-fork status` (read-only apart from a fetch): upstream's head, the fork's commit count and base, and the installed package version. The `t3-fork` alias lives in the owner's zshrc, which agent shells skip.
- `themes/brave-kitty` is a submodule of JoshuaHayesWasHere/t3-themes. Change the theme in that repo; `t3-fork apply` picks up its latest `main`.
- On the fork, GitHub Actions runs Agent Context Budget and Windows Tests but not CI, so `vp run check:changed` before you push is the main gate.
