# 🔍 Vocabulary

:::{glossary}
Extension
: The end-user-installable delivery mechanism or container for {term}`plugins <plugin>`.
An extension is a collection of 1 or more {term}`plugins <plugin>`.
It's very common for {term}`frontend extensions <frontend extension>` and
{term}`server extensions <server extension>` to be bundled together as one extension
(in fact, we'll do that as part of this workshop).
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html#overview-of-extensions)

Core extension
: JupyterLab is composed of modular {term}`extensions <extension>`.
Core extensions are included with JupyterLab.

Server extension
: An extension which provides new functionality on the back-end.
Unlike frontend extensions, server extensions have access to the computer JupyterLab is
running on, for example to read files from disk.
[See the official docs for more!](https://jupyter-server.readthedocs.io/en/stable/developers/extensions.html)

MIME renderer extension
: An extension which teaches JupyterLab how to view a new type of file.
For example,
[jupyterlab-geojson](https://github.com/jupyterlab/jupyter-renderers/tree/main/packages/geojson-extension)
enables JupyterLab to open a "slippy map" to display geospatial vector data in the
GeoJSON format.
Also known as a "mimetype" extension.

Frontend extension
: An extension which modifies the user interface and/or adds functionality that executes
solely in the browser, and interacts with existing JupyterLab frontend components.
This includes themes, UI elements, viewers, code editors, and more.
They are built with [Lumino](https://lumino.readthedocs.io/en/latest/) and/or
[React](https://react.dev/learn) components.
Many frontend extensions are part of a larger extension with a
{term}`server extension <server extension>` component.

Plugin
: The atomic building blocks of JupyterLab architecture and JupyterLab {term}`extensions <extension>`.
Every extension includes >= 1 plugin.
In a technical sense, a plugin is a JavaScript object with certain metadata fields.
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html#plugins)

Widget
: User interface elements that can be displayed by users or by JupyterLab internals.
Widgets are provided by {term}`plugins <plugin>`.

Kernel
: A computational engine that executes code in a specific programming language.
It runs on the server, maintains runtime state (variables, imports, etc.), and returns
results back to the frontend.

Token
: A JavaScript singleton provided by and uniquely identifying a plugin (using the
`provides` metadata field on the {term}`plugin <plugin>` object), and can be depended on
by other plugins (using the `requires` or `optional` metadata fields
on the {term}`plugin <plugin>` object).
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html#tokens)

Menu bar
: Top-level menu which exposes context-specific commands.

Main area
: The central space where JupyterLab displays {term}`main area widgets <main area widget>`,
like Notebooks.
You can recognize the main area by the tab panel.
You can click and drag the tab at the top of any {term}`main area widget <main area widget>`
to subdivide the main area arbitrarily!

Main area widget
: A widget which opens as a tab or panel in the {term}`main area`.
For example, the launcher, the Jupyter Notebook editor, the code editor, or the terminal.

Status bar
: Displays various information about the current session, like the number of running
{term}`kernels <kernel>`.

Side panel
: Contains extra utilities, like the file browser and debugger.
The left and right side panels are just containers; objects can be moved between them freely.

Command
: A behavior that can be triggered from the JupyterLab interface.
For example, from the {term}`menu bar`.
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/latest/user/commands.html)

Command palette
: A keyboard-driven interface for triggering {term}`commands <command>`.
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/stable/user/commands.html#command-palette)

Launcher
: When a new JupyterLab tab is opened, the launcher is displayed.
It includes categorized buttons to launch any applications that are registered with the launcher.
[See the official docs for more!](https://jupyterlab.readthedocs.io/en/latest/extension/extension_points.html#launcher)

VRAM
: Video Random Access Memory - dedicated memory on a graphics processing unit (GPU) used for storing image data, textures, and other graphics-related information.
In the context of AI development, VRAM is crucial for loading and running large language models locally.
Typical consumer GPUs have between 4GB to 24GB of VRAM, with high-end models offering more.

LLM
: Large Language Model - a type of AI model trained on vast amounts of text data to understand and generate human-like text.
When we say "AI" in this workshop, we're referring to LLMs.
LLMs can assist with coding tasks like writing code, debugging, refactoring, and explaining complex concepts.
Examples include GPT-5, Claude, and Qwen.
These models are characterized by having billions of parameters and the ability to perform diverse language tasks without task-specific training.

Agentic AI
: AI systems that can autonomously take actions like an agent, rather than just providing suggestions.
The key differentiator is **tool use**. Agentic AI can execute commands (build, test, format), read files, search codebases, and make changes directly.
These tools work in an **agentic loop**: they plan an action, use a tool to execute it, observe the result, and decide the next step, iterating until the task is complete.
Examples include Cursor, Claude Code, and Cline.
Unlike chat-based or autocomplete AI, agentic tools act like a team member with tools — they don't just suggest, they do.

LLM token
: In the context of {term}`LLMs <LLM>`, a token is a unit of text (roughly 3-4 characters or ~0.75 words in English) that the model processes.
API providers charge based on the number of tokens consumed—both input tokens (text sent to the model) and output tokens (text generated by the model).
For example, "JupyterLab extension" is approximately 4 tokens.
Token counts determine both the cost of API calls and context window limits (e.g., a model with a 1M token context can process up to 1M tokens at once).
:::
