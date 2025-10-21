# 🧬 2 - Anatomy of an extension

:::{hint} Learning objectives
* Understand the difference between extensions, plugins, and widgets
* Learn how to get started with writing an extension
* ...
:::

:::{important} Outcomes
After completing this module, you will have created a new extension from the
[official template](https://github.com/jupyterlab/extension-template) and implemented a
new widget for displaying a random image and caption from a curated set.
The widget will be launchable from the {term}`command palette <command palette>` and the
{term}`launcher <launcher>`.
:::

:::{tip} Terms
{term}`extension <extension>`, {term}`plugin <plugin>`, {term}`widget <widget>`,
{term}`main area widget <main area widget>`, {term}`command <command>`,
{term}`command palette <command palette>`, {term}`launcher <launcher>`
:::

:::{note} Inspired by...
:class: dropdown

This tutorial is inspired by the official
[Astronomy picture of the day](https://jupyterlab.readthedocs.io/en/latest/extension/extension_tutorial.html)
tutorial with a few changes (some of which we plan to merge back):

* Include a server extension
* Add an interactive element (toolbar button) to refresh the image
* Use a set of public domain images from the Library of Congress (the NASA astronomy
  photo API is not reliable during the current government shutdown 😭)
* Remove unnecessary use of `conda` in favor of more standard tooling
* Structure the activities around in person teaching and exercises
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

First, let's look at
[the extension we're going to build today](https://github.com/jupytercon/jupytercon2025-developingextensions-demo).
The README in this repository describes the functionality we will build out.

First we'll look at the final extension together. It:

* Adds a new button to the launcher
* Adds a new command to the command palette
* When either of those is triggered, it opens a new tab/window with a viewer showing a
  random image and caption from a small curated collection of public domain images.
* The viewer also has a "refresh" button to trigger fetching a new image and caption.

Now, let's build it together from scratch.


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

    Be sure to select `frontend-and-server` as the "kind"!
    Everything else can be left as default.

    ![A demo of instantiating an extension project from the official template](../assets/images/init-from-template.gif)

3. List out the files that were created (`ls -la` or `tree -a` are good options)

4. Install the extension in development mode

    ```bash
    # Create and activate a virtualenv
    uv venv .venv
    source .venv/bin/activate

    # Install package in development mode
    uv pip install --editable ".[dev,test]"

    # Install the frontend and backend components of the extension in development mode:
    jupyter labextension develop . --overwrite
    jupyter server extension enable myextension

    # Rebuild extension Typescript source after making changes
    # IMPORTANT: You must do this every time you make a change!
    jlpm build
    ```

5. 🧪 Test it out! Run this command in a **separate terminal**.
   Remember to activate the virtual environment again with `source .venv/bin/activate`
   any time you create a new terminal.
   You can keep this terminal open and running JupyterLab in the background!

    ```bash
    jupyter lab
    ```

6. Confirm the extension was loaded. Open your browser's dev console (F12 or
   `CTRL+SHIFT+I`) and look for log messages reading:

   * `JupyterLab extension myextension is activated`
   * `This is /myextension/get-example endpoint!`

   **If you do not see these messages, let us know you need help!**

7. Directly test the server portion of the extension by visiting the endpoint in your
   browser (`http://localhost:8888/myextension/get-example`).
   You should see the same message as the last step:
   `This is /myextension/get-example endpoint!`


:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Initialize from extension template"
git push -u origin main
```
:::

<TODO: When should they have created a repo on GitHub?>


### Now let's do one complete development loop!

0. Close the JupyterLab server with `CTRL+C`.

1. Make any change to the codebase.
   For example, alter the text in a `console.log()` message.
   We suggest changing the server's message to `Hello, world` instead of `This is ...
   endpoint!`.
   While you're making that change, consider renaming some objects and see what code
   needs to be updated.
   Perhaps `RouteHandler` could be renamed to `HelloRouteHandler`?
   Perhaps `route_pattern` could be renamed to `hello_route_pattern` (Spoiler! We may
   have more routes later).

2. Rebuild the extension with `jlpm build`.

3. Start JupyterLab again with `jupyter lab`.

4. Test again following steps 5 & 6 above.
   Do you see the change in the console messages?
   Do you see the change when directly accessing the server with the browser?


:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Test development loop with a simple change"
git push -u origin main
```
:::


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


## Creating a widget

Our working extension is a basic "hello, world" application.
All it does is log a string to the console, then make a request to the back-end
for another string, which is also logged to the console.
This all happens once, when the extension is activated when the user opens JupyterLab.

Our goal is to display a viewer for a random photo and caption, with a refresh button to
instantly display a new image.
That viewer will be a {term}`widget`, so let's start by creating a widget that
will eventually house that content.


## Exercise B (20 minutes): Launching a "hello, world" {term}`widget <widget>`

### Create a "hello, world" widget

To display this widget in the {term}`main area <main area>`, we need to
implement a {term}`widget <widget>` which displays our content (for now, just
"Hello, world!"), and then include that content in a
{term}`main area widget <main area widget>`.

Create a new file `src/widget.ts` and add the widget code:

```{code} typescript
:linenos:

import { Widget } from '@lumino/widgets';
import { MainAreaWidget } from '@jupyterlab/apputils';

class ImageCaptionWidget extends Widget {
  // Initialization
  constructor() {
    super();

    // Create and append an HTML <p> (paragraph) tag to our widget's node in
    // the HTML document
    const hello = document.createElement('p');
    hello.innerHTML = "Hello, world!";
    this.node.appendChild(hello);
  }
}

export class ImageCaptionMainAreaWidget extends MainAreaWidget<ImageCaptionWidget> {
  constructor() {
    const content = new ImageCaptionWidget();
    super({ content });

    this.title.label = 'Random image with caption';
    this.title.caption = this.title.label;
    this.title.icon = imageIcon;
  }
}
```

We can't test this yet because we don't have a convenient way to display the
widget in JupyterLab yet.
Let's fix that now.

We'll start by creating our command to display the widget:


### Create a {term}`command <command>` to display the {term}`widget <widget>`

In `src/index.ts`, we need to update our plugin to define a command in our
{term}`plugin's <plugin` `activate` method:

```{code} typescript
:linenos:
:emphasize-lines: 2,26-38

import { requestAPI } from './request';
import { ImageCaptionMainAreaWidget } from './widget';

/**
 * Initialization data for the myextension extension.
 */
const plugin: JupyterFrontEndPlugin<void> = {
  id: 'myextension:plugin',
  description: 'A JupyterLab extension.',
  autoStart: true,
  activate: (
      app: JupyterFrontEnd,
  ) => {
    console.log('JupyterLab extension myextension is activated!');

    requestAPI<any>('hello')
      .then(data => {
        console.log(data);
      })
      .catch(reason => {
        console.error(
          `The myextension server extension appears to be missing.\n${reason}`
        );
      });

    //Register a new command:
    const command_id = 'image-caption:open';
    app.commands.addCommand(command_id, {
      execute: () => {
        // When the command is executed, create a new instance of our widget
        const widget = new ImageCaptionMainAreaWidget();

        // Then add it to the main area:
        app.shell.add(widget, 'main');
      },
      icon: imageIcon,
      label: 'View a random image & caption'
    });
  }
};

```

But right now, this command is not being used by anything!
Next, we'll add it to the {term}`command palette <command palette>`.


### Register our {term}`command <command>` with the {term}`command palette <command palette>`

First, import the command palette interface at the top of `src/index.ts`:

```{code} typescript
:linenos:
:emphasize-lines: 5

import {
  JupyterFrontEnd,
  JupyterFrontEndPlugin
} from '@jupyterlab/application';
import { ICommandPalette } from '@jupyterlab/apputils';
```

Then, add the command palette as a dependency of our plugin:

```{code} typescript
:linenos:
:emphasize-lines: 5,8-10

const plugin: JupyterFrontEndPlugin<void> = {
  id: 'myextension:plugin',
  description: 'A JupyterLab extension.',
  autoStart: true,
  requires: [ICommandPalette],  // dependencies of our extension
  activate: (
      app: JupyterFrontEnd,
      // The activation method receives dependencies in the order they are specified in
      // the "requires" parameter above:
      palette: ICommandPalette
  ) => {
```

Finally, we can use our `palette` object to register our {term}`command <command>` with
the {term}`command palette <command palette>`.

```{code} typescript
:linenos:
:emphasize-lines: 15

    //Register a new command:
    const command_id = 'image-caption:open';
    app.commands.addCommand(command_id, {
      execute: () => {
        // When the command is executed, create a new instance of our widget
        const widget = new ImageCaptionMainAreaWidget();

        // Then add it to the main area:
        app.shell.add(widget, 'main');
      },
      icon: imageIcon,
      label: 'View a random image & caption'
    });

    palette.addItem({ command: command_id, category: 'Tutorial' });
```


### Optional: Register with the {term}`launcher <launcher>`

Unlike the command palette, this functionality needs to be installed.
First, install `@jupylab/launcher` with `jlpm add @jupyterlab/launcher` to make
this dependency available for import.

You can import `ILauncher` with:

```typescript
import { ILauncher } from '@jupyterlab/launcher'
```

...and register your {term}`command` with the {term}`launcher`:

```typescript
launcher.add({ command: command_id });
```

We will leave the rest of the implementation up to you!

:::{hint}
Follow the same steps you followed to register your command with the {term}`command palette <command palette>`.
Reuse the same command!
:::


### Finally, we can test!

Stop your JupyterLab server (`CTRL+C`), then rebuild your extension (`jlpm
build`), then restart JupyterLab (`jupyter lab`).

If everything went well, now you can test the extension in your browser.

To test from the {term}`command palette <command palette>`, click
"View">"Commands" from the {term}`menu bar <menu bar>`, or use the shortcut
`CTRL+SHIFT+C`.
Begin typing "Random image" and the command palette interface
should autocomplete.
Select "Random image with caption" and press `ENTER`.
You should see a new tab open containing the text "Hello, world"!

If you registered your extension with the {term}`launcher <launcher>`, you can open a
new tab with the `+` button at the top of the {term}`main area <main area>` and click
the new button in the launcher.

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Add 'hello, world' widget and register with command palette"
git push -u origin main
```
:::


## What's next?

We've graduated from "Hello, world" in the console to "Hello, world" in a
{term}`main area widget <main area widget>`.
That's a big step, but remember our end goal: A viewer for random images and
captions.

We have all the building blocks now: a server to serve the image data from disk
with a caption, and a widget to display them.
Now we need to implement the logic and glue the pieces together.


## Exercise C: Serve images and captions from the server extension

<TODO>


## Exercise D: Add user interactivity to the widget

<TODO>


[^rebuild-not-always-required]: You don't actually _always_ need to rebuild -- only when
you change the JavaScript. If you only changed Python, you only need to restart
JupyterLab.
