# 🚀 7 - Independent Work: Build or Contribute

:::{hint} Session Format
This is **independent exploration time** - similar to office hours or a study hall. Work at your own pace on a project that interests you. Instructors are available to help when you get stuck.

**Duration**: 120 minutes (until the end of the day) 
**Format**: Self-directed with instructor support  
**Goal**: Apply what you've learned by building something new or contributing to an existing extension
:::

:::{tip} Choose Your Own Adventure
You have three paths:

1. **Build from Scratch** - Create a new extension using the template and AI assistance
2. **Contribute to Existing Extensions** - Add features or fix issues in established projects
3. **Explore and Experiment** - Try multiple small ideas to deepen your understanding

All paths are valuable! Pick what excites you most.
:::


## Path 1: Build Your Own Extension from Scratch

Create something entirely new using the extension template, AI assistance, and the patterns you've learned.

### Quick Start

1. Create a new extension from the template:

   ```bash
   cd ~/Projects
   mkdir my-jupyterlab-extension
   cd my-jupyterlab-extension
   git init
   ```

2. Instantiate the template:

   ```bash
   copier copy --trust https://github.com/jupyterlab/extension-template .
   ```

3. Choose your extension type based on your idea:
   - **Frontend only**: For UI-only features (widgets, buttons, panels)
   - **Frontend + Server**: When you need Python backend logic (data processing, file system access, external APIs)
   - **Theme**: Customize the look and feel of JupyterLab by creating your own theme extension (colors, fonts, icons)
   

4. Install and verify the base extension works:

   ```bash
   # Install in development mode
   pip install --editable ".[dev,test]"
   jupyter labextension develop . --overwrite
   
   # If you chose frontend + server:
   jupyter server extension enable <your-extension-name>
   
   # Build and test
   jlpm build
   jupyter lab
   ```

### Tested Example Projects

These three examples have been tested and confirmed working. Pick one that matches your interests - whether you want visual effects, creative customization, or full-stack development. Use them as-is to practice prompt engineering, or as inspiration for your own ideas.

---

#### 🎉 Confetti Celebration Button

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

---

#### 🎨 Custom Theme Extension

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

**Expected Results**:
- A new theme appears in Settings → Theme menu
- Theme includes custom colors matching your chosen aesthetic
- JupyterLab interface reflects the theme's visual style
- Theme can be toggled on/off

**What to Watch For**:

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

**Tips for Finding Images**:
- Search "[theme name] wallpaper 4k" for high-quality backgrounds
- Look for images with good contrast for text readability
- Consider color palettes - extract 3-5 main colors from your chosen image (AI can help you with it!)
- Free image sources: Unsplash, Pexels, Wallhaven

:::{tip}
This is a perfect creative project! You get immediate visual feedback, can personalize your JupyterLab environment, and learn how JupyterLab's theming system works. Plus, you'll have a custom theme you actually want to use daily. 

Themes are also great conversation starters - share your theme with other workshop participants!
:::

---

#### 📊 CPU Monitor Widget

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

**Expected Results**:
- Extension installs `psutil` successfully
- Widget appears in main area when launched
- CPU utilization percentage displays
- Updates automatically (polling every few seconds)
- No errors when temperature data is unavailable
- Widget layout is readable and organized

**What to Watch For**:

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

**Verified Setup**:
- ✅ macOS 15.7, Cursor (Claude Sonnet 4.5 MAX)
- ✅ `conda` environment: Python 3.13, Node.js 22, JupyterLab
- ⚠️ Temperature data gracefully handled as N/A on macOS (expected)
- 📹 [Watch the demo](https://www.loom.com/share/9f6d11d537a94a30af7559fd4d80eea2)

:::{tip}
This teaches you the full stack: backend API design, frontend-backend communication, error handling, and periodic updates. It's a perfect template for any monitoring or dashboard extension.
:::

---

### More Ideas by Difficulty

Pick an idea that matches your comfort level and interests:

**Beginner (Frontend Only)**:
1. **Theme switcher dropdown**: Add a quick theme selector to the toolbar for easy switching
2. **Clock widget**: Status bar item showing current time with configurable timezone
3. **Quote of the day**: Main area widget that fetches and displays random quotes
4. **Keyboard shortcut viewer**: Panel showing all available shortcuts
5. **Custom welcome screen**: Override the default launcher with your own design
6. **Pomodoro timer**: Status bar timer for focused work sessions with notifications

**Intermediate (Frontend + Server)**:
1. **File size analyzer**: Scan workspace directory and show largest files/folders
2. **Git status widget**: Display current branch, uncommitted changes count
3. **Environment inspector**: Show installed packages and Python/Node versions
4. **Todo list with persistence**: Sidebar panel that saves tasks to disk

**Advanced**:
2. **AI code assistant**: Create a chat with LLM
3. **Performance profiler**: Instrument notebook cells and show execution metrics
4. **Custom file format viewer**: Add support for viewing proprietary file types
5. **Real-time log viewer**: Stream and filter server logs in a widget


## Path 2: Contribute to Existing Extensions

Contributing to established extensions is a great way to learn real-world patterns and give back to the community.

### Why Contribute?

- Learn from production-quality code
- Work with maintainers who know JupyterLab well
- Your contribution helps thousands of users
- Build your portfolio and resume
- Connect with the Jupyter community

### Finding Contribution Opportunities

#### Extensions Looking for Contributors

Here are actively-maintained JupyterLab extensions that welcome contributions:

TODO: Add them

### How to Find Good First Issues

1. **Visit the repository** and look for labels:
   - `good first issue`
   - `help wanted`
   - `beginner-friendly`
   - `documentation`

2. **Check the project board** or GitHub Issues for:
   - Feature requests that align with your interests
   - Bug reports you can reproduce
   - Documentation improvements

3. **Browse recent issues** and look for:
   - Questions you can answer
   - Problems you've encountered yourself
   - Features you wish existed

### Making Your First Contribution

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

### Contribution Checklist

Before opening a PR, verify:

- [ ] Code follows the project's style guide
- [ ] Changes are tested (automated tests and manual testing)
- [ ] Documentation is updated if needed
- [ ] Commit messages are clear and descriptive
- [ ] PR description explains what and why
- [ ] You've read and followed CONTRIBUTING.md
- [ ] Tests pass in CI/CD (after opening PR)

### When Contributing Gets Stuck

**If you're blocked**:
- Comment on the issue asking for clarification
- Join the [Jupyter Zulip chat](https://jupyter.zulipchat.com) (or project-specific chat if available)
- Ask instructors during this session
- It's okay to pause and try something else

**If maintainers request changes**:
- Don't take it personally - it's about code quality
- Ask questions if feedback is unclear
- Make the requested changes iteratively
- Maintainers are volunteers - be patient and grateful


## Path 3: Explore and Experiment

Not ready to commit to a full project? Try multiple small experiments to deepen your understanding.

### Micro-Projects (10-15 minutes each)

These are bite-sized experiments you can complete quickly:

1. **Change an icon**: Pick an existing JupyterLab command and change its icon
2. **Add a menu item**: Create a new menu entry that opens a URL
3. **Create a notification**: Show a toast notification when JupyterLab loads
4. **Customize the theme**: Add custom CSS to change colors or fonts
5. **Log user actions**: Create a plugin that logs when users run cells

### Exploration Prompts

Use these prompts with your AI assistant to explore JupyterLab's capabilities:

```
Show me all the different places I can add UI elements in JupyterLab 
(toolbar, menu bar, status bar, sidebar, etc.) with minimal code examples.
```

```
Create a simple example of each JupyterLab widget type: MainAreaWidget, 
Panel, ToolbarButton, and Dialog.
```

```
Demonstrate how to listen for JupyterLab events: file opened, cell executed, 
theme changed, etc.
```

```
Show me how to store and retrieve user settings/preferences for my extension.
```

### Reading Code for Learning

Pick an extension from the list in Path 2 and explore its codebase:

1. **Start with `package.json`**: What dependencies does it use?
2. **Read `src/index.ts`**: How is the plugin structured?
3. **Find the widgets**: What UI components does it create?
4. **Trace a feature**: Pick one feature and follow it from UI to backend

**Use AI to understand**:
```
I'm reading the jupyterlab-git extension. Explain how the git panel 
widget is registered and what lifecycle methods it implements.
```


## General Guidance

### Development Workflow (Reminder)

The cycle you'll repeat many times:

1. **Write a clear prompt** - Context, requirements, constraints
2. **Review generated code** - Read it, understand it, don't blindly accept
3. **Build**: `jlpm build`
4. **Test**: Refresh JupyterLab (or restart if backend changed)
5. **Debug**: Browser console and terminal for errors
6. **Iterate**: Refine your prompt based on results
7. **Commit**: Save working states frequently

### When to Ask for Help

Don't stay stuck! Ask an instructor if:

- You've tried the same thing 3-4 times with no progress
- AI suggests something that contradicts JupyterLab patterns
- Build succeeds but feature doesn't appear
- You want guidance on architecture decisions
- You're unsure if your contribution approach is right

:::{tip} Core Developers Are Here!
Many Jupyter and JupyterLab core developers are in the room and at JupyterCon 2025. This is a unique opportunity to:
- Ask questions about extension development directly from the experts
- Get feedback on your architecture and design decisions
- Learn about upcoming changes and best practices
- Discuss contribution opportunities
- Connect with the people who built the platform you're extending

Don't be shy - they're here to help and love seeing new contributors!
:::

**Asking well gets better help**:
- Share your exact error message
- Show what you've already tried
- Explain what you expected vs. what happened
- Include relevant code snippets

### Making the Most of This Time

**Focus on learning, not perfection**:
- Getting stuck and debugging teaches you more than smooth sailing
- Small working features beat ambitious broken ones
- Exploring multiple approaches builds intuition

**Document your journey**:
- Take notes on what prompts work well
- Save error messages and solutions
- Write down "aha!" moments
- Update your extension's README as you go

**Connect with others**:
- Share what you're working on with neighbors
- Help each other debug issues
- Show off cool features you've created
- Ask each other questions

### Wrapping Up Your Work

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

### After the Workshop

**To continue developing**:
- Add automated tests (`pytest` for Python, `jest` for TypeScript)
- Set up continuous integration (template includes GitHub Actions)
- Write comprehensive documentation
- Create example notebooks showing your extension in use

**To share your work**:
- Publish to PyPI: `python -m build && twine upload dist/*`
- Add to the [JupyterLab Extensions list](https://github.com/mauhai/awesome-jupyterlab)
- Write a blog post about your development experience
- Present at a local Jupyter meetup

**To contribute more**:
- Subscribe to issues on extensions you've contributed to
- Join the [Jupyter Zulip chat](https://jupyter.zulipchat.com) for real-time discussions
- Participate in the [Jupyter Discourse forum](https://discourse.jupyter.org/)
- Attend [Jupyter Community Calls](https://jupyter.org/community#calendar) - bring your questions or request code review from experienced developers
- Help others getting started

:::{tip} The Journey Continues
Building JupyterLab extensions is a skill that develops over time. Every extension you build, every contribution you make, and every bug you debug deepens your understanding.

The patterns you've learned here - using AI assistance effectively, reading documentation, debugging systematically, and iterating rapidly - apply to all software development, not just JupyterLab.

Keep building, keep learning, and welcome to the JupyterLab extension developer community! 🚀
:::

