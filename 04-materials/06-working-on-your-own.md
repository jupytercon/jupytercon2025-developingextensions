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

#### 🎨 Custom theme extension

#### 📊 CPU monitor widget

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
