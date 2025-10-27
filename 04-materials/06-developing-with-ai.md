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

### Setting Expectations: Your AI Partner is Like a Skilled Junior Developer

Before we dive into tools and techniques, let's set the right mindset for working with AI.

**Don't expect perfection.** If AI gets 95% of what you need correct, that's incredible—exactly what you'd get from a talented junior developer given the same instructions.

**Key mental model:** Think of your AI assistant as a fast, eager junior developer who:
- ✅ Learns quickly and has read tons of documentation (but maybe not the latest stuff)
- ✅ Can scaffold code and explore APIs incredibly fast
- ✅ Sometimes makes mistakes or misunderstands requirements
- ✅ Benefits from clear instructions and iterative feedback
- ✅ Gets better with guidance and context

**The right mindset:**
- Don't get frustrated by small mistakes—iterate and guide
- Review all generated code (you wouldn't merge a PR without review)
- Ask questions: "Why did you choose this approach?"
- Treat errors as learning opportunities for both you and the AI
- Don't be afraid to roll back to the beginning and start over if AI doubled down on a wrong path

:::{important} Perfection is Off the Mark
You would never hire a developer and expect absolute perfection in their creative work. Apply the same perspective to AI. If it gets most things right and you refine the rest, that's a massive productivity win.
:::

### Understanding LLMs (Large Language Models)

AI coding assistants are powered by **Large Language Models (LLMs)** — neural networks trained on vast amounts of text and code. These models can:
- Understand context from your codebase
- Generate code snippets based on natural language descriptions
- Explain existing code and suggest improvements
- Debug errors by analyzing stack traces and code patterns

### Where LLMs Live: Deployment Models

**Frontier Models (Cloud-Hosted):**
- **Examples:** 
  - **Claude Sonnet 4.5** (Anthropic): Best coding model, $3/$15 per M tokens
  - **GPT-5** (OpenAI): Best overall reasoning, $1.25/$10 per M tokens
  - **Gemini 2.5 Pro** (Google): Best for speed/context (1M tokens), multimodal
  - **Claude 4 Opus** (Anthropic): Best for long-horizon coding (30+ hour tasks), $15/$75 per M tokens
- **Deployment:** Run on massive server infrastructure by model providers
- **Access:** Pay-per-token via API keys
- **Pros:** State-of-the-art capabilities, specialized for different tasks (coding vs reasoning vs speed), no local compute needed
- **Cons:** Requires internet connection, ongoing costs, data leaves your machine

**Mid-Tier and Efficient Models (Cloud or Local):**
- **Examples:** Claude Haiku, Qwen3-30B-A3B (approaching GPT-4o performance), Mistral Small 3.2, Llama 3.3-70B
- **Deployment:** Can run on cloud APIs or self-hosted on consumer hardware
- **Pros:** Lower cost or free (if self-hosted), faster responses, good balance of capability and efficiency
- **Cons:** Less capable than frontier models, self-hosting requires GPU resources (typically 16GB+ VRAM)

**Open-Source & Open-Weight Models (2025 State-of-the-Art):**
- **Highly Recommended for Coding:**
  - **Qwen3-235B-A22B** (Apache 2.0): 235B params with 22B active, 262K context, exceptional reasoning
  - **GLM-4.5** (Open License): Strong coding and agentic abilities, runs on consumer hardware
  - **GLM-4.5 Air**: Optimized for 48GB RAM laptops when quantized
  - **Qwen3-Coder**: Specialized for code generation tasks
  - **DeepSeek-R1**: 671B params (37B active), MIT license, advanced reasoning (86.7% on AIME)
  - **OpenAI GPT-OSS-120B/20B** (Apache 2.0): Near o4-mini performance, consumer-friendly
- **Other Notable Models:** Llama 3.3-70B (Meta), Mistral Small 3.2, Gemma 3 (Google)
- **Licenses:** Vary from fully open (Apache 2.0, MIT) to restricted commercial use
- **Deployment:** Can be self-hosted using tools like [Ollama](https://ollama.com/), [LM Studio](https://lmstudio.ai/), or [vLLM](https://github.com/vllm-project/vllm)
- **Pros:** Full control, no API costs, data stays local, latest Chinese models often outperform Western alternatives
- **Cons:** Requires technical setup and adequate hardware (16GB+ VRAM for smaller models, 48GB+ for larger ones)

### Understanding AI Coding Tool Categories

Not all AI coding tools are created equal. Understanding the different categories helps you choose the right tool and set appropriate expectations.

#### ❌ Chat-Based AI (ChatGPT, Claude web interface)

**How it works:**
- You paste code snippets → AI gives you code back
- No context of your full project structure
- Requires manual copy/paste workflow

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

#### ⚠️ Autocomplete AI (GitHub Copilot basic mode)

**How it works:**
- Suggests code as you type (like enhanced IntelliSense)
- Predicts what you'll write next based on context

**Use cases:**
- ✅ Writing boilerplate code
- ✅ Completing obvious patterns

**Limitations:**
- ⚠️ Often 90% right (which means constantly fighting it)
- ⚠️ Interrupts your flow with suggestions
- ⚠️ No understanding of "correctness"—just statistical likelihood
- ⚠️ Can be distracting or helpful depending on your preference.

**Verdict:** Some people really prefer this mode (i.e. experienced developers who know when to ignore it, someone who prefers light touch AI interactions), but it can slow down beginners.

#### ✅ Agentic AI (Cursor, Claude Code, Cline, GitHub Copilot Workspace)

**How it works:**
- Understands your entire codebase
- Can execute commands (build, test, format)
- Reads documentation and error messages
- Self-corrects when things go wrong
- Works iteratively with you

**Use cases:**
- ✅ Building complete features from requirements
- ✅ Refactoring across multiple files
- ✅ Debugging with full context
- ✅ Generating tests based on implementation
- ✅ Updating documentation alongside code

**Key difference:** It doesn't just suggest—it **acts** like a team member with tools.

**Example workflow:**
```
You: "Add image filters to this widget. Use Pillow on the backend."

Agentic AI:
1. Reads your widget.ts to understand structure
2. Checks pyproject.toml for dependencies
3. Adds Pillow to dependencies
4. Creates new endpoint in routes.py
5. Updates widget with filter buttons
6. Runs `jlpm build` to verify TypeScript compiles
7. Reports: "Done. Found one type error, fixed it."
```

**Verdict:** **This is what we're using in this workshop.** Agentic AI is a fundamentally different experience from chat or autocomplete.

:::{tip} Focus on Agentic Tools
If you've tried AI coding before and found it frustrating, chances are you were using chat-based AI or basic autocomplete. The techniques in this workshop are designed for **agentic, tool-using AI** that can understand and operate on your full project.
:::

### AI IDEs and Tools for Extension Development

Now that you understand the categories, here are the **agentic AI tools** you can use in this workshop, ranging from GUI-first to CLI-native:

#### 1. **Cursor** (Popular UI-First IDE)
- **What it is:** A fork of VS Code with deep AI integration
- **LLM Options:** 
  - Built-in models (Claude Sonnet 4.5, GPT-5, Gemini 2.5 Pro) with Cursor subscription (starting $20/month)
  - Bring your own API key (OpenAI, Anthropic, Google, or other providers)
  - Can be configured to use local models (Qwen3-Coder, GLM-4.5, DeepSeek-R1) via OpenAI-compatible APIs
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
  - Works with Claude Sonnet 4.5, Claude 4 Opus, and other Claude models
- **Best for:** CLI warriors who live in the terminal; Simon Willison's primary coding tool
- **Install:** Requires Node.js 18+, then `npm install -g @anthropic/claude-code`

:::{tip} Self-Hosting LLMs
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

According to Simon Willison's 2025 recommendations, Chinese models (Qwen, GLM) often outperform Western alternatives (Llama, Mistral, Gemma) for coding tasks.

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
- [Claude Sonnet 4.5 review](https://simonwillison.net/2025/Sep/29/claude-sonnet-4-5/) — Simon calls it "the best coding model in the world"
- [Parallel Coding Agents](https://simonwillison.net/2025/Oct/5/parallel-coding-agents/) — Simon's October 2025 workflow with Claude Sonnet 4.5
- [Simon Willison's Model Recommendations](https://simonwillison.net/2025/Oct/) — Qwen3 and GLM-4.5 for open-source coding; Chinese models often outperform Western alternatives
- [Ollama Documentation](https://ollama.com/) — Self-hosting open-source models
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


## 🔧 Exercise 0 (15 minutes): Configure Your AI Development Environment

Before jumping into code generation, let's set up the "invisible infrastructure" that makes AI assistants work well. This configuration is what makes the difference between mediocre and excellent AI-generated code.

### Pre-Exercise Checklist

Make sure you have:

- [ ] Git repository initialized with clean working tree
- [ ] Your AI tool installed (Cursor, Claude Code, or Cline)
- [ ] Extension template cloned or previous exercise code available
- [ ] Basic understanding: AI is a junior developer (95% accuracy = success!)

### Part 1: Understand Your AI Rules (AGENTS.md)

**What are AI Rules?**

AI Rules (also called Cursor Rules, or system prompts) are instructions that automatically precede every conversation with your AI assistant. They're like permanent coaching that guides the AI's behavior.

**AGENTS.md: The Emerging Standard**

In 2025, the AI coding ecosystem converged on **AGENTS.md** as the universal format for agent instructions. Created through collaboration between OpenAI, Google, Sourcegraph, and others, AGENTS.md replaces fragmented tool-specific formats—it's just plain Markdown, no special schemas needed.

**Tool Support Status:**

| Tool | Support | Format |
|------|---------|--------|
| **Cursor** | ✅ Native | AGENTS.md + .cursor/rules/ |
| **GitHub Copilot** | ✅ Native | AGENTS.md (maintains .github/copilot-instructions.md for backward compatibility) |
| **Zed Editor** | ✅ Native | AGENTS.md |
| **Roo Code** | ✅ Native | AGENTS.md |
| **Claude Code** | ⚙️ Via symlink | Create: `ln -s AGENTS.md CLAUDE.md` |
| **Gemini CLI** | ⚙️ Config | Add to .gemini/settings.json: `{"context": {"fileName": ["AGENTS.md"]}}` |
| **Aider** | ⚙️ Config | Add to .aider.conf.yml: `read: AGENTS.md` |
| **Continue.dev** | ❌ Not yet | Use .continue/rules/ |
| **Cline** | ❌ Not yet | Use .clinerules/rules.md |

**Key advantage:** Single AGENTS.md file works across your entire team regardless of which AI tool they use—no more maintaining separate .cursorrules, CLAUDE.md, and GEMINI.md files.


For this workshop, the official copier template provides AGENTS.md and can create symlinks for Claude Code and Gemini CLI. You should already have these rules configured in your repo if you selected 'Y' on the copier's question about AI tools. Let's understand what's there and why it helps.

#### What's in Your AGENTS.md File

1. **Check that the file exists:**

   ```bash
   ls -la AGENTS.md
   cat AGENTS.md  # Review the contents
   ```

2. **Key sections you'll find:**

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

3. **Verify AI recognizes the rules:**

   :::{note} No Visual Indicator
   Cursor automatically reads and applies AGENTS.md, but there's **no visual indicator** in the interface showing it's active.
   [![The Cursor interface showing the user promopt "Can I use package-lock.json with this repo?" with a part of the AI model "thinking" tokens saying "From the rules section, there's a clear directive about package management: ✅ Do: Use jlpm exclusively"](../assets/images/implicit-agents-md.png)
   :::
   
   Open Cursor, start a new chat (Ask Mode), and ask:
   ```
   What package manager should I use for JupyterLab extensions?
   ```
   
   AI should respond with `jlpm`, not `npm` or `yarn` - that comes from your AGENTS.md rules!

   Try another test:
   ```
   What TypeScript conventions should we follow?
   ```
   
   AI should mention strict mode, camelCase, avoiding `any` type, etc.
   
   **If AI gives wrong answers** (like suggesting `npm` instead of `jlpm`):
   - Restart Cursor
   - Make sure AGENTS.md is in your project root (not a subdirectory)
   - Check that the file is named exactly `AGENTS.md` (case-sensitive)

:::{tip} The Magic Behind Good AI Code
When you see AI generate well-structured JupyterLab code in exercises, it's not magic - it's reading your AGENTS.md file! These rules are why AI knows to:
- Use `jlpm` instead of `npm`
- Put commands in `src/index.ts`  
- Extend the right base classes
- Follow JupyterLab naming patterns

Without proper AI context that AGENTS.md provides, we observed AI generating generic code, or using the wrong package manager or thinking too much about source vs. prebuilt extensions.
:::

If you'd like, you can modify the provided AI rules to include your favorite tools, package managers, and conventions.

### Part 2: Register Official Documentation

AI models may not have current JupyterLab documentation in their training data. Let's fix that:

#### For Cursor Users:

1. **Open Cursor Settings:**
   - macOS: `Cmd + ,` then click "Cursor Settings" (not regular Settings)
   - Windows/Linux: `Ctrl + ,` then click "Cursor Settings"

2. **Navigate to Docs:**
   - Settings → Cursor → Docs
   - Click "Add Docs"

3. **Add these documentation sources:**

   | Name | URL |
   |------|-----|
   | JupyterLab | `https://jupyterlab.readthedocs.io/` |
   | Lumino | `https://lumino.readthedocs.io/` |
   | Jupyter Server | `https://jupyter-server.readthedocs.io/` |

4. **Add dependency documentation** (if using these libraries):

   | Name | URL | When to add |
   |------|-----|-------------|
   | Pillow | `https://pillow.readthedocs.io/` | Image processing |
   | Pandas | `https://pandas.pydata.org/docs/` | Data manipulation |
   | Plotly | `https://plotly.com/python/` | Interactive plots |

5. **Test documentation integration:**

   In Cursor chat:
   ```
   Using @JupyterLab documentation, how do I create a command that opens a new widget?
   ```
   
   AI should reference actual JupyterLab APIs and link to docs.

#### For Claude Code / Cline Users:

Documentation integration requires additional setup:

- **Claude Code:** Documentation is NOT built into Claude's training. You need to:
  - Enable web search in settings, OR
  - Use MCP (Model Context Protocol) tools to fetch documentation, OR
  - Manually paste relevant documentation into your prompts
- **Cline:** Can be configured to access web docs via settings

### Part 3: Set Up Your Git Workflow

**Critical:** Your Git game must be top-shelf when working with AI.

1. **Enable source control panel** (Cursor/VS Code):
   - Open the Source Control view (`Cmd/Ctrl + Shift + G`)
   - Keep this panel visible alongside your AI chat

2. **Understand the Four Safety Levels:**

   ```
   Level 1: Unsaved      →  Files on disk (Cmd/Ctrl + Z to undo)
   Level 2: Staged       →  git add (can unstage)
   Level 3: Committed    →  git commit (can reset)
   Level 4: Pushed       →  git push (permanent)
   ```

3. **Adopt this workflow:**

   ```bash
   # After AI generates code:
   # 1. Review changes in Source Control panel
   
   # 2. Stage changes you like:
   git add src/widget.ts  # stage individual files
   
   # 3. If AI continues and breaks something:
   git restore src/widget.ts  # revert to staged version
   
   # 4. Once everything works:
   git commit -m "Add image filter buttons with AI assistance"
   
   # 5. If you want to undo a commit:
   git reset --soft HEAD~1  # keep changes, undo commit
   ```

4. **Configure Git to show better diffs:**

   ```bash
   # Better word-level diffs for TypeScript/Python
   git config --global diff.algorithm histogram
   
   # Show function names in diff headers
   git config --global diff.python.xfuncname '^[ \t]*((class|def)[ \t].*)$'
   ```

:::{danger} Git is Your Safety Net
AI can suggest code that breaks your extension. With frequent commits and staging, you can fearlessly experiment and roll back instantly. **This is not optional**—it's how you work safely with AI.
:::

### Part 4: Choose Your AI Model Wisely

**Model selection impacts both quality and cost.**

#### For Cursor Users:

1. **Enable model selector:**
   - Settings → Cursor Settings → General
   - Find "Usage Summary" → Set to "Always" (not "Auto")
   - This shows your credit usage at bottom of chat panel

2. **Choose models strategically:**

   | Task | Recommended Model | Why |
   |------|------------------|-----|
   | Planning & Reasoning | GPT-5 or Claude Sonnet 4.5 (Thinking) | GPT-5 leads reasoning benchmarks; Claude excellent for extended thinking |
   | Coding (Best Overall) | Claude Sonnet 4.5 | "Best coding model in the world" per Simon Willison; 99.29% safety rate |
   | Long Coding Sessions | Claude 4 Opus | Sustains focus for 30+ hours, ideal for large refactors and multi-step tasks |
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

:::{tip} Start Big, Optimize Later
For planning and architecture, always use the highest-quality model available:
- **Cloud:** GPT-5 for reasoning, Claude Sonnet 4.5 for coding
- **Self-hosted:** DeepSeek-R1 or Qwen3-235B-A22B
  
You can downgrade to faster/cheaper models (Claude Haiku 4.5, GPT-5 Mini, or GLM-4.5 Air) for routine edits, but don't skimp on the thinking phase.
:::

### Verification: Is Your Environment Ready?

Run this quick test to verify everything is configured:

1. **Open your extension in Cursor/AI tool**

2. **Start a new chat and ask:**

   ```
   Please verify my JupyterLab development setup:
   1. Check that TypeScript is configured correctly
   2. Verify Python package structure
   3. List the coding standards I should follow
   4. Tell me what documentation you have access to
   ```

3. **AI should respond with:**
   - ✅ References to your project structure
   - ✅ Mentions rules from AGENTS.md (strict mode, camelCase, etc.)
   - ✅ Shows awareness of JupyterLab patterns
   - ✅ (If using Cursor) Mentions registered documentation

4. **If something's missing:**
   - Restart Cursor/AI tool
   - Check that AGENTS.md or .cursor/rules exist
   - Verify documentation was added to Cursor settings

:::{admonition} You're Ready!
:class: tip
Once AI can reference your project structure, coding rules, and JupyterLab documentation, you're set up for success. This configuration is what separates "AI that generates random code" from "AI that writes code that matches your project's patterns."
:::

---

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
- **Planning first, coding second** - Let AI architect before implementing
- How to communicate complex feature requirements to an AI assistant
- How AI can help navigate unfamiliar APIs (PIL/Pillow for image processing)
- The iterative development cycle: plan → implement → test → refine
- How to debug and fix issues with AI assistance
- Managing AI context with fresh chats for new phases

### Demo: The Power (and Peril) of One-Shot Prompts

Before we dive into our structured approach, let's witness what modern AI can accomplish with a single, well-crafted prompt. This demonstration shows both the impressive capabilities and important limitations of AI-driven development.

:::{admonition} One-Shot Prompt Magic
:class: note

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
:::

#### What Happens with This Prompt?

When you give this prompt to an AI agent like Cursor or Claude Code, it will typically:

1. **Analyze your existing codebase** to understand the current structure
2. **Make architectural decisions** about implementation patterns
3. **Generate 200+ lines of code** across multiple files
4. **Update dependencies** in pyproject.toml
5. **Create new endpoints** in your backend
6. **Modify the frontend widget** with new UI controls
7. **Run build commands** to verify everything compiles

**In about 2-3 minutes**, you could have a fully functional image editor!

#### The Hidden Cost: Decisions Made Without You

While impressive, this one-shot approach makes numerous decisions on your behalf:

**Architecture Decisions:**
- ❓ Should images be processed server-side or client-side?
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
- ❓ Python type hints
- ❓ Test coverage

:::{warning} The Product Manager Trap
When you use one-shot prompts, you're essentially saying: "AI, you be the product manager, architect, and developer all at once." 

This works great for prototypes, but in production code, you need to understand and own these decisions.
:::

#### Try It Yourself (Optional Demo)

If you want to experience the one-shot approach:

1. **Create a new branch to experiment:**
   ```bash
   git checkout -b one-shot-demo
   ```

2. **Give your AI the one-shot prompt above**

3. **Watch as it generates the entire feature** (2-3 minutes)

4. **Test the functionality:**
   ```bash
   jlpm build
   jupyter lab
   ```

5. **Examine what was created:**
   - Look at the code structure
   - Notice the architectural choices
   - Find at least 3 decisions you might have made differently

6. **Roll back when done:**
   ```bash
   git diff  # See all the changes
   git checkout .  # Discard all changes
   git checkout main  # Return to main branch
   git branch -D one-shot-demo  # Delete the demo branch
   ```

#### The Better Way: Structured, Iterative Development

While one-shot prompts are impressive for demos, professional development requires a more thoughtful approach. We'll now proceed with a structured workflow that:

1. **Plans before coding** - Understand the architecture first
2. **Implements in phases** - Build incrementally with checkpoints
3. **Reviews each step** - Catch issues early
4. **Maintains control** - You make the key decisions

This takes longer (45 minutes vs. 3 minutes) but results in:
- ✅ Code you understand and can maintain
- ✅ Architecture that fits your needs
- ✅ Proper error handling and edge cases
- ✅ Learning opportunities at each step

### Step 1: Create an Implementation Plan (Don't Skip This!)

**The rise of the Product Manager mindset:** AI works best with detailed specifications, not agile "figure it out as we go." Embrace structured planning.

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
:::

### Step 2: Implement Phase by Phase

**Context window management:** Instead of one long chat for everything, start fresh for each phase.

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

**Why new chats?** Keeps context window small (<50%), prevents AI confusion, reduces costs. File-based plans allow us to start new chats and still keep the required context.

### Step 3: Adopt a "Product Manager" Mindset

**Effective prompts use Product Manager thinking: clear requirements, constraints, and acceptance criteria.**

Instead of:
> "Add some image filters to the viewer"

Try this structure:

````markdown
**User Story:** As a user, I want to apply filters to images before analyzing them.

**Acceptance Criteria:**
- [ ] Clicking "Grayscale" converts image to grayscale immediately
- [ ] Changes are visible without page reload
- [ ] Original image is preserved (can reset)
- [ ] Multiple filters can be applied in sequence
- [ ] Error messages show if filter fails

**Technical Requirements:**
- Backend: Use Pillow's `ImageFilter` module
- API: POST to `/api/<extension>/edit-image`
  - Request: `{filename: string, filter_type: string, params?: object}`
  - Response: `{success: boolean, image_data: string (base64)}`
- Frontend: Add toolbar buttons using `@jupyterlab/apputils` `Toolbar`
- State: Track edit history in widget state

**Non-Requirements (for later):**
- Don't implement save functionality yet (Phase 3)
- Don't need undo/redo (Phase 3)
- Don't worry about performance optimization

**Questions for AI:**
- What's the best way to handle large images?
- Should we cache the base64 data or re-fetch?
- How do we prevent race conditions if user clicks multiple filters quickly?
````

This level of detail helps AI give you exactly what you want.

### The Prompts

We'll demonstrate this development process using two different AI tools:
- **Cursor AI** (IDE-integrated approach)
- **Claude Code** (CLI-based approach)

Here's the implementation prompt for Phase 1 (feel free to adapt it):

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

:::{tip} Prompt Engineering: The Complete Guide
**Effective prompts use the "Product Manager" structure:**

**1. User Story** (sets context):
```
As a data scientist, I want to apply image filters before analysis, 
so I can preprocess images without leaving JupyterLab.
```

**2. Specific Requirements** (what to build):
```
- Add filter buttons: grayscale, sepia, blur, sharpen
- Filters apply immediately (no reload)
- Multiple filters can be chained
- Original image can be restored
```

**3. Technical Constraints** (how to build):
```
- Backend: Use Pillow (PIL) ImageFilter module
- API: POST /api/<extension>/edit-image
- Frontend: Toolbar buttons using @jupyterlab/apputils
- State: Track applied filters for undo
```

**4. Acceptance Criteria** (how to verify):
```
- [ ] Clicking "Grayscale" converts image
- [ ] Change is visible within 500ms
- [ ] Console shows no errors
- [ ] Multiple filters can apply sequentially
```

**5. Non-Requirements** (scope control):
```
- Don't implement save yet (Phase 2)
- Don't worry about performance optimization
- Don't need keyboard shortcuts
```

**6. Questions for AI** (get expert input):
```
- What's the best way to handle large images?
- Should we cache processed images?
- How do we prevent race conditions?
```

**Ask AI to cite documentation:**
- "Reference @JupyterLab docs for widget lifecycle"
- "Link to Pillow API for each filter you use"
- "Show TypeScript type definitions from @jupyterlab/services"

**Request explanations (learn while building):**
- "Explain why you chose this approach over [alternative]"
- "Comment the code explaining the processing pipeline"
- "What are the tradeoffs of backend vs. frontend processing?"

**Provide feedback loops:**
- "This works, but the sepia is too strong—make it 50% less intense"
- "Good approach, but let's refactor to extract the filter logic"
- "Perfect! Now add tests for edge cases"

**The more specific you are, the better AI performs.**
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

**The debugging workflow:** Don't manually debug—let AI help! It can read error messages, understand context, and propose fixes.

#### Issue 1: Build Errors (TypeScript)

**Symptom:** `jlpm build` fails with TypeScript errors

**Instead of manually debugging:**

1. **Copy the full error output**

2. **Ask AI to diagnose and fix:**
   ```
   The build failed with these TypeScript errors:
   
   [paste error output]
   
   Please:
   1. Explain what's causing this
   2. Fix the issues
   3. Explain why the fix works
   ```

**AI will:**
- Identify the root cause (missing import, wrong type, etc.)
- Fix the code across multiple files if needed
- Run `jlpm build` again to verify
- Explain the issue so you learn

**Example error AI handles well:**
```
error TS2339: Property 'filterType' does not exist on type 'IImageEditRequest'
```

AI will add the property to the interface definition automatically.

#### Issue 2: Runtime Errors (Browser Console)

**Symptom:** Extension loads but features don't work; errors in browser console

**Debugging with AI:**

1. **Open browser console** (`F12` or `Cmd+Option+I`)

2. **Copy the error and screenshot the UI state**

3. **Provide both to AI:**
   ```
   The filter buttons don't work. Here's what happens:
   
   [drag screenshot showing the UI]
   
   Console error:
   [paste error]
   
   What's wrong? Fix the issue and explain.
   ```

**AI excels at:**
- Simple async/await syntax errors (missing await, incorrect .then() usage)
- API request failures (CORS, wrong endpoint, malformed JSON)
- Event handler issues (button not triggering callback)
- State management bugs (React re-render issues)

**Pro tip:** Use visual context! Screenshot shows AI exactly what's broken.

#### Issue 3: Backend Errors (Python Traceback)

**Symptom:** Python server crashes or endpoint returns 500 error

**AI debugging workflow:**

1. **Find the full traceback in terminal** (where `jupyter lab` is running)

2. **Copy everything** (don't excerpt—AI needs full context)

3. **Paste into chat:**
   ```
   The /edit-image endpoint is failing. Full traceback:
   
   [paste entire traceback]
   
   The request body is:
   {"filename": "image.jpg", "filter_type": "grayscale"}
   
   Fix the Pillow image processing logic and explain the issue.
   ```

**AI will:**
- Identify which line causes the crash
- Check if it's a logic error, missing dependency, or type issue
- Fix the code
- Suggest tests to prevent regression

**Common issues AI catches:**
- File path errors (image not found)
- Pillow API misuse (wrong filter parameters)
- Missing error handling (unhandled exceptions)
- JSON serialization issues (can't encode PIL.Image objects)

#### Issue 4: Extension Not Loading

**Symptom:** Extension doesn't appear in JupyterLab after build

**Systematic AI debugging:**

```
My extension isn't loading in JupyterLab. Please check:

1. Is the extension properly installed? Run: jupyter labextension list
2. Is the server extension enabled? Run: jupyter server extension list
3. Check the browser console for any plugin loading errors
4. Verify the plugin ID in package.json matches what's exported in src/index.ts

Report findings and fix any issues.
```

**AI will systematically check** each step and fix configuration issues.

#### Issue 5: AI Generates Wrong Code

**Symptom:** AI's solution doesn't match what you asked for

**How to redirect AI:**

❌ **Don't say:** "This is wrong, try again"

✅ **Do say:**
```
This almost works, but I wanted X instead of Y.

Current behavior: When I click "Grayscale", nothing happens
Expected behavior: Image should convert to grayscale immediately

The issue seems to be in the event handler. Can you check if:
- The button's onClick is wired correctly?
- The API request is actually being sent?
- The response is updating the widget state?

Please fix and explain what was wrong.
```

**Provide:**
- Specific behavior delta (current vs. expected)
- Your hypothesis about where the issue is
- Request explanation (AI teaches you debugging)

#### Issue 6: AI Is Confused or Gives Inconsistent Answers

**Symptom:** AI suggests code that contradicts what it just wrote, or seems "lost"

**Likely cause:** Context window is too full (>70%)

**Solution:**
1. **Start a fresh chat** (`Cmd/Ctrl + K`)
2. **Reference your plan:**
   ```
   Continuing work on @plans/image-editing-feature.md
   
   Phase 1 is complete (committed). I'm debugging an issue where [describe issue].
   
   The error is: [paste error]
   ```

3. **Add relevant files to context:**
   - Click `+` in chat
   - Add only the files related to the bug (widget.ts, routes.py)
   - Don't add the entire project

**Fresh context = fresh perspective.**

:::{danger} When AI Debugging Isn't Working
If after 3-4 iterations AI isn't solving the issue:

1. **Take a break** - Fresh eyes help (you and AI)
2. **Read the code yourself** - You might spot the issue
3. **Search documentation** - AI might be hallucinating an API
4. **Ask a human** - Instructors, teammates, Jupyter Zulip chat
5. **Simplify the problem** - Create minimal reproduction

AI is powerful but not omniscient. Sometimes manual debugging is faster.
:::

### AI-Powered Debugging Checklist

Before asking AI for help, gather:

- [ ] Full error message (not just first line)
- [ ] Code context (what file, what function)
- [ ] What you were trying to do
- [ ] What happened instead
- [ ] Steps to reproduce
- [ ] Screenshots (for UI issues)

**More context = better AI debugging.**

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

### Advanced: Visual Design with Screenshots

AI can understand screenshots! This is incredibly powerful for debugging UI issues or communicating design intent.

#### Debugging UI with Screenshots

1. **Take a screenshot** of the issue (Cmd+Shift+4 on Mac, Snipping Tool on Windows)

2. **Drag screenshot into Cursor chat:**

   ```
   The filter button spacing looks off [drag screenshot here]
   
   Please adjust the CSS so buttons have:
   - 8px margin between them
   - 16px padding inside each button
   - Match JupyterLab's standard button styling from @jupyterlab/ui-components
   ```

3. **AI will:**
   - Analyze the visual layout
   - Identify specific CSS issues
   - Propose fixes targeting the exact elements

#### Communicating Design Intent

You can show AI examples of what you want:

```
I want the filter toolbar to look like this [drop screenshot of Photoshop's filter panel]

Match this visual style:
- Icon-based buttons (not text)
- Tooltips on hover
- Grouped by filter category
- Same color scheme as JupyterLab's toolbar
```

AI will interpret the visual design and generate appropriate code.

#### When to Use Screenshots:

- ✅ Layout/spacing problems ("buttons are misaligned")
- ✅ Color/theming issues ("doesn't match JupyterLab theme")
- ✅ Showing desired design ("make it look like this")
- ✅ Demonstrating bugs ("the image disappears when I click this")
- ✅ Component placement ("move this above the image")

#### Example Workflow:

```
1. Implement feature
2. Take screenshot of result
3. Drag to AI: "The buttons should be above the image, not below"
4. AI adjusts layout
5. Reload, take new screenshot
6. Drag to AI: "Perfect, but reduce the spacing by half"
```

**This iterative visual feedback loop is remarkably effective.**

### Managing Context and Costs

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

**How to maintain continuity:**
```
New chat: "We are ready for Phase 2 of @plans/image-editing-feature.md

Phase 1 is complete and committed. Now implementing advanced filters."
```

The plan file provides context without bloating the chat window.

---

## Path B: Claude Code (CLI)

For developers who prefer working in the terminal, Claude Code provides a powerful command-line interface for AI-assisted development.

:::{note} Live Demo
An instructor will demonstrate this path in real-time. Follow along or take notes to try it yourself afterward.
:::

### Setup Claude Code

1. Install Node.js (if not already installed):

   ```bash
   # macOS (using Homebrew)
   brew install node@18
   
   # Ubuntu/Debian
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt install -y nodejs
   
   # Verify installation
   node -v && npm -v
   ```

2. Install Claude Code:

   ```bash
   npm install -g @anthropic/claude-code
   ```

3. Configure your Anthropic API key:

   ```bash
   export ANTHROPIC_API_KEY="your-api-key-here"
   ```

   :::{tip}
   Add this to your `~/.bashrc` or `~/.zshrc` to persist across sessions:
   ```bash
   echo 'export ANTHROPIC_API_KEY="your-key"' >> ~/.zshrc
   ```
   :::

4. Navigate to your extension directory:

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

### Quick Reflection (Optional)

Think about these questions—we'll discuss as a group:

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

### Key Takeaways

✅ **AI excels at:**
- **Scaffolding and boilerplate** - New endpoints, UI components, tests
- **Navigating unfamiliar APIs** - Pillow, new JupyterLab features
- **Systematic tasks** - Following patterns, applying transforms
- **Explanation and education** - "Why did you choose this approach?"
- **Self-correction** - Fixing build errors, addressing type issues
- **Iterative refinement** - Adjusting based on your feedback

⚠️ **AI may struggle with:**
- **Complex architectural decisions** - When to use State DB vs. props
- **Complex async bugs** - Race conditions, timing issues, subtle promise chaining errors
- **Performance optimization** - Knowing when code is "fast enough"
- **Project-specific conventions** - Without AGENTS.md guidance
- **Ambiguous requirements** - "Make it better" vs. specific criteria

🎯 **The sweet spot:**
AI is most effective when you provide:
1. **Clear requirements** (Product Manager mindset)
2. **Project context** (AGENTS.md rules, documentation)
3. **Phased plans** (not trying to do everything at once)
4. **Iterative feedback** (junior developer coaching)
5. **Safety nets** (Git commits, testing)

:::{tip} Best Practices from This Exercise
1. **Plan before coding**: Written plans keep AI focused across sessions
2. **Start fresh chats**: Keep context <50% for best results
3. **Commit frequently**: After every working phase
4. **Review everything**: You wouldn't merge a PR without review
5. **Ask questions**: "Why did you choose X over Y?"
6. **Provide feedback**: "This works, but let's refactor for clarity"
7. **Use visual tools**: Screenshots communicate design intent effectively
8. **Monitor context**: Watch that percentage, start new chats proactively
9. **Document patterns**: Update AGENTS.md when you establish new conventions
10. **Learn from AI**: Request explanations to understand generated code
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

---

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
