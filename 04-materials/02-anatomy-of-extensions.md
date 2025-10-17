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


## Exercise A (10 minutes): Extension creation and development loop

### First, create a new extension from the [official template](https://github.com/jupyterlab/extension-template)

:::{important} Prerequisites
* `copier`. Install with any of the following options:
    * Into an existing environment (don't forget to activate it first):
        * `pip install "copier~=9.2" jinja2-time`
        * `conda install -c  conda-forge "copier~=9.2" jinja2-time`
    * Globally:
        * `pipx install "copier~=9.2" --preinstall jinja2-time`
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


💾 **Make a Git commit and push to GitHub now!**


### Now let's do one complete development loop!

0. Close the JupyterLab server with `CTRL+C`.

1. Make any change to the codebase.
   For example, alter the text in a `console.log()` message.
   I suggest changing the server's message to `Hello world` instead of `This is ...
   endpoint!`.
   While you're making that change,

2. Rebuild the extension with `jlpm build`.

3. Start JupyterLab again with `jupyter lab`.

4. Test again following steps 5 & 6 above.
   Do you see the change in the console messages?
   Do you see the change when directly accessing the server with the browser?


💾 **Make a Git commit and push to GitHub now!**

<TODO: When should they have created a repo on GitHub?>


### What just happened?

We know how to get started: we learned how to instantiate a new extension from the
official template and set it up for development.

We know how to iterate: we learned that the JupyterLab extension development loop is...

* Make a change to the code.
* Shut down JupyterLab (`CTRL+C`).
* Rebuild the extension with `jlpm build` [^rebuild-not-always-required].
* Start JupyterLab with `jupyter lab`.

Now we have all the knowledge we need to keep iterating on our extension!
🎓 Well done!


## Beyond "Hello, world"

Our working extension is a basic "hello world" application.
All it does is log a string to the console, then make a request to the back-end
for another string, which is also logged to the console.
This all happens once, when the extension is activated when the user opens JupyterLab.

We can do something more interesting than that! Let's:

* Display some content in a {term}`main area widget`
* Add a launcher button to open that widget
* Add an interactive behavior to our widget


### Widget time!

Let's create our widget in `src/widget.ts`:

```typescript
import { Widget } from '@lumino/widgets';
import { requestAPI } from './handler';

export class TutorialWidget extends Widget {
  // Initialization
  constructor() {
    super();

    // Create and append the HTML <img> tag to our widget's node in the HTML
    // document
    this.img = document.createElement('img');
    this.node.appendChild(this.img);

    // Initialize the image from the server extension
    this.load_image()
  }

  // Fetching data from the server extension
  load_image(): void {
    requestAPI<any>('image')
      .then(data => {
        console.log(data);
        this.img.src = data.image_url;
      })
      .catch(reason => {
        console.error(
          `The tutorial_extension server extension appears to be missing.\n${reason}`
        );
      });
  }

  // Information for the type checker
  img: HTMLImageElement;
}
```

[^rebuild-not-always-required]: You don't actually _always_ need to rebuild -- only when
you change the JavaScript. If you only changed Python, you just need to restart
JupyterLab.
