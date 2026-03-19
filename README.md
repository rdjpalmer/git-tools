# git-tools

A collection of custom git utilities.

## Installation

Clone the repo and add it to your PATH:

```sh
git clone git@github.com:rdjpalmer/git-tools.git ~/src/git-tools
```

Add the following to your `~/.zshrc` (or `~/.bashrc`):

```sh
export PATH="$HOME/src/git-tools:$PATH"
```

Then reload your shell:

```sh
source ~/.zshrc
```

## Setting up git aliases

You can alias each tool as a git subcommand. Add these to your `~/.gitconfig` (or run the commands below):

```sh
git config --global alias.b '!branches'
git config --global alias.ac '!commit-ai-coauthor'
git config --global alias.fix '!fix'
git config --global alias.sm '!spice-merge'
```

Then use with:

```sh
git b
git ac
git ac -m "fix: resolve auth bug"
git fix
git sm --dry-run
```

## Tools

### branches

Interactively switch branches, sorted by most recently committed. Uses fzf with a commit log preview. The current branch is marked with `*`.

**Requires:** [fzf](https://github.com/junegunn/fzf)

### fix

Interactively create a fixup commit. Uses fzf to pick from the last 500 commits (no merges), then runs `git commit --fixup <hash>`. Passes through any extra args (e.g. `git fix -m "message"`).

**Requires:** [fzf](https://github.com/junegunn/fzf)

### commit-ai-coauthor

Interactively create commits with AI co-author attribution. Uses fzf to select an AI assistant (Claude, Cursor, Copilot, etc.), then pick a model from popular options filtered by co-author (or type custom). Optionally add contribution metadata, then commits with proper `Co-authored-by` and trailer format.

**Requires:** [fzf](https://github.com/junegunn/fzf)

**Flags:**

| Flag | Description |
|------|-------------|
| `-m "message"` | Use the given commit message (skips editor) |
| `--contrib N` / `-c N` | LLM contribution percentage (e.g. `50`, `75%`, `100`). Skips the prompt. |
| `--last-used` | Skip AI and model selection; use the last-used AI co-author and model |
| `--help` / `-h` | Show usage and exit |

All other arguments (e.g. `--amend`, `--no-verify`, `-v`) are passed through to `git commit`. With `-m` and `--last-used`, the command is fully non-interactive.

**Configuration:** AI config is read from `~/.config/git-tools/ai-config` (overrides the bundled `ai-config` in the repo). Format: `Name|email|default_model|model1,model2,...` — edit to add AIs, change models, or update emails. The bundled config is shared across tools.

**Log and memory:** Each AI-attributed commit is appended to `~/.config/git-tools/commit-ai-coauthor.log` (tab-separated: timestamp, ai_name, model, commit_hash). The last-used AI and model are remembered and pre-selected in fzf on the next run.

### spice-merge

Sequentially merge a stack of PRs managed by [git-spice](https://github.com/abhinav/git-spice). Works like a local merge queue: merges PRs bottom-up through the stack, rebasing, pushing, and waiting for CI between each merge. Re-running after a failure picks up from the next unmerged PR.

**Requires:** [jq](https://github.com/jqlang/jq), [git-spice (gs)](https://github.com/abhinav/git-spice), [GitHub CLI (gh)](https://cli.github.com/)

**Flags:**

| Flag | Description |
|------|-------------|
| `--stack` | Merge the entire stack (bottom to top) |
| `--current` | Merge up to and including the current branch (default) |
| `--branch NAME` | Merge up to and including the named branch |
| `--pr NUMBER` | Merge up to and including the given PR number |
| `--squash` | Squash and merge (passed to `gh pr merge`) |
| `--merge` | Create a merge commit (passed to `gh pr merge`) |
| `--rebase` | Rebase and merge (passed to `gh pr merge`) |
| `--dry-run` | Show the merge plan without executing |
| `--help` / `-h` | Show usage and exit |
