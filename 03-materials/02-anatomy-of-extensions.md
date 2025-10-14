# 🧬 Anatomy of an extension

:::{hint} Learning objectives
* Understand the difference between extensions, plugins, and widgets
* Learn how to get started with writing an extension
* ...
:::

:::{important} Outcomes
After completing this module, you will have created a new extension from the
[official template](https://github.com/jupyterlab/extension-template) and ... 🏗️ TODO ...
:::

:::{tip} Terms
{term}`extension`, {term}`plugin`, {term}`widget`
:::


## Extensions, plugins, widgets?

An {term}`extension` and a {term}`plugin` sound like the same thing at first.
The important thing to understand is that plugins are functionality, and extensions are
the package around that functionality.
Plugins are the fundamental building block of the JupyterLab architecture, and
extensions are the delivery mechanism or "container" for those building blocks.
Extensions are the thing you `pip install`, and they often contain more than one plugin.

A {term}`widget` is a user interface component provided by a plugin, either for use by
the end user to display (e.g. an interactive visualization of data) or for JupyterLab to
display (e.g. a document viewer that opens when you double-click a particular file
type).

```{mermaid}
graph TB

    subgraph Extension["Extension"]
        subgraph Plugin["Plugin(s) (n>=1)"]
            Widget["Widget(s) (n>=0)"]
        end
    end

    style Widget stroke-dasharray: 5 5
```


## Exercise A: Create an extension from the [official template](https://github.com/jupyterlab/extension-template)

:::{important} Prerequisites
* `copier`. Install with any of the following options:
    * Into an existing environment (don't forget to activate it first):
        * `pip install "copier~=9.2" jinja2-time`
        * `conda install -c  conda-forge "copier~=9.2" jinja2-time`
    * Globally:
        * `uv tool install --with jinja2-time copier~=9.2`
        * `pixi global install "copier~=9.2" --with jinja2-time`
:::


0. Change to the parent directory where you want to work, e.g.

    ```bash
    cd ~/Projects
    ```

1. Create an extension repo in a new directory:

    ```bash
    mkdir my-jupyterlab-extension
    cd my-jupyterlab-extension
    git init
    ```

2. Instantiate the template to get started on your new extension!

    ```bash
    copier copy --trust https://github.com/jupyterlab/extension-template .
    ```

3. Install it in development mode

    ```bash
    # Create and activate a virtualenv
    uv venv .venv
    source .venv/bin/activate

    # Install package in development mode
    uv pip install -e ".[test,dev]"
    # Link your development version of the extension with JupyterLab
    # TODO: This errors unless the user has a global JupyterLab install
    jupyter labextension develop . --overwrite
    # Server extension must be manually installed in develop mode
    jupyter server extension enable myextension
    # Rebuild extension Typescript source after making changes
    jlpm build
    ```

4. 🧪 Test it out!

    ```bash
    jupyter lab
    ```

5. Confirm the extension was loaded. Open your browser's dev console (F12 or
   `CTRL+SHIFT+I`) and look for log messages reading:

   * `JupyterLab extension myextension is activated`
   * `This is /myextension/get-example endpoint!`
