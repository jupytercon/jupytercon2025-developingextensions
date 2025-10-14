# 🔍 Vocabulary

:::{glossary}
Extension
: The end-user-installable delivery mechanism or container for {term}`plugins <plugin>`.
An extension is a collection of 1 or more {term}`plugins <plugin>`.

Core extension
: JupyterLab is composed of modular {term}`extensions <extension>`.
Core extensions are included with JupyterLab.

Plugin
: The atomic building blocks of JupyterLab architecture and JupyterLab {term}`extensions <extension>`.
Every extension includes >= 1 plugin.
In a technical sense, a plugin is a JavaScript object with certain metadata fields.

Widget
: User interface elements that can be displayed by users or by JupyterLab internals.
Widgets are provided by {term}`plugins <plugin>`.

Kernel
: A computational engine that executes code in a specific programming language.
It runs on the server, maintains runtime state (variables, imports, etc.), and returns
results back to the frontend.

[Token](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html#tokens)
: A JavaScript singleton provided by and uniquely identifying a plugin (using the
`provides` metadata field on the {term}`plugin <plugin>` object), and can be depended on
by other plugins (using the `requires` or `optional` metadata fields
on the {term}`plugin <plugin>` object).

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
