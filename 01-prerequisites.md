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

We will provide commands to create your environment and install the necessary
dependencies.

We will use `micromamba` for all commands, but you can substitute `conda` or `mamba` and
they should still work.

If you're using Anaconda or Miniconda,
[configure `conda-forge` as your primary channel](https://conda-forge.org/docs/user/transitioning_from_defaults/).
We strongly recommend
[replacing Anaconda/miniconda with Miniforge](https://conda-forge.org/docs/user/transitioning_from_defaults/#uninstalling-anaconda-and-installing-miniforge)
to avoid complications and the possibility of being subject to Anaconda's terms of
service.


## Something to manage Python dependencies

**And know how to use it!**

We will provide commands, but you should be comfortable with the basics like creating
and activating virtual environments.

For example, [pip](https://pip.pypa.io/en/stable/), [uv](https://docs.astral.sh/uv/),
[conda/mamba](https://conda-forge.org/download/), or
[pixi](https://pixi.sh/latest/).

If you're using conda, please
[configure `conda-forge` as your primary channel](https://conda-forge.org/docs/user/transitioning_from_defaults/).
We recommend [replacing Anaconda/miniconda with Miniforge](https://conda-forge.org/docs/user/transitioning_from_defaults/#uninstalling-anaconda-and-installing-miniforge)
to avoid complications.


## Programming experience

There is a lot of JavaScript & TypeScript involved in extension development.
If you are comfortable with Python, you will likely be comfortable with the level of
TypeScript we'll be using.

Experience writing JavaScript, TypeScript, or programming for the browser is a bonus,
but not required.
