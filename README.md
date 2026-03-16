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
```

Then use with:

```sh
git b
```

## Tools

### branches

Interactively switch branches, sorted by most recently committed. Uses fzf with a commit log preview. The current branch is marked with `*`.
