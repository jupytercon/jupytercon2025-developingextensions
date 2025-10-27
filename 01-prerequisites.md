# 🚦 Prerequisites

## A laptop!

**If you're using Windows, we highly recommend coming prepared with Windows Subsystem for
Linux pre-configured.**
This workshop uses `bash` for commands, but everything will also work with `zsh` (the
new default for MacOS)!


## [Git](https://git-scm.com/downloads) & a GitHub account!

Git will enable you to version-control your extension.

A GitHub account will enable you to share the extension you build.


### Authentication with GitHub

You'll need to be able to login to GitHub from your working environment to push your
changes.

An easy way to get started is the [gh CLI](https://github.com/cli/cli#installation) and
the `gh auth login` command.


## A code editor / IDE

**And know how to use it!**

Feel free to use your favorite.
Any command-line or graphical editor is fine!

[VSCode](https://code.visualstudio.com/) is a safe choice.
For the AI-enabled development portion, [Cursor](https://cursor.com/) is another good
choice!


## A conda CLI ([micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html) preferred)

This workshop will use conda to manage and isolate dependencies.

We will provide commands to create your environment and install the necessary
dependencies.
We will use `micromamba` for all commands, but you can substitute `conda` or `mamba` and
they should still work.

If you're using Anaconda or Miniconda,
[configure `conda-forge` as your primary channel](https://conda-forge.org/docs/user/transitioning_from_defaults/).

You may also use [pixi](https://pixi.sh/latest/), but we expect you to be
self-sufficient if you do.


## Programming experience

There is JavaScript & TypeScript involved in extension development.
If you are comfortable with Python, you will likely be comfortable with the level of
TypeScript we'll be writing.

Experience writing JavaScript, TypeScript, or programming for the browser is a bonus,
but **not required**.
