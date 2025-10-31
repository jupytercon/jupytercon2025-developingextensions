# 🐞 4 - Debugging: What to do when things go wrong?

:::{tip} Outcome
By the end of this module, we'll have looked into JupyterLab extension development errors in both the terminal and browser console, and used the browser console functionalities to gather information that will help us resolve our errors.
:::

## 💡 Introduction

You might find yourself spending a significant portion of your development time debugging an extension that is not building, or some functionality that is now broken. With the right tools and approach,
debugging can be an experience that leaves you feeling accomplished rather than frustrated. In this section we will
go over some common issues that you might encounter when working on extensions, and a few ways in which you can
tackle them.

---

## 1️⃣ Understanding Error Messages

Knowing the kind of error you are dealing with is already a solid step forward.

### 🔍 Types of Errors

#### ⏰ By when they occur

##### Build-time errors
Build-time errors happen when you're compiling your TypeScript code or building your extension.
These are often syntax errors, type mismatches or missing/unused imports that don't allow
you to run your code until they are fixed.

**Common examples:**
- `error TS2304: Cannot find name 'Widget'` (missing import)
- `error TS2551: Property 'refesh' does not exist` (typo in method name)
- `error TS2322: Type 'string' is not assignable to type 'number'` (type mismatch)

Build-time errors appear in your terminal during `jlpm build`.

##### Runtime errors
Runtime errors happen when your extension logic is executing, maybe a failed network request, or an undefined property that you've tried accessing.

**Common examples:**
- `TypeError: Cannot read property 'name' of undefined` (accessing undefined object)
- `ReferenceError: myVariable is not defined` (variable doesn't exist)
- `404 Not Found` (failed API request)
- `NetworkError: Failed to fetch` (network connection issue)

Runtime errors show up in the browser console or `jupyter lab` terminal logs after your extension is loaded.

:::{important} Quick Reference
:class: simple
:icon: false

| Error Type | Where to Look | When It Happens |
|------------|---------------|-----------------|
| **Build-time** | Terminal (`jlpm build`) | During compilation |
| **Runtime (Frontend)** | Browser console | After extension loads |
| **Runtime (Backend)** | `jupyter lab` terminal logs | After extension loads |
:::

#### 🗺️ By where they occur

With a full stack extension, which includes a server component, it is important to understand which side, server-side or client-side, is causing the error.

##### Python server errors

Python errors will show up in the terminal from which you launched your JupyterLab instance (the server terminal).

**Example:**
```python
ValueError: Invalid endpoint configuration
  File "jupytercon2025_extension_workshop/routes.py", line 32, in get
```


##### Javascript frontend errors

JavaScript error details will be available in the browser console.

**Example:**
```javascript
Uncaught TypeError: widget.render is not a function
    at activate (index.ts:23)
```

:::{tip}
There may be times where an issue initially appears to be a frontend problem, for example, data not being displayed, but it is actually a backend problem, like an API endpoint returning the wrong data, or a 500 error.
:::

### 📚 Reading Stack Traces

- Identifying the source of the error
  When JavaScript throws an error, it gives you a stack trace, a trail showing the path the code took before it crashed. It may seem overwhelming at first, but starting from the top and looking for recognizable file names can help. While the stack trace might show you multiple function calls usually only one or two of those are in _your_ code. The others are likely in other libraries like JupyterLab itself. Finding your line in the code, you can find out where things have gone wrong.

- Minified vs source-mapped traces
  Also important, if you see names like `bundle.js:4323`, that's minified code. In development mode with {term}`source maps <Source maps>` enabled, you'll see actual files names like `Panel.tsx:53`, a much more helpful identifier. Extensions created with the `extension-template` should have source maps enabled already.

## 2️⃣ Terminal

### 📋 Terminal Logs

- Where to find JupyterLab server logs
  In the terminal where you run `jupyter lab`, you'll find a great deal of information.

During build time, webpack will tell you about compilation errors, missing dependencies, or TypeScript type errors. And while the messages can be verbose, you can scroll up to the first error and work through any subsequent errors.

## 3️⃣ Browser Developer Tools

This is where the real magic happens! Head to your browser and open the developer tools. Let's take a look at some of the most useful tabs.

:::{tip} 🔧 How to open DevTools

- **Windows/Linux**: Press `F12` or `Ctrl+Shift+I`
- **Mac**: Press `Cmd+Option+I`
  :::

### 💬 Console Tab

- Viewing runtime errors and warnings

   Sometimes, the best tool is {term}`print debugging <Print debugging>`, which is like a print statement for the browser console, using a simple `console.log()`. Walking through your changes and adding a logging statement to verify/inspect data, or ensure your code reaches certain logic can be quick and effective.

:::{tip}
Here's a tip: use different console methods to make your logs easier to scan:

- `console.log()` for general information
- `console.warn()` for things that aren't quite right but aren't breaking anything
- `console.error()` for actual problems
- `console.table()` for arrays and objects (seriously, try this one!)

You can then also filter console messages by type.
:::

- Preserving logs across reloads:
  If you find it would be helpful to reference earlier logs, those that appear just as the page loads, or maybe for comparison, you can look in your browser's `Console` tab for the option to "Persist log"/"Preserve log"

- JupyterLab's internal logging
  As mentioned in the previous sections of this talk, making use of JupyterLab's internal logging by running JupyterLab with `--debug` can also provide further insight into the problem you are facing.

### 🌐 The Network Tab: Following the data trail

If your extension makes API calls to a server (JupyterLab or otherwise), the Network tab can provide you plenty of the details you might need. You can start by checking that the extension resources have been loaded.

Open the Network tab and interact with your extension. You'll see a list of all the HTTP requests made, and click on any request to see:

- the URL that was called
- the request headers and payload
- the response headers and body
- the HTTT status code (200 is OK/good, 404 means not found, and 500 means server error)
  This can be especially useful when debugging issues where data you were expecting is not showing up, or getting an unexpected response from an API call, as getting a 200 response does not always mean you are getting the response you anticipated.

### ⏸️ The Sources Tab: Let's Pause

This is where you can actually step through your code line by line, verifying the logic and behavior.

First, make sure you're running JupyterLab in an environment were your extension is installed and source maps are enabled. Source maps are enabled by default if you're working from the `extension-template`.

Then:

1. Navigate to the Sources tab
2. Find your extension's TypeScript files in the file tree (look for a folder with your extension's name)
3. Click on a line number to set a breakpoint
4. Now, interact with your extension in a way that will execute that line
5. When the code hits your breakpoint, the code execution will pause. You'll see a visual indicator of this in your browser session.

### 🎨 The Elements/Inspector Tab: Examining the DOM

When something looks wrong visually, either a button is in the wrong place, content is missing or styles aren't being applied as expected, you want to visit the Elements tab.

You can right-click on any element in the page and select "Inspect" to jump directly to that element in the Elements tab.

From there, you can:

- See the HTML structure your extension created
- View all CSS styles applied to an element
- See which styles are being overridden (they'll be marked by a strikethrough)
- Inspecting DOM elements created by your extension
- Check computed values (the final calculated values after all CSS is applied)
- Temporarily modify HTML or CSS to test fixes

:::{hint}
There are times where you might right-click on an element expecting to see the browser with the "Inspect" option, but are instead met with a JupyterLab menu. There are a few cases where JupyterLab prevents this default browser behavior and displays it's own context menu, but you can still access that browser menu by: holding `Shift` while right-clicking, or using the dev tools "Select an element" picker (using the keyboard shortcut `Cmd + Shift + C` for macOS or `Ctrl + Shift + C` for Windows/Linux).
:::

---

## 4️⃣ Best Practices

### 🎯 Debugging Workflow

- Start simple: `console.log` first
  When something breaks, you don't need to automatically reach for advanced tools. First adding a few `console.log()` statements can help you: verify that your function is being called, confirm the data has the shape you expect, or discern whether or not you're reaching a line of code that's problematic. Sometimes you'll realize what the issue is quickly with just these simple verification steps.
- Progress to browser DevTools
  If console logs aren't enough, open the browser console and look for errors. Read the stack trace and click on the file links to jump to the section of code where the issue originates from.
- Checkout the Network tab if applicable
  Look through the Network tab if data is involved. Make sure your API calls are succeeding and retrieving the data that you expect.
- Use browser debugger breakpoints for complex issues
  For more complex issues involving state changes, timing issues, or logic problems, set breakpoints in the Sources tab and step through the code. Watch variables change, examine the call stack and understand the flow.
- Keep development environment consistent
  Is the extension installed and enabled? Did your latest changes build successfully?

### 🛡️ Prevention is better

Of course, even better than debugging, would be to have working code that behaves as you expect. A few practices that can save you time debugging are:

- **Let TypeScript help you**: The red squiggles that you might see in your editor are important. Type errors are often caught at compile time and don't become an issue as you're running your extension code in the browser.
- **Use a {term}`linter <Linter>` and a {term}`formatter <Formatter>`**: Tools like {term}`ESLint` and {term}`Prettier` catch common mistakes and help enforce consistent patterns. They'll give you warnings about unused variables or potential null reference errors.
- **Write tests for critical functionality**: While it may take time upfront, unit tests for testing complex logic and important API calls can help you identify the source of errors earlier on.
- **Write clear error messages**: When you add error handling in your code, make the error messages specific, clear, and concise. "Failed to load data", is okay and might be helpful in the moment, but as your code base grows and time passes, "Failed to fetch notebook metadata from /api/notebooks/{id} (Error: {msg})" can make for an easier time tracking the source of an error.

---

## 5️⃣ Hands-on Exercise

:::{exercise} Debug a Broken Extension
Practice debugging techniques by:

1. Identifying errors in a provided broken extension
2. Using console logs to trace execution
3. Setting breakpoints with browser DevTools
4. Fixing the issues step by step
   :::

Work through the issues systematically, using the appropriate debugging tool for each problem. Remember: start simple, use console logs, then progress to more advanced tools as needed.

### Instructions

0. Make sure all your work up to now is saved and/or committed.
1. Navigate to the `src` directory where your `widget.ts` file is located
2. Copy the following modified `widget.ts` file contents and paste them into your local `src/widget.ts` file

```{code} typescript
:linenos:
:filename: src/widget.ts

import { Widget } from '@lumino/widgets';
import { MainAreaWidget, ToolbarButton } from '@jupyterlab/apputils';
import { imageIcon, refreshIcon } from '@jupyterlab/ui-components';

import { requestAPI } from './request';

class ImageCaptionWidget extends Widget {
  // Initialization
  constructor() {
    super();

    // Create and append an HTML <p> (paragraph) tag to our widget's node in
    // the HTML document
    const hello = document.createElement('p');
    hello.innerHTML = 'Hello, world!';
    this.node.appendChild(hello);

    const center = document.createElement('center');
    this.node.appendChild(center);

    // Put an <img> tag into the <center> tag, and also save it as a class
    // attribute so we can update it later.
    this.img = document.createElement('img');
    center.appendChild(this.img);

    // Do the same for a caption!
    this.caption = document.createElement('p');
    center.appendChild(this.caption);

    // BUG #1: Typo in method name - TypeScript will catch this at build time
    // Initialize the image from the server extension
    this.load_imge();
  }

  // Fetch data from the server extension and save the results to img and
  // caption class attributes
  load_image(): void {
    // BUG #3: Wrong endpoint name - will cause 404 error
    requestAPI<any>('random-image-captions')
      .then(data => {
        console.log(data);
        this.img.src = `data:image/jpeg;base64, ${data.b64_bytes}`;
        this.caption.innerHTML = data.caption;
      })
      .catch(reason => {
        console.error(`Error fetching image data.\n${reason}`);
      });
  }

  // Information about class attributes for the type checker
  img: HTMLImageElement;
  caption: HTMLParagraphElement;
}

export class ImageCaptionMainAreaWidget extends MainAreaWidget<ImageCaptionWidget> {
  constructor() {
    const widget = new ImageCaptionWidget();
    super({ content: widget });

    this.title.label = 'Random image with caption';
    this.title.caption = this.title.label;
    this.title.icon = imageIcon;

    // Add a refresh button to the toolbar
    const refreshButton = new ToolbarButton({
      icon: refreshIcon,
      tooltip: 'Refresh image',
      onClick: () => {
        // BUG #2: Wrong method name - using 'as any' bypasses TypeScript checking
        // This will cause a runtime "is not a function" error when clicked
        // Should be load_image(), not refresh()
        (widget as any).refresh();
      }
    });
    this.toolbar.addItem('refresh', refreshButton);
  }
}
```

### Key Debugging Tools

1. **Terminal/Compiler** - TypeScript catches errors at build time
2. **Browser Console** - Runtime errors and logging
3. **Network Tab** - API calls and HTTP status codes

### The 3 Bugs

#### Bug #1: Build-Time Error (Terminal)

- **Type:** TypeScript compilation error
- **Trigger:** Run `jlpm run build` in the terminal

If you try building the extension with this modified `widget.ts` file, you should see a compilation error in the terminal.

**Expected error:**
```text
src/widget.ts:32:10 - error TS2551: Property 'load_imge' does not exist on type 'ImageCaptionWidget'. Did you mean 'load_image'?

32     this.load_imge();
            ~~~~~~~~~

  src/widget.ts:37:3
    37   load_image(): void {
         ~~~~~~~~~~
    'load_image' is declared here.

Found 1 error in src/widget.ts:32
```

#### Bug #2: Runtime Error (Browser Console)

- **Type:** Method doesn't exist
- **Trigger:** Clicking the refresh button

:::{warning} **The Danger of `as any`**
The type cast `(widget as any).refresh()` tells TypeScript: _"Trust me, I know what I'm doing."_

But if `refresh()` doesn't actually exist on the widget:
- **Compile time:** No errors (type checking disabled)
- **Runtime:** `TypeError: widget.refresh is not a function`

This is why `as any` should be avoided—it hides bugs that TypeScript could catch!
:::

**Expected error:**
```console
961.29c067b15a524e556eed.js?v=29c067b15a524e556eed:2 Uncaught TypeError: widget.refresh is not a function
    at Object.onClick (widget.ts:78:25)
    at Ws.o (jlab_core.e595af6ce37775e8a915.js?v=e595af6ce37775e8a915:1:1764774)
    at [... internal JupyterLab code ...]
```

#### Bug #3: Network Error (Browser Network Tab)

- **Type:** Wrong API endpoint (404 status code)
- **Trigger:** Clicking the refresh button

:::{important} **Using the Network Tab**
When your extension builds successfully but data doesn't load, check the **Network tab** in DevTools:

1. Open the DevTools → **Network** tab
2. Filter by **Fetch/XHR**
3. Look for failed requests (red status codes at the endpoint `random-image-captions`)
4. Click the request to inspect the URL and response

**This bug:** The endpoint `'random-image-captions'` (plural) returns **404 Not Found** because the correct endpoint is `'random-image-caption'` (singular).
:::

**What you'll see in Network tab:**
| Field | Value |
|-------|-------|
| Request URL | `http://localhost:8890/jupytercon2025-extension-workshop/random-image-captions?1761852662635` |
| Request Method | `GET` |
| Status Code | **404 Not Found** |
| Remote Address | `[::1]:8890` |
| Referrer Policy | `strict-origin-when-cross-origin` |

---

## 🔧 Common Debugging Scenarios

Let's walk through some common problems when working on developing extensions, and how to approach debugging them.

### ❌ "My extension is not loading!"

First, check the terminal where you're running the build. Did the build complete successfully?
Next, make sure you actually installed the extension and refresh the page. Run `jupyter labextension list` in your terminal to verify it's installed.

Finally, check the browser console for any red errors.

:::{important} Common culprits:
:class: simple
:icon: false

- Syntax errors in your code
- Missing dependencies
- Errors in your plugin's `activate()` function
  :::

### 🖼️ "My UI isn't updating!"

This one can be tricky, as there are a number of possible root causes, especially once you begin using libraries like React, creating more complex and reactive UIs. For the purposes of our tutorial today, we'll only focus on potential CSS specificity conflicts. Sometimes an element _is_ updating but we can't see it because it is being hidden by CSS, or pushed off-screen. Use the elements tab to verify the DOM is changing.

### 🌐 "My API call isn't working!"

Open the Network tab and interact with your extension to trigger the API call. Then look at:

1. **Is the request being made?** If not, maybe your code isn't actually calling the API.
2. **What's the URL?** Check for any typos, incorrect endpoint or ports, or missing path segments.
3. **What's the status code?** Where 500 means there's a server error, 404 means the request endpoint doesn't exist and 403 means your request is unauthorized.
4. **What's the response body?** Even if you do get a 200 status code, the data structure might not be what you expect.

## 📚 Additional Resources

- [JupyterLab Developer Guide](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html)
- [Chrome DevTools Documentation](https://developer.chrome.com/docs/devtools/)
- [Python Debugging Documentation](https://docs.python.org/3/library/debug.html)
