# 🧩 6 - Working on your own extension

:::{important} Outcome
After completing this module, you will have:
- Selected and started work on a JupyterLab extension project that interests you
- Applied concepts from previous sessions to real-world extension development
- Connected with instructors and peers for guidance on your specific project
:::


:::{note}
We will be using the [official template](https://github.com/jupyterlab/extension-template) as the starting point to creating your own extension.
:::

## 🎯 Session Format
This is an **open exploration session** - similar to office hours or a study hall. You'll choose a project that interests you and work on it with instructor support. Don't worry about finishing - the goal is to practice the development workflow and problem-solving.

:::{tip} Choose Your Own Adventure
You have two paths:

1. **Build from Scratch** - Create a new extension using the template and AI assistance
2. **Contribute to Existing Extensions** - Add features or fix issues in established projects

All paths are valuable! Pick what excites you most.
:::

## 🚀 Getting Started

Before diving into your chosen path, make sure you have:

- A clear goal (extension idea or GitHub issue to work on)
- Development environment ready (if building new: follow [Setup](02-anatomy-of-extensions.md#️-setup) and [Exercise A](02-anatomy-of-extensions.md#️-exercise-a-15-minutes-extension-creation-and-development-loop))
- Access to [JupyterLab API docs](https://jupyterlab.readthedocs.io/en/latest/api/) and [Extension Examples](https://github.com/jupyterlab/extension-examples)

## 🔨 Path 1: Build your own extension from scratch

Create something entirely new using the extension template, AI assistance, and the patterns you've learned.

### ⚡ Quick start

Follow these sections from chapter 2 to start a new extension from a template:

1. [🛠️ Setup](02-anatomy-of-extensions.md#️-setup) - Set up your environment, Git, and GitHub repository
2. [🏋️ Exercise A: Extension creation and development loop](02-anatomy-of-extensions.md#️-exercise-a-15-minutes-extension-creation-and-development-loop) - Create your extension from the template and test the development loop

Once you have your extension set up and working, return here to choose an inspiration project below.

### 💫 Inspiration

The following three examples with prompts have been tested and confirmed working. Pick one that matches your interests - whether you want visual effects, creative customization, or full-stack development. Use them as-is to practice prompt engineering, or as inspiration for your own ideas.

#### 🎉 Confetti celebration button

**Complexity**: Beginner (Frontend only)
**Time**: 15-20 minutes
**What you'll work with**: Status bar integration, DOM manipulation, visual effects

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

If the confetti doesn't appear, try prompting:
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
**What you'll work with**: Theme customization, CSS styling, JupyterLab theming system

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
**What you'll work on**: REST API integration, backend data processing, error handling

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
- Widget gracefully handles when temperature data is unavailable
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

### 🌟 More extension ideas

Pick an idea that matches your comfort level and interests:

#### 🎓 Beginner (frontend only)
1. **Theme switcher dropdown**: Add a quick theme selector to the toolbar for easy switching
2. **Quote of the day**: Main area widget that fetches and displays random quotes
3. **Keyboard shortcut viewer**: Panel showing all available shortcuts
4. **Pomodoro timer**: Status bar timer for focused work sessions with notifications

#### 🚦 Intermediate (frontend + server)
1. **File size analyzer**: Scan workspace directory and show largest files/folders
2. **Git status widget**: Display current branch, uncommitted changes count
3. **Environment inspector**: Show installed packages and Python version
4. **Todo list with persistence**: Sidebar panel that saves tasks to disk

#### 🎯 Advanced
5. **Custom welcome screen**: Override the default launcher with your own design
2. **AI code assistant**: Create a chat with LLM
3. **Performance profiler**: Instrument notebook cells and show execution metrics
4. **Custom file format viewer**: Add support for viewing unsupported file types
5. **Real-time log viewer**: Stream and filter server logs in a widget

### 💡 Even more ideas from JupyterLab community
[JupyterLab Extension Ideas](https://github.com/jupyterlab/jupyterlab/issues?q=sort%3Aupdated-desc+state%3Aopen+label%3A%22tag%3AExtension+Idea%22)

## 🤝 Path 2: Contribute to existing extensions

Contributing to established extensions is a great way to learn real-world patterns and give back to the community.

### 🌱 Why contribute?

- Learn from production-quality code
- Work with maintainers who know JupyterLab well
- Your contribution helps thousands of users
- Build your portfolio and resume
- Connect with the Jupyter community

### 🔍 Find your project

#### 🌐 Popular open source extensions

See our [Show & Tell session](04-show-and-tell.md) for a curated list of popular JupyterLab extensions and inspiration on projects to contribute to.

#### 🐛 Beginner-friendly JupyterLab core
Want to contribute to JupyterLab itself? Here are good first issues:

**JupyterLab Core:**
- [Good First Issues](https://github.com/jupyterlab/jupyterlab/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
- [Documentation Issues](https://github.com/jupyterlab/jupyterlab/issues?q=is%3Aissue+is%3Aopen+label%3Adocumentation)

**Extension Template:**
- [Extension Template Issues](https://github.com/jupyterlab/extension-template/issues)

### ✅ Check the project needs
**Check the project board or GitHub Issues** for:
- Feature requests that align with your interests
- Bug reports you can reproduce
- Documentation improvements

**Browse recent issues** and look for:
- Questions you can answer
- Problems you've encountered yourself
- Features you wish existed

### 🎬 Making your first contribution

1. **Set up the development environment**:

   ```bash
   # Fork the repository on GitHub first, then:
   git clone https://github.com/YOUR-USERNAME/extension-name.git
   cd extension-name

   # Follow the project's CONTRIBUTING.md instructions
   # Usually similar to:
   pip install --editable ".[dev,test]"
   jupyter labextension develop . --overwrite
   jlpm build
   ```

2. **Create a branch** for your work:

   ```bash
   git checkout -b fix/issue-123-button-alignment
   ```

3. **Make your changes**:
   - Start small - fix one thing at a time
   - Follow the project's code style
   - Add or update tests if applicable
   - Update documentation

4. **Test thoroughly**:

   ```bash
   # Run the project's test suite
   jlpm test
   pytest

   # Test manually in JupyterLab
   jupyter lab
   ```

5. **Commit and push**:

   ```bash
   git add .
   git commit -m "Fix button alignment in toolbar (#123)"
   git push -u origin fix/issue-123-button-alignment
   ```

6. **Open a Pull Request**:
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Select your fork and branch
   - Describe what you changed and why
   - Reference the issue number if applicable

:::{tip} Using AI for Contributions
AI can help you understand existing code:

```
Explain how this widget's lifecycle works - especially the initialize()
and dispose() methods.
```

```
I want to add a new filter option to this panel. Show me where to add
the UI component and how to wire it to the existing filtering logic.
```

```
This extension uses React. Convert this example to use React best practices
while maintaining compatibility with the existing component structure.
```
:::

### 📋 Contribution checklist

Before opening a PR, verify:

- Code follows the project's style guide
- Changes are tested (automated tests and manual testing)
- Documentation is updated if needed
- Commit messages are clear and descriptive
- PR description explains what and why
- You've read and followed CONTRIBUTING.md
- Remove unused or unreachable code ("dead code") and unused artifacts (imports, variables, files, commented-out code)
- Tests pass in CI/CD (after opening PR)

### 🔧 When contributing gets stuck

**If you're blocked**:
- Comment on the issue asking for clarification
- Join the [Jupyter Zulip chat](https://jupyter.zulipchat.com) (or project-specific chat if available)
- Ask instructors during this session
- It's okay to pause and try something else

**If maintainers request changes**:
- Don't take it personally - writing high quality code requires collaboration
- Ask questions if feedback is unclear
- Make the requested changes iteratively
- Maintainers are volunteers - be patient and courteous

## 🔄 Development Workflow

This is the cycle you'll repeat many times during development:

1. **Write a clear prompt** - Provide context, requirements, and constraints
2. **Review generated code** - Read and understand it; don't blindly accept
3. **Build**: Run `jlpm build` to compile your extension
4. **Test**: Refresh JupyterLab (or restart if backend changed)
5. **Debug**: Check browser console and terminal for errors
6. **Iterate**: Refine your prompt based on results
7. **Commit**: Save working states frequently with git

### 💪 Tips for productive development

- **Focus on small wins**: Small working features beat ambitious broken ones
- **Document as you go**: Note what prompts work well, save error solutions, update README
- **Learn through debugging**: Getting stuck teaches you more than smooth sailing
- **Collaborate**: Share progress with neighbors, help each other debug, show off features

## 💬 Getting Help

### 👥 During this session

**Where to get help:**
- Raise your hand for instructor help
- Discuss with neighbors
- [Jupyter Zulip](https://jupyter.zulipchat.com/#feed) for real-time chat

**When to ask for help** - Don't stay stuck! Ask an instructor if:
- You've tried the same thing 3-4 times with no progress
- AI suggests something that contradicts JupyterLab patterns
- Build succeeds but feature doesn't appear
- You want guidance on architecture decisions
- You're unsure if your contribution approach is right

**How to ask effectively:**
- Share your exact error message
- Show what you've already tried
- Explain what you expected vs. what happened
- Include relevant code snippets

:::{tip} Core Developers Are Here!
Many Jupyter and JupyterLab core developers are in the room and at JupyterCon 2025. This is a unique opportunity to:
- Ask questions about extension development directly from the experts
- Get feedback on your architecture and design decisions
- Learn about upcoming changes and best practices
- Discuss contribution opportunities
- Connect with the people who built the platform you're extending

Don't be shy - they're here to help and love seeing new contributors!
:::

### 🎁 Wrapping up your work

Before the session ends, take 10 minutes to:

1. **Commit your changes**:
   ```bash
   git add .
   git commit -m "Work in progress: [what you accomplished]"
   git push
   ```

2. **Document your progress** in README.md:
   - What you built or contributed
   - What works and what's still in progress
   - Interesting challenges you solved
   - Next steps you'd like to take

3. **Reflect** on what you learned:
   - What surprised you about AI-assisted development?
   - What JupyterLab concepts clicked for you?
   - What would you explore next?


### 🚪 After the tutorial

#### 🛠️ To continue developing
- Add automated tests (`pytest` for Python, `jest` for TypeScript)
- Set up continuous integration (template includes GitHub Actions)
- Write comprehensive documentation
- Create example notebooks showing your extension in use

#### 📢 To share your work
- Publish to PyPI: `python -m build && twine upload dist/*`
- Open-source on GitHub: write a good README, set `jupyterlab-extension` as a repo topic
- Write a blog post about your development experience
- Present at a local developer meetup or a conference
- Track downloads

#### 🌍 To contribute more
- Subscribe to issues on extensions you've contributed to
- Join the [Jupyter Zulip chat](https://jupyter.zulipchat.com) for real-time discussions and help from contributors and users
- Participate in the [Jupyter Discourse forum](https://discourse.jupyter.org/)
- Attend [Jupyter Community Calls](https://jupyter.org/community#calendar) - bring your questions or request code review from experienced developers
- Help others getting started

## 📚 Reference Materials

Bookmark these resources for when you need them during development:

### 📖 Documentation
- [JupyterLab Extension Developer Guide](https://jupyterlab.readthedocs.io/en/latest/extension/extension_dev.html)
- [JupyterLab API Reference](https://jupyterlab.readthedocs.io/en/latest/api/)
- [Jupyter Server Extension Guide](https://jupyter-server.readthedocs.io/en/latest/developers/extensions.html)

### 💻 Code Examples
- [Extension Examples Repository](https://github.com/jupyterlab/extension-examples) - 40+ example extensions
- [JupyterLab GitHub](https://github.com/jupyterlab/jupyterlab) - Core extensions source code

### 🔑 Key JupyterLab APIs

Common tokens and services you'll use:

| API | Use Case | Documentation |
|-----|----------|---------------|
| `INotebookTracker` | Track active notebooks | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/notebook.html) |
| `IStatusBar` | Add widgets to status bar | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/statusbar.html) |
| `ICommandPalette` | Register commands | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/apputils.html) |
| `IFileBrowserFactory` | Access file browser | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/filebrowser.html) |
| `IThemeManager` | Theme customization | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/apputils.html) |
| `ISettingRegistry` | Extension settings | [Docs](https://jupyterlab.readthedocs.io/en/latest/api/modules/settingregistry.html) |

### 🖥️ Server Extensions
- [Jupyter Server Handlers](https://jupyter-server.readthedocs.io/en/latest/developers/extensions.html)
- [Tornado Request Handlers](https://www.tornadoweb.org/en/stable/web.html)

### 🔬 Tools & Discovery
- [Browser DevTools](https://developer.chrome.com/docs/devtools/overview) - Debug frontend
- Run JupyterLab with `--debug` flag for detailed server logs
- [Jupyter Marketplace](https://labextensions.dev/) - Snapshot of active extensions with stats
- [JupyterLab-Contrib Extensions](https://jupyterlab-contrib.github.io/extensions.html) - A list of some unofficial extensions by the community
- [jupyterlab-extension topic](https://github.com/topics/jupyterlab-extension) - Browse extensions on GitHub

:::{tip} The Journey Continues
Building JupyterLab extensions is a skill that develops over time. Every extension you build, every contribution you make, and every bug you debug deepens your understanding.

The patterns you've learned here - using AI assistance effectively, reading documentation, debugging systematically, and iterating rapidly - apply to all software development, not just JupyterLab.

Keep building, keep learning, and welcome to the JupyterLab extension developer community! 🚀
:::
