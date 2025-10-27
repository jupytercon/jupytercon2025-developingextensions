# 🧩 6 - Working on your own extension

:::{important} Outcome
After completing this module, you will have:
- Selected and started work on a JupyterLab extension project that interests you
- Applied concepts from previous sessions to real-world extension development
- Connected with instructors and peers for guidance on your specific project
:::

:::{tip} Terms
TBD
:::


:::{note}
We will be using the [official template](https://github.com/jupyterlab/extension-template) as the starting point to creating your own extension.
:::

## 🎯 Session Format
This is an **open exploration session** - similar to office hours or a study hall. You'll choose a project that interests you and work on it with instructor support. Don't worry about finishing - the goal is to practice the development workflow and problem-solving.

:::{tip} Choose Your Own Adventure
You have three paths:

1. **Build from Scratch** - Create a new extension using the template and AI assistance
2. **Contribute to Existing Extensions** - Add features or fix issues in established projects

All paths are valuable! Pick what excites you most.
:::

## Path 1: Build your own extension from scratch

Create something entirely new using the extension template, AI assistance, and the patterns you've learned.

### Quick start

Follow these sections from chapter 2 to start a new extension from a template:

1. [🛠️ Setup](02-anatomy-of-extensions.md#️-setup) - Set up your environment, Git, and GitHub repository
2. [🏋️ Exercise A: Extension creation and development loop](02-anatomy-of-extensions.md#️-exercise-a-15-minutes-extension-creation-and-development-loop) - Create your extension from the template and test the development loop

Once you have your extension set up and working, return here to choose an inspiration project below.

### Inspiration

The following three examples with prompts have been tested and confirmed working. Pick one that matches your interests - whether you want visual effects, creative customization, or full-stack development. Use them as-is to practice prompt engineering, or as inspiration for your own ideas.

#### 🎉 Confetti celebration button

**Complexity**: Beginner (Frontend only)
**Time**: 15-20 minutes
**What you'll learn**: Status bar integration, DOM manipulation, visual effects

**The Prompt**:
```
Add a button in the status bar to celebrate something! It should show confetti
on top of the UI for 3 seconds
```

**Expected Results**:
- Status bar shows a celebration button (usually bottom-right)
- Clicking triggers confetti animation overlay
- Animation appears on top of all UI elements
- Confetti clears after ~3 seconds
- Multiple clicks work correctly

**What to Watch For**:

If the confetti doesn't appear:
```
The button is there but I don't see confetti. Check that the z-index
is high enough and the confetti container is properly positioned.
```

To customize:
```
Make the confetti more colorful and add a sound effect when it triggers.
```

**Verified Setup**:
- ✅ macOS 15.7, Claude Code (Claude Sonnet 4.5)
- ✅ `conda` environment: Python 3.13, Node.js 22, JupyterLab
- 📹 [Watch the demo](https://www.loom.com/share/2afabea0184045fa868271f9ab0ca083)

:::{tip}
This is a perfect first project - it's self-contained, purely visual, and you'll immediately see if it works. It teaches you about JupyterLab's status bar API without backend complexity.
:::

#### 🎨 Custom theme extension

**Complexity**: Beginner (Frontend only)
**Time**: 20-30 minutes
**What you'll learn**: Theme customization, CSS styling, JupyterLab theming system

**The Prompt**:
```
Create a theme based on [your favorite movie/show/game/aesthetic]
```

**Example**:
```
Create a theme based on Netflix show KPop Demon Hunters
```

**Expected results**:
- A new theme appears in Settings → Theme menu
- Theme includes custom colors matching your chosen aesthetic
- JupyterLab interface reflects the theme's visual style
- Theme can be toggled on/off

**What to watch for**:

To add a background image to the main panel:
```
Can we use this image as a background for main panel?
[paste your image URL]
```

Example:
```
Can we use this image as a background for main panel?
https://www.billboard.com/wp-content/uploads/2025/07/kpop-demon-hunters-billboard-1800.jpg?w=942&h=628&crop=1
```

To refine the styling:
```
The background image is too bright - make it more subtle with reduced opacity
and add a dark overlay so text is readable.
```

To adjust colors:
```
Update the sidebar and toolbar colors to match the theme's color palette.
Use [specific colors] for accent elements.
```

**Tips for finding images**:
- Search "[theme name] wallpaper 4k" for high-quality backgrounds
- Look for images with good contrast for text readability
- Consider color palettes - extract 3-5 main colors from your chosen image (AI can help you with it!)
- Free image sources: Unsplash, Pexels, Wallhaven

:::{tip}
This is a perfect creative project! You get immediate visual feedback, can personalize your JupyterLab environment, and learn how JupyterLab's theming system works. Plus, you'll have a custom theme you actually want to use daily.

Themes are also great conversation starters - share your theme with other workshop participants!
:::

#### 📊 CPU monitor widget

**Complexity**: Intermediate (Frontend + Server)
**Time**: 30-40 minutes
**What you'll learn**: REST API integration, backend data processing, graceful error handling

**The Prompt**:
```
Create a JupyterLab extension that monitors CPU stats:
- Use psutil on the backend to get utilization and temperature data
- Create a REST API endpoint
- Display it in a widget in the main area
- Handle gracefully if some fields aren't available
```

**Expected results**:
- Extension installs `psutil` successfully
- Widget appears in main area when launched
- CPU utilization percentage displays
- Updates automatically (polling every few seconds)
- No errors when temperature data is unavailable
- Widget layout is readable and organized

**What to watch for**:

If `psutil` isn't installed:
```
I'm getting ModuleNotFoundError for psutil. Update pyproject.toml to
include psutil as a dependency.
```

If the widget doesn't update:
```
The widget shows data once but doesn't update. Add automatic polling
every 2 seconds to refresh the CPU stats.
```

To extend it:
```
Add a graph that shows CPU usage over the last 60 seconds,
and highlight in red when usage is above 80%.
```

**Verified setup**:
- ✅ macOS 15.7, Cursor (Claude Sonnet 4.5 MAX)
- ✅ `conda` environment: Python 3.13, Node.js 22, JupyterLab
- ⚠️ Temperature data gracefully handled as N/A on macOS (expected)
- 📹 [Watch the demo](https://www.loom.com/share/9f6d11d537a94a30af7559fd4d80eea2)

:::{tip}
This teaches you the full stack: backend API design, frontend-backend communication, error handling, and periodic updates. It's a perfect template for any monitoring or dashboard extension.
:::

### More extension ideas

Pick an idea that matches your comfort level and interests:

#### Beginner (frontend only)
1. **Theme switcher dropdown**: Add a quick theme selector to the toolbar for easy switching
2. **Clock widget**: Status bar item showing current time with configurable timezone
3. **Quote of the day**: Main area widget that fetches and displays random quotes
4. **Keyboard shortcut viewer**: Panel showing all available shortcuts
5. **Custom welcome screen**: Override the default launcher with your own design
6. **Pomodoro timer**: Status bar timer for focused work sessions with notifications

#### Intermediate (frontend + server)
1. **File size analyzer**: Scan workspace directory and show largest files/folders
2. **Git status widget**: Display current branch, uncommitted changes count
3. **Environment inspector**: Show installed packages and Python/Node versions
4. **Todo list with persistence**: Sidebar panel that saves tasks to disk

#### Advanced
2. **AI code assistant**: Create a chat with LLM
3. **Performance profiler**: Instrument notebook cells and show execution metrics
4. **Custom file format viewer**: Add support for viewing proprietary file types
5. **Real-time log viewer**: Stream and filter server logs in a widget

### 💡 More ideas from JupyterLab community
[JupyterLab Extension Ideas](https://github.com/jupyterlab/jupyterlab/issues?q=sort%3Aupdated-desc+state%3Aopen+label%3A%22tag%3AExtension+Idea%22)

## Path 2: Contribute to existing extensions

Contributing to established extensions is a great way to learn real-world patterns and give back to the community.

### Why contribute?

- Learn from production-quality code
- Work with maintainers who know JupyterLab well
- Your contribution helps thousands of users
- Build your portfolio and resume
- Connect with the Jupyter community

## Path 3: Explore and experiment



## Community Extensions
[Jupyterlab-contrib](https://jupyterlab-contrib.github.io/extensions.html)
[Jupyter Marketplase](https://labextensions.dev/)



## 🔧 Key JupyterLab APIs

### Commonly Used Tokens/Services:
| API | Use Case | Documentation |
|-----|----------|---------------|
| `INotebookTracker` | Track active notebooks | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/notebook.html) |
| `IStatusBar` | Add widgets to status bar | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/statusbar.html) |
| `ICommandPalette` | Register commands | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/apputils.html) |
| `IFileBrowserFactory` | Access file browser | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/filebrowser.html) |
| `IThemeManager` | Theme customization | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/apputils.html) |
| `ISettingRegistry` | Extension settings | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/settingregistry.html) |


### For Server Extensions:

- **Jupyter Server Handlers**: [Documentation](https://jupyter-server.readthedocs.io/en/latest/developers/extensions.html)
- **Tornado Request Handlers**: [Tornado Docs](https://www.tornadoweb.org/en/stable/web.html)


## 🐛 Beginner-Friendly GitHub

Want to contribute to JupyterLab itself? Here are good first issues:

**JupyterLab Core:**
- [Good First Issues](https://github.com/jupyterlab/jupyterlab/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- [Documentation Issues](https://github.com/jupyterlab/jupyterlab/issues?q=is%3Aissue+is%3Aopen+label%3Adocumentation)

**Extension Template:**
- [Extension Template Issues](https://github.com/jupyterlab/extension-template/issues)

## 📚 Essential Resources

### Documentation:
- [JupyterLab Extension Developer Guide](https://jupyterlab.readthedocs.io/en/latest/extension/extension_dev.html)
- [JupyterLab API Reference](https://jupyterlab.readthedocs.io/en/latest/api/)
- [Jupyter Server Extension Guide](https://jupyter-server.readthedocs.io/en/latest/developers/extensions.html)

### Code Examples:
- [Extension Examples Repository](https://github.com/jupyterlab/extension-examples) - 40+ example extensions
- [JupyterLab GitHub](https://github.com/jupyterlab/jupyterlab) - Core extensions source code

### Tools:
- [Browser DevTools](https://developer.chrome.com/docs/devtools/overview) - Debug frontend
- Run JupyterLab with `--debug` to see more server side details

## 🚀 Getting Started Checklist

- [ ] Choose an extension idea (or GitHub issue)
- [ ] Create extension from template (if new project)
- [ ] Identify which JupyterLab APIs you'll need
- [ ] Look up API documentation and examples
- [ ] Set up development environment #TODO: links and `jlpm` commands for quick reference
- [ ] Start coding!
- [ ] Test frequently (`jupyter lab --dev-mode`) #TODO: watch for iterative dev
- [ ] Ask instructors for help when stuck

## 💬 Getting Help

**During this session:**
- Raise your hand for instructor help
- Discuss with neighbors
- [Zulip](https://jupyter.zulipchat.com/#feed) for real-time chat

**After the tutorial:**
- [JupyterLab Discourse](https://discourse.jupyter.org/c/jupyterlab/17) - Community forum
- [Zulip Chat](https://jupyter.zulipchat.com/) - Continue conversing with the Jupyter Community!
- [Stack Overflow](https://stackoverflow.com/questions/tagged/jupyter)
