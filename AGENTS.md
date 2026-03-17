# AGENTS.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Overview

A collection of shell scripts that extend `git` with interactive, fzf-powered utilities. Scripts are designed to be added directly to `$PATH` and optionally aliased as git subcommands.

**Dependency:** All tools require [`fzf`](https://github.com/junegunn/fzf).

## Tools

### `branches`
Interactive branch switcher. Lists branches sorted by most recent commit, marks the current branch with `*`, shows a `git log` preview, and checks out the selected branch.

### `fix`
Interactive fixup commit. Lists last 500 commits (no merges) in fzf with a `git show` preview, then runs `git commit --fixup <hash>`. Passes through extra args (e.g. `-m "message"`).

### `commit-ai-coauthor`
Interactive commit helper that appends AI co-authorship trailers. Flow:
1. Checks for staged changes (exits with "no changes added to commit" if none — same as `git commit`)
2. If no TTY, falls back to plain `git commit [-m "..."]`
3. fzf: select AI co-author from `ai-config`
4. fzf: select or type a model
5. Prompts for LLM contribution percentage (default: 50%)
6. Opens `$EDITOR` for commit message (or uses `-m "..."`)
7. Commits with `Co-authored-by`, `Assistant-model`, and `LLM-Contrib` trailers via `git interpret-trailers`

Options: `-m "message"` (skip editor), `--last-used` (skip AI/model selection, use last-used), `--help` (show usage).
With `-m "..."` and `--last-used`, the command is fully non-interactive.

Logs each AI-attributed commit to `~/.config/git-tools/commit-ai-coauthor.log` (tab-separated: timestamp, ai_name, model, commit_hash). Reads the last line to pre-select the previous AI and model in fzf.

### `ai-config`
Shared data file (not a script). Format per non-comment line:
```
Name|email|default_model|model1,model2,...
```
User overrides go in `~/.config/git-tools/ai-config` (XDG-aware). The bundled file is the fallback. Both files are read by `commit-ai-coauthor` via the `read_ai_config()` function.

## Development Notes

- All scripts use `#!/usr/bin/env bash` with `set -euo pipefail`.
- Scripts resolve their own directory via `BASH_SOURCE[0]` + `command -v` fallback (handles PATH invocation).
- There is no build step, test suite, or package manager — scripts are run directly.
- To test changes, invoke the scripts directly or via `git b` / `git ac` aliases after adding the repo to `$PATH`.

## Installation (for reference)

```sh
export PATH="$HOME/src/git-tools:$PATH"  # in ~/.zshrc

git config --global alias.b '!branches'
git config --global alias.ac '!commit-ai-coauthor'
git config --global alias.fix '!fix'
```
