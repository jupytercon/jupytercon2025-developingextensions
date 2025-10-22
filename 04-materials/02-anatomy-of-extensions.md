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

This tutorial is inspired by many prior works.

* SciPy 2018 tutorial:
  [repo](https://github.com/jupyterlab/scipy2018-jupyterLab-tutorial)
  | [video](https://youtu.be/Gzun8PpyBCo?si=M6NWrbQvMpJX1Yjg&t=5390)
* JupyterCon 2018 tutorial:
  [repo](https://github.com/jupyterlab/jupytercon-jupyterlab-tutorial)
* SciPy 2019 tutorial:
  [repo](https://github.com/jupyterlab/scipy2019-jupyterlab-tutorial)
  | [video](https://www.youtube.com/watch?v=RFabWieskak)
* JupyterCon 2020 tutorial:
  [repo](https://github.com/jupytercon/jupytercon2020-developingextensions)
* SciPy 2022 tutorial:
  [repo](https://github.com/marthacryan/developing-extensions-tutorial)
  | [video](https://www.youtube.com/watch?v=9_-siU-_XoI)
* The official
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

🚀 Now, let's build it together from scratch.


## 🏋️ Exercise A (15 minutes): Extension creation and development loop

### Create a repository in GitHub and clone it

0. Change to the parent directory where you want to work, e.g.

    ```bash
    cd ~/Projects
    ```

1. Create a repository in GitHub and clone it

    ```bash
    # TODO: Test
    gh repo create jupytercon2025-extension-workshop --public --clone
    ```

2. Change directory into your new repository

    ```bash
    cd jupytercon2025-extension-workshop
    ```


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

2. Instantiate the template to get started on your new extension!

    ```bash
    copier copy --trust https://github.com/jupyterlab/extension-template .
    ```

    Please input:

    * Kind: `frontend-and-server`
    * Javascript package name: `jupytercon2025-extension-workshop`

    Everything else can be left as default if you prefer.

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
    jupyter server extension enable jupytercon2025_extension_workshop

    # Rebuild extension Typescript source after making changes
    # IMPORTANT: You must do this every time you make a change!
    jlpm build
    ```

5. 🧪 Test it out! Run this command in a **separate terminal**.
   It will open JupyterLab in your browser automatically.
   Remember to activate the virtual environment again with `source .venv/bin/activate`
   any time you create a new terminal.
   You can keep this terminal open and running JupyterLab in the background!

    ```bash
    jupyter lab
    ```

6. Confirm the extension was loaded. Open your browser's dev console (F12 or
   `CTRL+SHIFT+I`) and look for log messages reading:

   * `JupyterLab extension jupytercon2025-extension-workshop is activated!`
   * `Hello, world! This is the '/jupytercon2025-extension-workshop/hello' endpoint. Try
     visiting me in your browser!`

   **If you do not see these messages, let us know you need help!**

7. Directly test the server portion of the extension by visiting the endpoint in your
   browser (`http://localhost:8888/jupytercon2025-extension-workshop/hello`).
   You should see the same message as the last step:
   `Hello, world! This is the '/jupytercon2025-extension-workshop/hello' endpoint. Try
   visiting me in your browser!`


:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Initialize from extension template"
git push -u origin main
```
:::


### Now let's do one complete development loop!

0. Close the JupyterLab server with `CTRL+C`.

1. Make any change to the codebase.
   For example, alter the text in a `console.log()` message.
   We suggest changing `Hello, world!` in the server's message (in
   `jupytercon2025_extension_workshop/routes.py`) to `Hello,
   <your-name-here>!`.

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


## 🏋️ Exercise B (20 minutes): Launching a "hello, world" {term}`widget <widget>`

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
import {
  imageIcon,
} from '@jupyterlab/ui-components';

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
    const widget = new ImageCaptionWidget();
    super({ content: widget });

    this.title.label = 'Random image with caption';
    this.title.caption = this.title.label;
    this.title.icon = imageIcon;
  }
}
```

Our widget is using JavaScript to define HTML elements that will appear in the widget.
That looks like this:

```{mermaid}
graph TB

    subgraph Widget["Widget (this.node)"]
        subgraph Hello["&lt;p&gt; element (hello)"]
            Content["'Hello, world!' (hello.innerHTML)"]
        end
    end
```

And the HTML looks roughly like this:

```html
<div id="our-widget">
  <p>Hello, world!</p>
</div>
```

We can't test this because we don't have a convenient way to display the
widget in JupyterLab yet.
Let's fix that now.


### Create a {term}`command <command>` to display the {term}`widget <widget>` in the {term}`main area <main area>`

In `src/index.ts`, we need to update our plugin to define a command in our
{term}`plugin's <plugin>` `activate` method:

```{code} typescript
:linenos:
:emphasize-lines: 2,26-37

import { requestAPI } from './request';
import { ImageCaptionMainAreaWidget } from './widget';

/**
 * Initialization data for the jupytercon2025-extension-workshop extension.
 */
const plugin: JupyterFrontEndPlugin<void> = {
  id: 'jupytercon2025-extension-workshop:plugin',
  description: 'A JupyterLab extension that displays a random image and caption.',
  autoStart: true,
  activate: (
    app: JupyterFrontEnd,
  ) => {
    console.log('JupyterLab extension jupytercon2025-extension-workshop is activated!');

    requestAPI<any>('hello')
      .then(data => {
        console.log(data);
      })
      .catch(reason => {
        console.error(
          `The jupytercon2025_extension_workshop server extension appears to be missing.\n${reason}`
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

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Add 'hello, world' widget and register with command palette"
git push -u origin main
```
:::


### Optional: Register with the {term}`launcher <launcher>`

Unlike the command palette, this functionality needs to be installed.
First, install `@jupyterlab/launcher` with `jlpm add @jupyterlab/launcher` to make
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

#### Test it!

Repeat the build and test procedure from the previous step.

Open a new tab with the `+` button at the top of the {term}`main area <main area>` and
click the new button in the launcher.

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Register widget with launcher"
git push -u origin main
```
:::


#### My launcher button works, but it has no icon!

Adding an icon is one extra step.
We can import the icon in `src/index.ts` like so:

```typescript
import { imageIcon } from '@jupyterlab/ui-components';
```

and add the icon to the command's metadata:

```{code} typescript
:linenos:
:emphasize-lines: 7

    app.commands.addCommand(command, {
      execute: () => {
        const widget = new ImageCaptionMainAreaWidget();

        app.shell.add(widget, 'main');
      },
      icon: imageIcon,
      label: 'View a random image & caption'
    });
```

Give it another test, and you should see an icon.

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Define icon for widget open command"
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


## 🏋️ Exercise C: Serve images and captions from the server extension

### Set up images and captions

Create a new directory at `jupytercon2025_extension_workshop/images`:

```bash
mkdir jupytercon2025_extension_workshop/images
```

Then, place images in this directory.
You can choose your own images (favorite cat pictures?) or download images from
[our demo repository](https://github.com/jupytercon/jupytercon2025-developingextensions-demo/tree/main/jupytercon2025_extension_workshop/images).

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Add images"
git push -u origin main
```
:::

Now we need a way to associate captions with each image.
We'll use a list of Python dictionaries (mappings) to do so.
Create a new file `jupytercon2025_extension_workshop/images_and_captions.py` and populate it with:

```{code} python
:filename: jupytercon2025_extension_workshop/images_and_captions.py

# Public domain images from https://www.loc.gov/free-to-use/cats/
IMAGES_AND_CAPTIONS = [
	{ "filename": "brunnhilde.jpg", "caption": "Brünnhilde" },
	{ "filename": "cats.jpg", "caption": "Cats" },
	{ "filename": "cat-cher-evolution.jpg", "caption": "Evolution of a cat-cher" },
	{ "filename": "the-entanglement.jpg", "caption": "The entanglement" },
]
```


### Update the server to serve images and captions

Our server behaviors are defined in
`jupytercon2025_extension_workshop/routes.py`, so that module will need to know
about how to access our image data and the associated captions.
We'll import the data structure we just defined and define a new `Path` object
that references the images directory we just created.
We'll need the `random` standard library module in the next step, so we'll
import that too while we're here:

```{code} python
:linenos:
:emphasize-lines: 2-4, 10-12
:filename: jupytercon2025_extension_workshop/routes.py

import base64
import json
import random
from pathlib import Path

from jupyter_server.base.handlers import APIHandler
from jupyter_server.utils import url_path_join
import tornado

from .images_and_captions import IMAGES_AND_CAPTIONS

IMAGES_DIR = Path(__file__).parent.absolute() / "images"


class HelloRouteHandler(APIHandler):
	...
```

Next, we'll set up a new route handler in our server extension.
This route handler will select a random entry from the `IMAGES_AND_CAPTIONS`
constant we imported, open that image, encode its data as a string,
and then return the string-encoded image data alongside the caption.

```{code} python
:linenos:
:emphasize-lines: 5-13
:filename: jupytercon2025_extension_workshop/routes.py

class HelloRouteHandler(APIHandler):
	...


class ImageAndCaptionRouteHandler(APIHandler):
    @tornado.web.authenticated
    def get(self) -> ImageBytesCaption:
        random_selection = random.choice(IMAGES_AND_CAPTIONS)

        # Read the data and encode the bytes in base64
        with open(IMAGES_DIR / random_selection["filename"], "rb") as f:
            b64_bytes = base64.b64encode(f.read()).decode("utf-8")

        self.finish(json.dumps({
            "b64_bytes": b64_bytes,
            "caption": random_selection["caption"],
        }))
```

Finally, we need to connect our new handler to the appropriate route:

```{code} python
:linenos:
:emphasize-lines: 6, 9
:filename: jupytercon2025_extension_workshop/routes.py

def setup_route_handlers(web_app):
    host_pattern = ".*$"
    base_url = web_app.settings["base_url"]

    hello_route_pattern = url_path_join(base_url, "jupytercon2025-extension-workshop", "hello")
    image_route_pattern = url_path_join(base_url, "jupytercon2025-extension-workshop", "random-image-caption")
    handlers = [
        (hello_route_pattern, HelloRouteHandler),
        (image_route_pattern, ImageAndCaptionRouteHandler),
    ]

    web_app.add_handlers(host_pattern, handlers)
```


#### Test!

Now's the best time for us to stop and test before moving on to consuming this
data with our widget.
Does our server endpoint return the data we expect it to?
If there's something wrong and we jump straight to working on the UI, we could
have a bad time.

Since we only altered Python code, we don't need to run `jlpm build`.
We can see our changes by restarting JupyterLab and visiting
`http://localhost:8888/jupytercon2025-extension-workshop/random-image-caption`.

Refresh the page several times, and you should see the data change with each
refresh (except when the same image is randomly selected multiple times in a
row!).

:::{hint}
Not working right?

Look in the terminal that's running the `jupyter lab` command.
Any errors being triggered from the Python code will show up there.

Perhaps you missed an import in a previous step!
:::

:::{important} 💾 **Make a Git commit and push to GitHub now!**
:icon: false

```bash
git add .
git commit -m "Add random image and caption endpoint"
git push -u origin main
```
:::


### Connect the {term}`widget` to the {term}`server extension`

<TODO>


## 🏋️ Exercise D: Add user interactivity to the widget

<TODO>


## 🏋️ Exercise E: Preserve layout

<TODO>


[^rebuild-not-always-required]: You don't actually _always_ need to rebuild -- only when
you change the JavaScript. If you only changed Python, you only need to restart
JupyterLab.
