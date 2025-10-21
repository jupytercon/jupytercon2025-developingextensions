# 🧩 5 - Working on your own extension

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
This is an **open exploration session**. You'll choose a project that interests you and work on it with instructor support. Don't worry about finishing - the goal is to practice the development workflow and problem-solving.


## 💡 Extension Ideas
[JupyterLab Extension Ideas](https://github.com/jupyterlab/jupyterlab/issues?q=sort%3Aupdated-desc+state%3Aopen+label%3A%22tag%3AExtension+Idea%22)

## Community Extensions
[Jupyterlab-contrib](https://jupyterlab-contrib.github.io/extensions.html)
[Jupyter Marketplase](https://labextensions.dev/)

### Frontend Extensions
**Beginner-Friendly:**
TBD
**Intermediate:**
TBD

### Extensions with Server Components
**Beginner-Friendly:**
TBD

**Intermediate:**
TBD

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
