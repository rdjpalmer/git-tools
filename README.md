# git-tools

A collection of custom git utilities.

## Requirements

- [fzf](https://github.com/junegunn/fzf)

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
```

Then use with:

```sh
git b
git ac
git ac -m "fix: resolve auth bug"
git fix
```

## Tools

### branches

Interactively switch branches, sorted by most recently committed. Uses fzf with a commit log preview. The current branch is marked with `*`.

### fix

Interactively create a fixup commit. Uses fzf to pick from the last 500 commits (no merges), then runs `git commit --fixup <hash>`. Passes through any extra args (e.g. `git fix -m "message"`).

### commit-ai-coauthor

Interactively create commits with AI co-author attribution. Uses fzf to select an AI assistant (Claude, Cursor, Copilot, etc.), then pick a model from popular options filtered by co-author (or type custom). Optionally add contribution metadata, then commits with proper `Co-authored-by` and trailer format.

**Configuration:** AI config is read from `~/.config/git-tools/ai-config` (overrides the bundled `ai-config` in the repo). Format: `Name|email|default_model|model1,model2,...` — edit to add AIs, change models, or update emails. The bundled config is shared across tools.
