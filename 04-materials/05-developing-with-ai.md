# 🤖 5 - Developing extensions with AI assistance

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

:::{note} Inspired by...
:class: dropdown

This module's teaching approach is inspired by:

* [Agentic AI Programming for Python Course](https://training.talkpython.fm/courses/agentic-ai-programming-for-python)
  by TalkPython Training
:::


## 🚀 AI-assisted development in 2025

If you haven't used AI-assisted development tools yet, you're about to experience a significant shift in how you write code. AI coding assistants can help you explore APIs, generate boilerplate, debug errors, and iterate on features much faster than traditional workflows.

### 🎯 Setting expectations

Before we dive into tools and techniques, let's set the right mindset for working with AI.

**Key mental model:** Think of your AI assistant as a fast, eager junior developer who:
- ✅ Learns quickly and has read tons of documentation (but maybe not the latest stuff)
- ✅ Can scaffold code and explore APIs incredibly fast
- ✅ Sometimes makes mistakes or misunderstands requirements
- ✅ Benefits from clear instructions and iterative feedback
- ✅ Gets better with guidance and context

:::{important} Don't expect perfection
If AI gets 95% of what you need correct, that's incredible — exactly what you'd get from a talented junior developer given the same instructions.
You would never hire a developer and expect absolute perfection in their creative work. Apply the same perspective to AI. If it gets most things right and you refine the rest, that's a massive productivity win.
:::

**The right mindset:**
- Don't get frustrated by small mistakes — iterate and guide
- **Review all generated code** (you wouldn't merge a PR without review)
- Ask questions: "Why did you choose this approach?"
- Treat errors as learning opportunities for both you and the AI
- Don't be afraid to roll back to the beginning and start over if AI doubled down on a wrong path

### 🧠 Understanding {term}`LLMs <LLM>` (large language models)

AI coding assistants are powered by **Large Language Models ({term}`LLMs <LLM>`)** — neural networks trained on vast amounts of text and code. These models can:
- Understand context from your codebase
- Generate code snippets based on natural language descriptions
- Explain existing code and suggest improvements
- Debug errors by analyzing stack traces and code patterns

### ☁️ Where {term}`LLMs <LLM>` live: deployment models

**Frontier Models (Cloud-Hosted):**
- **Deployment:** Run on massive server infrastructure by model providers
- **Access:** Pay-per-token via API keys
- **Examples:**
  - **Claude Sonnet 4.5** (Anthropic): Best coding model, \$3/\$15 per M tokens
  - **GPT-5** (OpenAI): Best overall reasoning, \$1.25/\$10 per M tokens
  - **Gemini 2.5 Pro** (Google): Best for speed/context (1M tokens), multimodal
  - **Claude 4 Opus** (Anthropic): Best for long-horizon coding (30+ hour tasks), \$15/\$75 per M tokens
- **Pros:** State-of-the-art capabilities, specialized for different tasks (coding vs reasoning vs speed), no local compute needed
- **Cons:** Requires internet connection, ongoing costs, data leaves your machine

**Mid-tier and efficient models (cloud or local):**
- **Deployment:** Can run on cloud APIs or self-hosted on consumer hardware
- **Examples:**
  - **Claude Haiku**
  - **Qwen3-30B-A3B** (approaching GPT-4o performance),
  - **Mistral Small 3.2**
  - **Llama 3.3-70B**
- **Pros:** Lower cost or free (if self-hosted), faster responses, good balance of capability and efficiency
- **Cons:** Less capable than frontier models, self-hosting requires GPU resources (typically 16GB+ {term}`VRAM`)


**Open-source & open-weight models (2025 state-of-the-art):**
- **Licenses:** Vary from fully open (Apache 2.0, MIT) to restricted commercial use
- **Deployment:** Can be self-hosted using tools like [Ollama](https://ollama.com/), [LM Studio](https://lmstudio.ai/), or [vLLM](https://github.com/vllm-project/vllm)
- **Examples:**
  - **Qwen3-235B-A22B** (Apache 2.0 license): 235B params with 22B active, 262K context, exceptional reasoning
  - **GLM-4.5** (Open License): Strong coding and agentic abilities, runs on consumer hardware
  - **GLM-4.5 Air**: Optimized for 48GB RAM laptops when quantized
  - **Qwen3-Coder**: Specialized for code generation tasks
  - **DeepSeek-R1**: 671B params (37B active), MIT license, advanced reasoning (86.7% on AIME)
  - **OpenAI GPT-OSS-120B/20B** (Apache 2.0): Near o4-mini performance, consumer-friendly
- **Pros:** Full control, no API costs, data stays local, latest open models often outperform closed frontier ones
- **Cons:** Requires technical setup and adequate hardware (16GB+ VRAM for smaller models, 48GB+ for larger ones)

:::{dropdown} 💡 Self-Hosting {term}`LLMs <LLM>` (Optional)
If you want to run models locally (for privacy or cost savings), tools like [Ollama](https://ollama.com/) make it easy:

```bash
# Install Ollama
# macOS: Download from https://ollama.com/download/mac
# Windows: Download from https://ollama.com/download/windows
# Linux:
curl -fsSL https://ollama.com/install.sh | sh

# Download and run a recommended coding model
# For powerful machines (24GB+ VRAM):
ollama run qwen3-coder

# For laptops/consumer hardware (16GB RAM):
ollama run glm-4.5-air

# For reasoning tasks:
ollama run deepseek-r1

# Use with Cursor/Cline via OpenAI-compatible API
# Point to http://localhost:11434/v1
```

**Model Selection Guide:**
- **Best for coding on powerful hardware:** Qwen3-235B or GLM-4.5
- **Best for laptops (48GB RAM):** GLM-4.5 Air (quantized)
- **Best for consumer GPUs (16-24GB):** Qwen3-Coder or DeepSeek-R1-Distill
- **Budget option:** GPT-OSS-20B (runs on 16GB RAM)

Most AI tools can be "coerced" into using local models by configuring them to point to an OpenAI-compatible API endpoint.
:::


### 🛠️ AI tools for extension development

Not all AI coding tools are created equal. **In this workshop, we'll use {term}`agentic AI <agentic AI>` tools** that can understand your codebase, execute commands, and iterate with you—a fundamentally different and more productive experience than chat or autocomplete.

:::{note} Why Agentic AI? Detailed comparison of AI tool categories
:class: dropdown

#### 🥉 Chat-based AI (ChatGPT, Claude web interface)

**How it works:** You paste code snippets → AI gives you code back

**Use cases:**
- ✅ Quick one-off questions ("How do I use async/await in TypeScript?")
- ✅ Explaining error messages
- ✅ Learning new concepts

**Limitations:**
- ❌ Doesn't understand your codebase
- ❌ Can't run tests or verify solutions
- ❌ Every conversation starts from scratch
- ❌ You're the one integrating fragments into your project

**Verdict:** Good for learning, frustrating for building features.

#### 🥈 Autocomplete AI (GitHub Copilot basic mode)

**How it works:**
- Suggests code as you type (like enhanced IntelliSense)
- Predicts what you'll write next based on context

**Use cases:**
- ✅ Writing boilerplate code
- ✅ Completing obvious patterns

**Limitations:**
- ⚠️ Often 90% right (which means constantly fighting it)
- ⚠️ Interrupts your flow with suggestions
- ⚠️ No understanding of "correctness" — just statistical likelihood
- ⚠️ Can be distracting or helpful depending on your preference.

**Verdict:** Some people really prefer this mode (i.e. experienced developers who know when to ignore it, someone who prefers light touch AI interactions), but it can slow down beginners.

#### 🥇 Agentic AI (Cursor, Claude Code, Cline, GitHub Copilot Workspace)

**How it works:**
- Understands your entire codebase
- Can execute commands (build, test, format)
- Reads documentation and error messages
- Works iteratively with you

**Use cases:**
- ✅ Building complete features from requirements
- ✅ Refactoring across multiple files
- ✅ Debugging with full context
- ✅ Generating tests based on implementation
- ✅ Updating documentation alongside code

**Key difference:** It doesn't just suggest — it **acts** like a team member with tools.

**Verdict:** When building real features, prioritize agentic tools. They understand your codebase, execute commands, and iterate with you—a fundamentally different (and more productive) experience than chat or autocomplete.
:::

We'll work with **Cursor** to demonstrate the AI-assisted workflow, then repeat key steps using **Claude Code** for a CLI-based approach. Both tools offer similar capabilities, so you can choose whichever fits your preferred workflow after the workshop.

#### 1. 🖱️ **Cursor**
- **What it is:** A fork of VS Code with deep AI integration
- **Pricing:** Free Hobby plan (includes one-week Pro trial, limited agent requests, and limited tab completions). Paid plans \$20-200/mo offer extended/unlimited usage limits and faster response times. See [cursor.com/pricing](https://cursor.com/pricing) for details.
- **LLM Options:**
  - Built-in models (Claude Sonnet 4.5, GPT-5, Gemini 2.5 Pro and more) with Cursor subscription
- **Best for:** Developers who want a polished, GUI-driven experience
- **Alternatives:** [Windsurf](https://codeium.com/windsurf) (free tier, \$15/mo Pro), [GitHub Copilot Workspace](https://github.com/features/copilot) (\$10-39/mo), [Cline](https://cline.bot/) (VS Code extension, free), [Continue](https://continue.dev/) (VS Code/JetBrains extension, free or \$10/mo Teams), [Roo Code](https://roocode.com/) (VS Code extension, free or \$20/mo Pro), [Kilocode](https://kilocode.ai/) (VS Code/JetBrains, free or \$29/user/mo Teams), [Replit Agent](https://replit.com/) (cloud-based)
- **Download:** [cursor.com](https://cursor.com/)

#### 2. 💻 **Claude Code**
- **What it is:** Command-line interface for Claude, optimized for coding workflows
- **LLM Options:**
  - Requires Claude subscription or Anthropic API key. Can also work through cloud providers, like Amazon Bedrock
  - Works with Opus 4.1, Sonnet 4.5, Haiku 4.5, and other Claude models
- **Best for:** CLI warriors who live in the terminal
- **Alternatives:** [Gemini CLI](https://github.com/google-gemini/gemini-cli) (free tier available), [Cline](https://github.com/cline/cline) (VS Code extension with CLI mode, free), [Continue](https://github.com/continuedev/continue) (IDE/terminal/CI agent, free), [Plandex](https://github.com/plandex-ai/plandex) (designed for large projects), [aichat](https://github.com/sigoden/aichat) (all-in-one LLM CLI), [GitHub Copilot CLI](https://github.com/github/copilot-cli), [Aider](https://github.com/Aider-AI/aider) (Git-integrated, open-source), [Google Jules](https://jules.google/) (async background agent, beta)
- **Install:** Requires Node.js, then `npm install --global @anthropic/claude-code`

:::{note} Further Reading
:class: dropdown
- [Simon Willison's blog on AI-assisted programming](https://simonwillison.net/tags/ai-assisted-programming/) — Practical insights from a prolific developer
- ["Not all AI-assisted programming is vibe coding (but vibe coding rocks)"](https://simonwillison.net/2025/Mar/19/vibe-coding/) — Understanding different AI coding styles
- [Claude Sonnet 4.5 review](https://simonwillison.net/2025/Sep/29/claude-sonnet-4-5/) — Simon calls it "the best coding model in the world"
- [Parallel Coding Agents](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/) — Simon's October 2025 workflow with Claude Sonnet 4.5
- [Simon Willison's Model Recommendations](https://simonwillison.net/2025/Oct/) — Qwen3 and GLM-4.5 for open-source coding; Chinese models often outperform Western alternatives
- [Ollama Documentation](https://ollama.com/) — Self-hosting open-source models
- [LM Studio](https://lmstudio.ai/) — GUI for running local LLMs
:::

## 🏁 Getting started

### 📦 Repo
For this module, we will start with an existing extension that we built in chapter 2. If you are not caught up or just joining us for the afternoon session, please grab a reference implementation from [our demo repository](https://github.com/mfisher87/jupytercon2025-developingextensions-demo).

In {doc}`02-anatomy-of-extensions`, we started off by cloning an official [JupyterLab extension template](https://github.com/jupyterlab/extension-template).
This template was recently enhanced to include AI-specific configurations and rulesets.
Then, we built a JupyterLab extension that displays random images with captions from a curated collection.

Now, we'll use AI to extend this viewer with image editing capabilities.

#### 🔄 Option 1: Continue with your own extension

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

4. Skip to [AI tool](#ai-tool) below.

#### 📥 Option 2: Clone the finished extension

If you'd prefer to start fresh or didn't complete the anatomy module:

1. Clone the demo repository:

   ```bash
   cd ~/Projects
   git clone https://github.com/jupytercon/jupytercon2025-developingextensions-demo.git jupytercon2025-ai-workshop
   cd jupytercon2025-ai-workshop
   ```


3. Install and verify the extension works:

   ```bash
   # Create/activate environment
   micromamba create -n jupytercon2025 python pip nodejs=22 gh "copier~=9.2" jinja2-time
   micromamba activate jupytercon2025

   # Install the extension in development mode
   pip install --editable ".[dev,test]"
   jupyter labextension develop . --overwrite
   jupyter server extension enable jupytercon2025_extension_workshop

   # Build and start JupyterLab
   jlpm build
   jupyter lab
   ```

Make sure your git tree is clean, there are no unsaved and uncommitted files. This is going to be important later

(ai-tool)=
### ⚙️ AI tool

We will be using Cursor and Claude Code throughout this tutorial. Please install them if you would like to follow along.

You are totally welcome to use any AI tool you have installed on your computer! Many of them follow similar patterns and expose similar functionality.

#### 🎨 Setting up Cursor

1. **Download Cursor**
   - Visit [cursor.com](https://cursor.com/download) and download the installer for your operating system
   - Install Cursor like any other application

2. **Create a Cursor account**
   - Launch Cursor
   - You'll be prompted to sign in or create an account
   - Sign up for a free account
   - The [Hobby plan](https://cursor.com/pricing) includes a one-week Pro trial

:::{tip}
We recommend you sign up for a free Hobby plan for this workshop! You'll have one week to access agentic AI features.
:::

#### ⌨️ Setting up Claude Code

1. **Use your workshop environment**
   - You already have Node.js 22 installed in your `jupytercon2025` environment
   - Make sure the environment is activated:
   ```bash
   micromamba activate jupytercon2025
   ```

2. **Install Claude Code**
   ```bash
   npm install --global @anthropic/claude-code
   ```

3. **Set up Claude subscription or Anthropic API key**
   - More details to be added

By now, you should have:
- ✅ Cursor installed with a free account created
- ✅ Claude Code installed with an API key configured
- ✅ Your extension repository open and ready to work with

## 📋 Exercise A (15 minutes): Understand AI rules

Before jumping into code generation, let's set up the "invisible infrastructure" that makes AI assistants work well. This configuration is what makes the difference between mediocre and excellent AI-generated code.

### AI rules

AI Rules (also called Cursor Rules, or system prompts) are instructions that automatically precede every conversation with your AI assistant. They're like permanent coaching that guides the AI's behavior.

**AGENTS.md: The Emerging Standard**

In 2025, the AI coding ecosystem converged on **AGENTS.md** as the universal format for agent instructions. Emerging as an open standard with OpenAI convening an industry working group and growing adoption across the ecosystem, AGENTS.md replaces fragmented tool-specific formats—it's just plain Markdown, no special schemas needed.

**Tool Support Status:**

| Tool | Support | Format |
|------|---------|--------|
| **Cursor** | ✅ Native | AGENTS.md + .cursor/rules/ |
| **GitHub Copilot** | ✅ Native | AGENTS.md (maintains .github/copilot-instructions.md for backward compatibility) |
| **Zed Editor** | ✅ Native | AGENTS.md |
| **Roo Code** | ✅ Native | AGENTS.md |
| **Claude Code** | ⚙️ Via symlink | Create: `ln -s AGENTS.md CLAUDE.md` |
| **Gemini CLI** | ⚙️ Via symlink | Create: `ln -s AGENTS.md GEMINI.md` |
| **Aider** | ⚙️ Config | Add to .aider.conf.yml: `read: AGENTS.md` |
| **Continue.dev** | ❌ Not yet | Use .continue/rules/ |
| **Cline** | ❌ Not yet | Use .clinerules/rules.md |

**Key advantage:** Single AGENTS.md file works across your entire team regardless of which AI tool they use—no more maintaining separate .cursorrules, CLAUDE.md, and GEMINI.md files.

For this workshop, the official copier template provides AGENTS.md and can create symlinks for Claude Code and Gemini CLI. You should already have these rules configured in your repo if you selected 'Y' on the copier's question about AI tools. Let's understand what's there and why it helps.

### What's in Your AGENTS.md File

:::{tip} The Magic Behind Good AI Code
When you see AI generate well-structured JupyterLab code in exercises, it's not magic - it's reading your AGENTS.md file! These rules are why AI knows to:
- Use `jlpm` instead of `npm`
- Put commands in `src/index.ts`
- Extend the right base classes
- Follow JupyterLab naming patterns

Without proper AI context that AGENTS.md provides, we observed AI generating generic code, or using the wrong package manager or thinking too much about source vs. prebuilt extensions.
:::

1. **Open Cursor app**
2. **Set up cursor cli command**

3. **Open your extension folder in Cursor**

```bash
cursor ~/Projects/jupytercon2025-extension-workshop
```

4. **Take a moment to get familiar with the interface**
   Main area for coding tabs, left side panel for file browser and extensions, right side panel for a chat interface. All should look very similar to VSCode or JupyterLab.

5. **Check that the rules file exists:**
   Look for `AGENTS.md` file in your extension root

   :::{important}
   If neither `AGENTS.md` exists, **let an instructor know!** These files contain important context and rules that help AI assistants generate better JupyterLab extension code.
   :::

6. **Review the ruleset file**
   Open `AGENTS.md`
   Key sections you'll find:

   **Code Quality Rules:**
   - Use structured logging (not `console.log()`)
   - Define explicit TypeScript interfaces
   - Avoid the `any` type
   - Use typeguards over type casts

   **Naming Conventions:**
   - Python: `snake_case` for functions, `PascalCase` for classes
   - TypeScript: `camelCase` for variables, `PascalCase` for classes
   - No mixing styles!

   **Project Structure Guidelines:**
   - Frontend code in `src/`
   - Backend Python in `<extension_name>/`
   - Commands registered in `src/index.ts`
   - Routes in `<extension_name>/routes.py`

   **JupyterLab-Specific Patterns:**
   - How to register commands
   - When to use `ReactWidget` vs `Widget`
   - REST API best practices with `ServerConnection`
   - State persistence with `IStateDB`

   **Development Workflow:**
   - When to run `jlpm build` (TypeScript changes)
   - When to restart Jupyter (Python changes)
   - How to debug (browser console, terminal logs)

   **Common Pitfalls to Avoid:**
   - ❌ Don't use `document.getElementById()` (use JupyterLab APIs)
   - ❌ Don't hardcode URLs (use `ServerConnection.makeSettings()`)
   - ❌ Don't forget `dispose()` methods (prevents memory leaks)
   - ❌ Don't mix `npm` and `jlpm` (use `jlpm` only)

**Why this matters:** These rules teach AI the JupyterLab patterns **before** it writes any code. Without them, AI might use generic React patterns or wrong APIs. With them, AI generates code that follows JupyterLab conventions from the start.

### Customize your AGENTS.md

It can be helpful to modify the provided generic "JupyterLab extension" AI rules to include your favorite tools, package managers, and conventions.

Let's modify the rules to include the package manager we are using and the environment name,
so that the Cursor would have an easier time building our extension.

Open your `AGENTS.md` file and find the "Environment Activation (CRITICAL)" section. Modify it to specify our workshop environment:

```diff
 ### Environment Activation (CRITICAL)

 **Before ANY command**, ensure you're in the correct environment:

-```bash
-# For conda/mamba/micromamba (replace `conda` with `mamba` or `micromamba` depending on the prompter's preferred tool):
-conda activate <environment-name>
-
-# For venv:
-source <path-to-venv>/bin/activate  # On macOS/Linux
-<path-to-venv>\Scripts\activate.bat # On Windows
-```
+Use micromamba:
+```bash
+micromamba activate jupytercon2025
+```

 **All `jlpm`, `pip`, and `jupyter` commands MUST run within the activated environment.**
```

This tells the AI assistant to use `micromamba` with the `jupytercon2025` environment that we're using in this workshop, making it easier for the AI to run build commands correctly.

### Verify that Cursor recognizes the rules

1. Open the Cursor Chat panel (`Cmd/Ctrl + L`) and choose Ask Mode

:::{note}
**⚠️ Avoid "Auto" mode** — it picks the cheapest model, not the best one.

**Recommended Models:**
- **GPT-5** for planning and reasoning tasks
- **Claude Sonnet 4.5** for coding tasks

These models provide the best balance of quality and capability for JupyterLab extension development.
:::

:::{dropdown} More Details on Model Selection
**Model selection impacts both quality and cost.**

1. **Enable model selector:**
   - Settings → Cursor Settings → General
   - Find "Usage Summary" → Set to "Always" (not "Auto")
   - This shows your credit usage at bottom of chat panel

2. **Choose models strategically:**

   | Task | Recommended Model | Why |
   |------|------------------|-----|
   | Planning & Reasoning | GPT-5 or Claude Sonnet 4.5 (Thinking) | GPT-5 leads reasoning benchmarks; Claude excellent for extended thinking |
   | Coding (Best Overall) | Claude Sonnet 4.5 | "Best coding model in the world" per Simon Willison; 99.29% safety rate |
   | Long Coding Sessions | Claude Opus 4.1 | Sustains focus for 30+ hours, ideal for large refactors and multi-step tasks |
   | Speed & Long Context | Gemini 2.5 Pro | 1M token context, sub-second streaming, best latency |
   | Quick fixes | Claude Haiku 4.5 or GPT-5 Mini | Faster, cheaper for simple edits and routine tasks |
   | Local Development | GLM-4.5 Air, Qwen3-235B, or DeepSeek-R1 | Best open models for self-hosting on consumer hardware |
   | Avoid | Auto | Cursor picks cheapest, not best |

3. **Watch your context usage:**
   - Look for percentage in chat (e.g., "23.4%")
   - Keep under 50% for best results
   - Above 70%? Start a new chat

4. **Monitor credits:**
   - Check `cursor.com/settings` → Usage
   - Typical costs: Planning ($1-2), Implementation ($0.30-0.50)

**Start Big, Optimize Later**
For planning and architecture, always use the highest-quality model available:
- **Cloud:** GPT-5 for reasoning, Claude Sonnet 4.5 for coding
- **Self-hosted:** DeepSeek-R1 or Qwen3-235B-A22B

You can downgrade to faster/cheaper models (Claude Haiku 4.5, GPT-5 Mini, or GLM-4.5 Air) for routine edits, but don't skimp on the thinking phase.
:::

2. Ask AI chat questions to verify that it recognizes the rules. Paste the below prompt into the chat

   ```
   What package manager should I use for JupyterLab extension frontend?
   ```

   :::{note} No Visual Indicator
   Cursor automatically reads and applies AGENTS.md, but there's **no visual indicator** in the interface showing it's active.
   [![The Cursor interface showing the user promopt "Can I use package-lock.json with this repo?" with a part of the AI model "thinking" tokens saying "From the rules section, there's a clear directive about package management: ✅ Do: Use jlpm exclusively"](../assets/images/implicit-agents-md.png)
   :::

   AI should respond with `jlpm`, not `npm` or `yarn` - that comes from your AGENTS.md rules!

   :::{tip} If AI gives wrong answers
   - Restart Cursor
   - Make sure AGENTS.md is in your project root (not a subdirectory)
   - Check that the file is named exactly `AGENTS.md` (case-sensitive)
   :::

3. Get ready for the development. Start a new chat and choose Agent Mode:
   ```
   Prepare my extension for development:
   1. Check that I am using the correct environment `jupytercon2025`
   2. Check that my environment have JupyterLab and nodejs installed
   3. Check that TypeScript is configured correctly
   4. Verify extension is installed
   5. Build my extension
   ```

   AI should respond with:
   - Checking your enviro
   - Mentions rules from AGENTS.md (strict mode, camelCase, etc.)
   - Shows awareness of JupyterLab patterns

:::{tip} You're Ready!
Once AI can reference your project structure, coding rules, and build your project, you're set up for success. This configuration is what separates "AI that generates random code" from "AI that writes code that matches your project's patterns."
:::


## 🏗️ Exercise B (30 minutes): Build it!
### Understanding your starting point

Before we extend the functionality, a quick reminder on what the extension currently does:

**Current Features:**
- ✅ Displays random images from a curated collection
- ✅ Shows captions for each image
- ✅ Refresh button to load a new random image
- ✅ Layout restoration (widget persists across JupyterLab sessions)

%**Frontend (TypeScript):**
%- `src/index.ts`: Plugin registration, command definitions, launcher/palette integration
%- `src/widget.ts`: Main UI widget that displays images and captions
%- `src/request.ts`: Utility for communicating with the backend API
%
%**Backend (Python):**
%- `jupytercon2025_extension_workshop/routes.py`: REST API handlers
%- `jupytercon2025_extension_workshop/images_and_captions.py`: Image metadata
%- `jupytercon2025_extension_workshop/images/`: Image files on disk

### Goal: Add Image Editing Capabilities

We'll use AI to extend this viewer with basic image editing features. This exercise demonstrates:
- Power and limitation of a single prompt approach to building features
- The iterative development cycle: plan → implement → test → refine
- How to debug and fix issues with AI assistance
- Monitoring AI context

### Power and peril of one-shot prompts

Before we dive into our structured approach, let's witness what modern AI can accomplish with a single, well-crafted prompt. This demonstration shows both the impressive capabilities and important limitations of AI-driven development.

With the right context and a detailed prompt, AI can build complete features in minutes. Here's a prompt that could generate our entire image editing extension:

```
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
```

### What Happens with This Prompt?

When you give this prompt to an AI agent like Cursor or Claude Code, it will typically:

1. **Analyze your existing codebase** to understand the current structure
2. **Make architectural decisions** about implementation patterns
3. **Generate 200+ lines of code** across multiple files
4. **Update dependencies** in pyproject.toml
5. **Create new endpoints** in your backend
6. **Modify the frontend widget** with new UI controls
7. **Run build commands** to verify everything compiles

Send the prompt and watch as it generates the entire feature. **In about 2-3 minutes**, you will have a fully functional image editor!

**Review the generated code**
- Cursor will suggest changes across multiple files
- Read through each change carefully
- Notice the architectural choices
- Look for comments explaining the approach
- Check if dependencies were added correctly

**Accept or modify** the suggestions
- Click "Accept" to apply all changes
- Or click individual files to review and edit before accepting

**Test the functionality:**
```bash
jlpm build
jupyter lab
```

**Test the new features**:
- Open the image viewer widget
- Try each filter button
- Check the browser console for errors (`F12` or `Cmd+Option+I`)
- Check the terminal running `jupyter lab` for Python errors
- Find at least 3 decisions you might have made differently

### The Hidden Cost: Decisions Made Without You

While impressive, this one-shot approach makes numerous decisions on your behalf:

**Architecture Decisions:**
- ❓ Base64 encoding vs. temporary file URLs?
- ❓ Stateful vs. stateless image processing?
- ❓ Where to store edited images?

**UI/UX Decisions:**
- ❓ Button placement and styling
- ❓ Slider ranges and defaults
- ❓ Error message presentation
- ❓ Loading state indicators

**Technical Implementation:**
- ❓ PIL filter parameters (blur radius, sharpen intensity)
- ❓ Image format handling (JPEG quality, PNG transparency)
- ❓ Memory management for large images
- ❓ Caching strategy for processed images

**Code Quality:**
- ❓ Error handling approach
- ❓ TypeScript type definitions
- ❓ Test coverage

:::{warning} The Product Manager Trap
When you use one-shot prompts, you're essentially saying: "AI, you be the product manager, architect, and developer all at once."

This works great for prototypes, but in production code, you need to understand and own these decisions.
:::

### Basic debugging

**The debugging workflow:** Don't manually debug—let AI help! It can read error messages, understand context, and propose fixes.

### Roll back when done

To undo all changes made by the one-shot prompt:

```bash
# Discard all changes to tracked files
git restore .

# Remove any new untracked files created by the AI
git clean -fd
```

## 📊 Exercise C (20 minutes): Product manager framework

### The better way: structured, iterative development

While one-shot prompts are impressive for demos, professional development requires a more thoughtful approach. We'll now proceed with a structured workflow that:

1. **Plans before coding** - Understand the architecture first
2. **Implements in phases** - Build incrementally with checkpoints
3. **Reviews each step** - Catch issues early
4. **Maintains control** - You make the key decisions
5. **Manages AI context** - Start with fresh chats for each phase

This takes longer but results in:
- ✅ Code you understand and can maintain
- ✅ Architecture that fits your needs
- ✅ Proper error handling and edge cases
- ✅ Learning opportunities at each step

### The rise of the Product Manager mindset
AI works best with detailed specifications, not agile "figure it out as we go." Embrace structured planning.

Before generating any code, we'll have AI create a phased implementation plan. This:
- ✅ Keeps AI focused and prevents scope creep
- ✅ Gives you a roadmap to refer back to
- ✅ Makes it easy to resume work across sessions
- ✅ Documents architectural decisions

1. **Create a plans directory:**

   ```bash
   mkdir plans
   ```

2. **Start a new chat in Cursor** and use this prompt:

   ````
   I'm extending a JupyterLab image viewer to add image editing capabilities.

   Please create a detailed implementation plan and save it to plans/image-editing-feature.md

   **Requirements:**
   - Add filter buttons (grayscale, sepia, blur, sharpen)
   - Use Pillow (PIL) on the backend for processing
   - New REST endpoint `/edit-image` for transformations
   - Update frontend to display edited images immediately
   - Basic crop functionality (50% from center)
   - Brightness/contrast sliders
   - Save edited image back to disk

   **DO NOT WRITE CODE YET.** Create a phased plan with:

   **Phase 1: MVP**
   - Basic filter buttons (grayscale, sepia)
   - Backend endpoint scaffolding
   - Frontend display of processed images

   **Phase 2: Advanced Filters**
   - Blur and sharpen filters
   - Crop functionality
   - Brightness/contrast adjustments

   **Phase 3: Polish**
   - Save functionality
   - Undo/redo buttons
   - Loading states and error handling

   For each phase, list:
   - Specific files to create/modify
   - Python/TypeScript dependencies needed
   - Testing approach
   - Potential issues to watch for

   Save this plan to plans/image-editing-feature.md
   ````

3. **Review the plan:**
   - Open `plans/image-editing-feature.md`
   - Read through each phase
   - Ask questions if anything is unclear:
     ```
     In Phase 1, why did you choose to handle images as base64?
     What are the alternatives?
     ```

4. **Commit the plan:**

   ```bash
   git add plans/image-editing-feature.md
   git commit -m "Add implementation plan for image editing feature"
   ```

**Why this matters:** You now have a versioned plan that AI (and you) can reference. As you work through phases, AI will stay focused on the current step.

:::{tip} Planning Mode vs. File-Based Plans
Cursor has a "Plan" mode that creates temporary plans. **Don't use it.** File-based plans are:
- ✅ Versioned in Git
- ✅ Synced across machines
- ✅ Reviewable and editable
- ✅ Persistent across sessions

Always save plans to files: `plans/*.md`

### Implement Phase by Phase

:::{tip} Managing Context and Costs
Instead of one long chat for everything, start fresh for each phase.

Why new chats? It has do with LLMs context window. Saturating the context window leads to AI confusion and runs up the costs. File-based plans allow us to start new chats and still keep the required context.

As you work through phases, keep an eye on these metrics:

**Context window percentage** (shown in Cursor chat):
- **< 30%:** Healthy, plenty of room
- **30-50%:** Good, AI still focused
- **50-70%:** Getting crowded, consider new chat soon
- **> 70%:** Start new chat immediately

**When to start a new chat:**
- ✅ Completed a phase (Phase 1 → Phase 2)
- ✅ Switching focus areas (backend → frontend)
- ✅ Context above 50%
- ✅ AI seems confused or gives inconsistent answers
- ✅ Major refactoring complete
:::

1. **Start a NEW chat** for Phase 1 (`Cmd/Ctrl + K`)

2. **Reference the plan:**

   ```
   We are ready for Phase 1 of @plans/image-editing-feature.md

   Please implement the MVP: basic grayscale and sepia filters with
   backend endpoint and frontend display.
   ```

   Note the `@plans/...` syntax tells AI to read that specific file.

3. **Review changes in Source Control** (keep this panel open!)

4. **Commit after Phase 1 works:**

   ```bash
   git add .
   git commit -m "Phase 1: Add basic image filters (grayscale, sepia)"
   ```

5. **Start ANOTHER fresh chat for Phase 2:**

   ```
   We are ready for Phase 2 of @plans/image-editing-feature.md

   Phase 1 is complete. Now implement advanced filters (blur, sharpen, crop).
   ```

6. **Commit after Phase 2 works:**

   ```bash
   git add .
   git commit -m "Phase 2: Add advanced filters (blur, sharpen, crop)"
   ```

::: Adopt a "product manager" mindset

:::{important} 💾 **Final Git commit and push!**
```bash
git add .
git commit -m "Complete Exercise 1: Image editing with AI assistance"
git push
```
:::

## 🖥️  Demo: AI from the command line (10 minutes)

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

## 🤔 Reflection and next steps

Phew! 😮‍💨 That was a lot! Now we've completed our exercises let's take a moment to reflect:

### 💭 Quick reflection

Think about these questions — we'll discuss as a group:

1. **What surprised you most about working with AI?**
   - Did it understand JupyterLab patterns better or worse than expected?
   - Were there moments where it "just got it" vs. moments where you had to guide it heavily?

2. **Which technique was most valuable for you?**
   - Planning first with phased implementation?
   - Using screenshots for UI debugging?
   - Starting fresh chats to manage context?
   - The AGENTS.md rules and documentation setup?

3. **What would you do differently next time?**
   - More detailed planning upfront?
   - Smaller phases?
   - Different prompting approach?

:::{note} Group Discussion
We'll share experiences as a group and create a live poll about which techniques resonated most. Your instructor will facilitate this discussion.
:::

### 🔑 Key takeaways

### 🎓 Challenge extensions (optional)

---

## 🎯 What's next?

You've now experienced the complete AI-assisted development workflow:
- ✅ Used AI to generate code for new features
- ✅ Debugged issues by iterating on prompts
- ✅ Learned to provide effective context and constraints
- ✅ Understood when to accept AI suggestions vs. when to customize

### 🌟 Continuing your journey

The next chapter provides **independent exploration time** where you can:

1. **Build your own extension from scratch** - Using the template and proven project ideas
2. **Contribute to existing extensions** - Give back to the community and learn from production code

Choose the path that interests you most, work at your own pace, and instructors will be available to help when you get stuck.

:::{tip} Before Moving On
Take a 5-minute break to:
- Stretch and rest your eyes
- Reflect on what you learned in Exercise 1
- Think about what you'd like to build or explore next
- Grab water or coffee ☕

See you in the next chapter for independent work time!
:::
