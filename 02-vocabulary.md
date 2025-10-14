# 🔍 Vocabulary

:::{glossary}
Extension
: The end-user-installable delivery mechanism or container for {term}`plugins <plugin>`.

Core extension
: JupyterLab is composed of modular {term}`extensions <extension>`.
Core extensions are included with JupyterLab.

Plugin
: The atomic building blocks of JupyterLab architecture and JupyterLab {term}`extensions <extension>`.
Every extension includes >= 1 plugin.

Widget
: User interface elements that can be displayed by users or by JupyterLab internals.
Widgets are provided by {term}`plugins <plugin>`.

Menu bar
: Top-level menu which exposes context-specific commands.

Main area
: The central space where JupyterLab runs {term}`main area widgets <main area widget>`.

Status bar
: Displays various information about the current session, like the number of running
{term}`kernels <kernel>`.

Side panel
: Contains extra utilities, like the file browser and debugger.
The left and right side panels are just containers; objects can be moved between them freely.

[Command](https://jupyterlab.readthedocs.io/en/latest/user/commands.html)
: A behavior that can be triggered from the JupyterLab interface.
For example, from the {term}`menu bar`.

Main area widget
: A widget which opens as a tab or panel in the {term}`main area`.
For example, the launcher, the Jupyter Notebook editor, the code editor, or the terminal.
:::
