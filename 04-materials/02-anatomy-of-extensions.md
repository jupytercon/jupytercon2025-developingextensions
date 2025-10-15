# 🧬 2 - Anatomy of an extension

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
{term}`extension <extension>`, {term}`plugin <plugin>`, {term}`widget <widget>`
:::


## Extensions and plugins and widgets -- oh, my!

An {term}`extension <extension>` and a {term}`plugin <plugin>` sound like the same thing at first.
The important thing to understand is that plugins are functionality, and extensions are
the package around that functionality.

{term}`Plugins <plugin>` are the fundamental building block of the JupyterLab architecture, and
extensions are the delivery mechanism or "container" for those building blocks.
Extensions are the thing you `pip install`, and they often contain more than one plugin.

A {term}`widget <widget>` is a user interface component provided by a plugin, either for use by
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


## Now we're ready to build an extension!

First, let's look at the extension we're going to build today.
The README in this repository describes the functionality we will build out,
and we can instantly try it out with a browser-only deployment of JupyterLite.

<TODO: Link to demo extension in another repo, deployed with JupyterLite on GH Pages>

Now, let's build it from scratch.


## Exercise A (10 minutes): Create an extension from the [official template](https://github.com/jupyterlab/extension-template)

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

    # Install the frontend and backend components of the extension in development mode:
    jupyter labextension develop . --overwrite
    jupyter server extension enable myextension

    # Rebuild extension Typescript source after making changes
    # IMPORTANT: You must do this every time you make a change!
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

6. Directly test the server portion of the extension by visiting the endpoint in your
   browser (`http://localhost:8888/myextension/get-example`).
   You should see the same message as the last step:
   `This is /myextension/get-example endpoint!`


## What just happened?

We just dove head-first in to extension development by:

* Instantiating the official extension template
* Creating a virtual environment and installing the Python package in development mode
* Installing the extension into JupyterLab
* Building the extension
* Running JupyterLab
* Testing that it's working
* Making a trivial change
* Testing again


## Beyond "Hello, world"

Our working extension is a basic "hello world" application.
All it does is log a string to the console, then make a request to the back-end
for another string, which is also logged to the console.
This all happens once, when the extension is activated when the user opens JupyterLab.

We can do something more interesting than that!
