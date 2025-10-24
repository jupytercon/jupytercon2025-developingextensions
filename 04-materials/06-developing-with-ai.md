# 🤖 6 - Developing extensions with AI assistance

::::{hint} Learning objectives
- Use an AI assistant (Cursor, Claude Code, etc.) to build and modify JupyterLab extensions by writing effective prompts with context, constraints, and acceptance criteria; point AI to official docs/APIs and require citations
- Generate and refine extension code (TypeScript/React UI, commands, settings schemas, Python endpoints) with AI guidance; run build/lint/tests and interpret errors to request focused fixes
- Configure and customize a local AI ruleset (AGENTS.md) for your development workflow; understand privacy, licensing, and safety considerations when sharing code with AI tools
::::

::::{tip} Outcome
After this module, you will have:
- Configured a local development environment with AI tools and the AGENTS.md ruleset; created a reproducible prompt checklist/playbook with doc/API citations
- Either created a new extension from the template with at least one user-visible feature, or contributed a meaningful enhancement to an existing extension; shipped it by publishing to PyPI/GitHub or opening a PR with passing tests
- Confidence to continue exploring extension ideas primarily by prompting, while being able to understand and edit the generated code
::::


## AI-Assisted Development in 2025

If you haven't used AI-assisted development tools yet, you're about to experience a significant shift in how you write code. AI coding assistants can help you explore APIs, generate boilerplate, debug errors, and iterate on features much faster than traditional workflows.

### Understanding LLMs (Large Language Models)

AI coding assistants are powered by **Large Language Models (LLMs)** — neural networks trained on vast amounts of text and code. These models can:
- Understand context from your codebase
- Generate code snippets based on natural language descriptions
- Explain existing code and suggest improvements
- Debug errors by analyzing stack traces and code patterns

### Where LLMs Live: Deployment Models

**Frontier Models (Cloud-Hosted):**
- **Examples:** OpenAI's GPT models, Anthropic's Claude models, Google's Gemini models
- **Deployment:** Run on massive server infrastructure by model providers
- **Access:** Pay-per-token via API keys (typically $0.002–$0.03 per 1K tokens)
- **Pros:** Most capable models, no local compute needed
- **Cons:** Requires internet connection, ongoing costs, data leaves your machine

**Smaller Models (Cloud or Local):**
- **Examples:** GPT-3.5, Claude Haiku, Llama 3 (8B/70B), Mistral, Qwen
- **Deployment:** Can run on cloud APIs or self-hosted on consumer hardware
- **Pros:** Lower cost or free (if self-hosted), faster responses
- **Cons:** Less capable than frontier models, self-hosting requires GPU resources

**Open-Source & Open-Weight Models:**
- **Examples:** Llama 3.1, Mistral, Qwen, DeepSeek Coder, StarCoder
- **Licenses:** Vary from fully open (Apache 2.0) to restricted commercial use
- **Deployment:** Can be self-hosted using tools like [Ollama](https://ollama.ai/), [LM Studio](https://lmstudio.ai/), or [vLLM](https://github.com/vllm-project/vllm)
- **Pros:** Full control, no API costs, data stays local
- **Cons:** Requires technical setup and adequate hardware (8GB+ VRAM for smaller models)

### AI IDEs and Tools for Extension Development

Here are three popular AI coding tools you can use in this workshop, ranging from GUI-first to CLI-native:

#### 1. **Cursor** (Popular UI-First IDE)
- **What it is:** A fork of VS Code with deep AI integration
- **LLM Options:** 
  - Built-in models (GPT-4, Claude 3.5) with Cursor subscription (starting $20/month)
  - Bring your own API key (OpenAI, Anthropic)
  - Can be configured to use local models via OpenAI-compatible APIs
- **Best for:** Developers who want a polished, GUI-driven experience
- **Download:** [cursor.com](https://cursor.com/)

#### 2. **Cline** (Open-Source VS Code Extension)
- **What it is:** A VS Code extension (formerly Claude Dev) that provides AI-assisted coding
- **LLM Options:**
  - Bring your own API key (OpenAI, Anthropic, Google, etc.)
  - Supports OpenAI-compatible APIs (e.g., Ollama, LM Studio)
  - Fully open-source and community-driven
- **Best for:** Developers who prefer VS Code and want flexibility
- **Install:** Search "Cline" in VS Code extensions or visit [github.com/cline/cline](https://github.com/cline/cline)

#### 3. **Claude Code** (CLI for Power Users)
- **What it is:** Command-line interface for Claude, optimized for coding workflows
- **LLM Options:**
  - Requires Anthropic API key
  - Works with Claude 3.5 Sonnet and other Claude models
- **Best for:** CLI warriors who live in the terminal
- **Install:** `pip install claude-code` or check [Anthropic's documentation](https://docs.anthropic.com/)

:::{tip} Self-Hosting LLMs
If you want to run models locally (for privacy or cost savings), tools like [Ollama](https://ollama.ai/) make it easy:

```bash
# Install Ollama (macOS/Linux)
curl -fsSL https://ollama.ai/install.sh | sh

# Download and run a coding model
ollama run deepseek-coder:6.7b

# Use with Cursor/Cline via OpenAI-compatible API
# Point to http://localhost:11434/v1
```

Most AI tools can be "coerced" into using local models by configuring them to point to an OpenAI-compatible API endpoint.
:::

### Choosing Your Tool

For this workshop, we recommend:
- **New to AI coding?** Start with **Cursor** for the smoothest experience
- **Already using VS Code or can run local models?** Try **Cline** for seamless integration
- **Comfortable with CLI?** **Claude Code** gives you full control

All three tools work well with the AI-enhanced extension template we'll use next.

### Further Reading

- [Simon Willison's blog on AI-assisted programming](https://simonwillison.net/tags/ai-assisted-programming/) — Practical insights from a prolific developer
- ["Not all AI-assisted programming is vibe coding (but vibe coding rocks)"](https://simonwillison.net/2025/Jan/7/vibe-coding/) — Understanding different AI coding styles
- [Ollama Documentation](https://ollama.ai/) — Self-hosting open-source models
- [LM Studio](https://lmstudio.ai/) — GUI for running local LLMs


## Getting Started: Clone the AI-Enhanced Extension Template

For this module, we'll use an enhanced version of the JupyterLab extension template that includes AI-specific configurations and rulesets.

:::{important} Prerequisites
* `copier`. Install with any of the following options:
    * Into an existing environment (don't forget to activate it first):
        * `pip install "copier~=9.2" jinja2-time`
        * `conda install -c conda-forge "copier~=9.2" jinja2-time`
    * Globally:
        * `uv tool install --with jinja2-time copier~=9.2`
        * `pixi global install "copier~=9.2" --with jinja2-time`
:::

### Clone and setup the template

1. Change to the parent directory where you want to work:

    ```bash
    cd ~/Projects
    ```

2. Create an extension repo in a new directory:

    ```bash
    mkdir my-jupyterlab-extension
    cd my-jupyterlab-extension
    git init
    ```

3. Instantiate the AI-enhanced template:

    ```bash
    copier copy --trust https://github.com/orbrx/extension-template-cursor .
    ```

:::{note}
By JupyterCon 2025, these AI-specific enhancements should be merged into the [official JupyterLab extension template](https://github.com/jupyterlab/extension-template).
:::


## 🏋️ Exercise 1 (45 minutes): Extending the Image Viewer with AI

In {doc}`02-anatomy-of-extensions`, you built a JupyterLab extension that displays random images with captions from a curated collection. Now, we'll use AI to extend this viewer with image editing capabilities.

### Option 1: Continue with Your Own Extension

If you completed the anatomy module and want to continue with your extension:

1. Navigate to your extension directory:

   ```bash
   cd ~/Projects/jupytercon2025-extension-workshop
   ```

2. Ensure your extension is on the final commit from the anatomy module (with the layout restoration feature).

3. Verify your extension is working:

   ```bash
   # Activate your environment
   micromamba activate jupytercon2025
   
   # Build and start JupyterLab
   jlpm build
   jupyter lab
   ```

4. Skip to [Verify AI Configuration](#verify-ai-configuration) below.

### Option 2: Clone the Finished Extension

If you'd prefer to start fresh or didn't complete the anatomy module:

1. Clone the demo repository:

   ```bash
   cd ~/Projects
   git clone https://github.com/jupytercon/jupytercon2025-developingextensions-demo.git jupytercon2025-ai-workshop
   cd jupytercon2025-ai-workshop
   ```

2. Check out the completed state from the anatomy module:

   ```bash
   git checkout exercise-e-step-1
   ```

3. Install and verify the extension works:

   ```bash
   # Create/activate environment
   micromamba create -n jupytercon2025-ai python pip nodejs gh "copier~=9.2" jinja2-time
   micromamba activate jupytercon2025-ai
   
   # Install the extension in development mode
   pip install --editable ".[dev,test]"
   jupyter labextension develop . --overwrite
   jupyter server extension enable jupytercon2025_extension_workshop
   
   # Build and start JupyterLab
   jlpm build
   jupyter lab
   ```

### Verify AI Configuration

Your extension should include AI-specific configuration files that help guide AI assistants in understanding JupyterLab extension development patterns.

1. Check for the AI ruleset file:

   ```bash
   # Look for either of these files in your extension root
   ls -la AGENTS.md CLAUDE.md 2>/dev/null
   ```

   :::{important}
   If neither `AGENTS.md` nor `CLAUDE.md` exists, **let an instructor know!** These files contain important context and rules that help AI assistants generate better JupyterLab extension code.
   :::

2. Review the ruleset file (optional but recommended):

   ```bash
   # View the AI ruleset
   cat AGENTS.md  # or CLAUDE.md
   ```

   These files should be identical and include:
   - JupyterLab architecture patterns and best practices
   - Common pitfalls to avoid
   - TypeScript/React conventions for JupyterLab
   - Testing and debugging guidance
   - Links to official documentation and APIs

### Understanding Your Starting Point

Before we extend the functionality, let's review what the extension currently does:

**Frontend (TypeScript):**
- `src/index.ts`: Plugin registration, command definitions, launcher/palette integration
- `src/widget.ts`: Main UI widget that displays images and captions
- `src/request.ts`: Utility for communicating with the backend API

**Backend (Python):**
- `jupytercon2025_extension_workshop/routes.py`: REST API handlers
- `jupytercon2025_extension_workshop/images_and_captions.py`: Image metadata
- `jupytercon2025_extension_workshop/images/`: Image files on disk

**Current Features:**
- ✅ Displays random images from a curated collection
- ✅ Shows captions for each image
- ✅ Refresh button to load a new random image
- ✅ Layout restoration (widget persists across JupyterLab sessions)

### Goal: Add Image Editing Capabilities

We'll use AI to extend this viewer with basic image editing features. This exercise demonstrates:
- How to communicate complex feature requirements to an AI assistant
- How AI can help navigate unfamiliar APIs (PIL/Pillow for image processing)
- The iterative development cycle: prompt → generate → test → refine
- How to debug and fix issues with AI assistance

### The Prompt

We'll demonstrate this development process using two different AI tools:
- **Cursor AI** (IDE-integrated approach)
- **Claude Code** (CLI-based approach)

Here's the prompt we'll use (feel free to adapt it):

````markdown
Extend this image viewer extension to add image editing capabilities:

Add editing controls to the widget:
- Buttons for filters: grayscale, sepia, blur, sharpen
- Basic crop functionality (50% crop from center)
- Brightness/contrast adjustments (slider controls)
- Save edited image back to disk

Use Pillow (PIL) on the backend to process images. The backend should:
- Accept the image filename and editing operation via REST API
- Apply the transformation using appropriate Pillow methods
- Return the processed image to the frontend as base64-encoded data

The frontend should:
- Update the displayed image immediately after each edit
- Show the current filter/transformation applied
- Allow chaining multiple edits before saving

Technical requirements:
- Add Pillow to the Python dependencies
- Create a new REST endpoint `/edit-image` in routes.py
- Add filter buttons to the widget toolbar
- Maintain the existing refresh functionality
````

:::{tip} Prompt Engineering Tips
**Effective prompts for AI coding assistants include:**

1. **Context**: What you're working on ("Extend this image viewer extension...")
2. **Specific requirements**: List concrete features ("Buttons for filters: grayscale, sepia...")
3. **Constraints**: Technology choices ("Use Pillow (PIL) on the backend...")
4. **Acceptance criteria**: How to verify success ("Update the displayed image immediately...")

**Ask AI to cite documentation:**
- "Point to official Pillow API docs for each filter"
- "Link to JupyterLab widget documentation for toolbar buttons"
- "Show me the TypeScript type definitions for REST responses"

**Request explanations:**
- "Explain why you chose this approach over alternatives"
- "Comment the code explaining the image processing pipeline"
- "What are the tradeoffs of processing images on the backend vs frontend?"
:::

### Before You Start: Choose Your AI Tool

Pick **one** of the following paths:

- 👉 [Path A: Cursor AI (IDE)](#path-a-cursor-ai-ide) - Recommended for beginners
- 👉 [Path B: Claude Code (CLI)](#path-b-claude-code-cli) - For terminal enthusiasts

---

## Path A: Cursor AI (IDE)

We'll demonstrate the complete development workflow using Cursor AI, showing you how to iterate on the prompt and debug issues.

:::{note} Live Demo
An instructor will demonstrate this path in real-time. Follow along or take notes to try it yourself afterward.
:::

### Setup Cursor AI

1. Download and install Cursor from [cursor.com](https://cursor.com/)

2. Configure your API key or subscription:
   - **Option 1:** Sign up for Cursor subscription ($20/month, includes API access)
   - **Option 2:** Bring your own OpenAI or Anthropic API key
     - Open Settings (`Cmd/Ctrl + ,`)
     - Navigate to "Cursor" → "API Keys"
     - Add your key

3. Open your extension folder in Cursor:

   ```bash
   cursor ~/Projects/jupytercon2025-extension-workshop
   ```

### Using Cursor to Generate Code

1. **Open the Cursor Chat panel** (`Cmd/Ctrl + L`)

2. **Add context files** by clicking the `+` button or dragging files:
   - `src/widget.ts` (where UI changes will go)
   - `jupytercon2025_extension_workshop/routes.py` (where backend changes will go)
   - `AGENTS.md` or `CLAUDE.md` (AI ruleset with JupyterLab patterns)
   - `package.json` and `pyproject.toml` (for dependency context)

3. **Paste the prompt** from above into the chat

4. **Review the generated code**:
   - Cursor will suggest changes across multiple files
   - Read through each change carefully
   - Look for comments explaining the approach
   - Check if dependencies were added correctly

5. **Accept or modify** the suggestions:
   - Click "Accept" to apply all changes
   - Or click individual files to review and edit before accepting

### Testing and Debugging

1. **Install new Python dependencies** (if Pillow was added):

   ```bash
   pip install --editable ".[dev,test]"
   ```

2. **Rebuild the extension**:

   ```bash
   jlpm build
   ```

3. **Restart JupyterLab** (stop with `Ctrl+C`, then):

   ```bash
   jupyter lab
   ```

4. **Test the new features**:
   - Open the image viewer widget
   - Try each filter button
   - Check the browser console for errors (`F12` or `Cmd+Option+I`)
   - Check the terminal running `jupyter lab` for Python errors

### Common Issues and How to Fix Them with AI

#### Issue 1: Build Errors

If `jlpm build` fails:

1. Copy the error message
2. Return to Cursor chat and ask:
   ```
   I got this build error when running `jlpm build`:
   
   [paste error]
   
   Please fix the TypeScript issues.
   ```

#### Issue 2: Runtime Errors

If you see errors in the JupyterLab UI or browser console:

1. Take a screenshot or copy the error
2. Ask Cursor:
   ```
   The filter buttons don't work. Here's the console error:
   
   [paste error]
   
   What's wrong with the API request? Show me the corrected code.
   ```

#### Issue 3: Backend Errors

If the Python server throws errors:

1. Copy the stack trace from the terminal
2. Ask Cursor:
   ```
   The /edit-image endpoint is failing with this error:
   
   [paste traceback]
   
   Fix the Pillow image processing logic.
   ```

### Iterating on the Prompt

After getting basic functionality working, try refining with follow-up prompts:

```
The sepia filter looks too strong. Make it more subtle.
```

```
Add undo/redo buttons to revert edits.
```

```
Save the edit history so I can see what filters were applied.
```

:::{important} 💾 **Make Git commits as you go!**
After each successful change:

```bash
git add .
git commit -m "Add [specific feature] with AI assistance"
```

This makes it easy to revert if AI suggests something that breaks your extension.
:::

---

## Path B: Claude Code (CLI)

For developers who prefer working in the terminal, Claude Code provides a powerful command-line interface for AI-assisted development.

:::{note} Live Demo
An instructor will demonstrate this path in real-time. Follow along or take notes to try it yourself afterward.
:::

### Setup Claude Code

1. Install Claude Code:

   ```bash
   pip install claude-code
   ```

2. Configure your Anthropic API key:

   ```bash
   export ANTHROPIC_API_KEY="your-api-key-here"
   ```

   :::{tip}
   Add this to your `~/.bashrc` or `~/.zshrc` to persist across sessions:
   ```bash
   echo 'export ANTHROPIC_API_KEY="your-key"' >> ~/.zshrc
   ```
   :::

3. Navigate to your extension directory:

   ```bash
   cd ~/Projects/jupytercon2025-extension-workshop
   ```

### Using Claude Code to Generate Code

1. **Start an interactive session**:

   ```bash
   claude-code
   ```

2. **Provide context** by referencing files in your prompt:

   ```
   I'm working on a JupyterLab extension. Please read these files for context:
   - src/widget.ts
   - jupytercon2025_extension_workshop/routes.py
   - AGENTS.md
   - package.json
   - pyproject.toml
   
   [Then paste the main prompt about adding image editing capabilities]
   ```

3. **Review and apply changes**:
   - Claude Code will show diffs for each file
   - Type `y` to accept, `n` to skip, or `e` to edit
   - Changes are applied directly to your files

### Testing and Debugging

Follow the same testing workflow as Path A:

1. Install dependencies: `pip install --editable ".[dev,test]"`
2. Rebuild: `jlpm build`
3. Restart: `jupyter lab`
4. Test in the browser

For errors, paste them back into Claude Code:

```
I got this error when testing the grayscale filter:

[paste error]

Please fix it.
```

### Claude Code Tips

**Run commands without leaving the chat:**

```
Can you also run `jlpm build` to verify this compiles?
```

**Ask for explanations:**

```
Before you change the code, explain how Pillow's ImageFilter.BLUR works
and why you're choosing this approach.
```

**Request tests:**

```
Generate pytest tests for the new /edit-image endpoint.
```

---

## Reflection and Next Steps

After completing this exercise (via either path), take a moment to reflect:

### What Worked Well?

- Did AI correctly understand the JupyterLab extension architecture?
- Were the generated changes isolated and focused?
- Did the AI cite relevant documentation (Pillow, JupyterLab APIs)?

### What Needed Refinement?

- Did you have to iterate on the prompt to get the right behavior?
- Were there bugs that required manual debugging?
- Did AI make assumptions you had to correct?

### Key Takeaways

✅ **AI excels at:**
- Generating boilerplate code (new endpoints, UI components)
- Navigating unfamiliar APIs (Pillow image processing)
- Explaining code and suggesting improvements
- Fixing syntax and type errors

⚠️ **AI may struggle with:**
- Complex architectural decisions
- Subtle bugs in asynchronous code
- Optimizing performance
- Understanding project-specific conventions without explicit guidance (that's why `AGENTS.md` helps!)

:::{tip} Best Practices
1. **Start small**: Get one feature working before adding complexity
2. **Test frequently**: Build and test after each AI-generated change
3. **Commit often**: Use Git to checkpoint working states
4. **Ask questions**: Don't accept code you don't understand—ask AI to explain
5. **Provide feedback**: If AI suggests something wrong, say so and explain why
:::

### Challenge Extensions (Optional)

If you finish early or want to continue exploring, try these follow-up prompts:

1. **Add image rotation:**
   ```
   Add 90-degree rotation buttons (clockwise and counter-clockwise)
   ```

2. **Implement a filter preview:**
   ```
   Show a small preview thumbnail for each filter before applying it
   ```

3. **Add keyboard shortcuts:**
   ```
   Add keyboard shortcuts for common filters (g for grayscale, s for sepia)
   ```

4. **Export functionality:**
   ```
   Add a "Download" button that saves the edited image to the user's computer
   ```

5. **Preset combinations:**
   ```
   Add preset buttons that apply multiple filters at once 
   (e.g., "Vintage" = sepia + slight blur)
   ```

:::{important} 💾 **Final Git commit and push!**
```bash
git add .
git commit -m "Complete Exercise 1: Image editing with AI assistance"
git push
```
:::


## What's Next?

You've now experienced the complete AI-assisted development workflow:
- ✅ Used AI to generate code for new features
- ✅ Debugged issues by iterating on prompts
- ✅ Learned to provide effective context and constraints
- ✅ Understood when to accept AI suggestions vs. when to customize

### Continuing Your Journey

The next chapter, {doc}`07-independent-work`, provides **independent exploration time** where you can:

1. **Build your own extension from scratch** - Using the template and proven project ideas
2. **Contribute to existing extensions** - Give back to the community and learn from production code
3. **Explore and experiment** - Try multiple small projects to deepen your understanding

Choose the path that interests you most, work at your own pace, and instructors will be available to help when you get stuck.

:::{tip} Before Moving On
Take a 5-minute break to:
- Stretch and rest your eyes
- Reflect on what you learned in Exercise 1
- Think about what you'd like to build or explore next
- Grab water or coffee ☕

See you in the next chapter for independent work time!
:::
