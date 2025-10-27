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

:::{note} Inspired by...
:class: dropdown

This module's teaching approach is inspired by:

* [Agentic AI Programming for Python Course](https://training.talkpython.fm/courses/agentic-ai-programming-for-python)
  by TalkPython Training
  * Ideas for teaching AI-assisted coding in 2025
:::


## AI-assisted development in 2025

If you haven't used AI-assisted development tools yet, you're about to experience a significant shift in how you write code. AI coding assistants can help you explore APIs, generate boilerplate, debug errors, and iterate on features much faster than traditional workflows.

### Setting expectations: your AI partner is like a knowledgeable junior developer

Before we dive into tools and techniques, let's set the right mindset for working with AI.

**Don't expect perfection.** If AI gets 95% of what you need correct, that's incredible — exactly what you'd get from a talented junior developer given the same instructions.

**Key mental model:** Think of your AI assistant as a fast, eager junior developer who:
- ✅ Learns quickly and has read tons of documentation (but maybe not the latest stuff)
- ✅ Can scaffold code and explore APIs incredibly fast
- ✅ Sometimes makes mistakes or misunderstands requirements
- ✅ Benefits from clear instructions and iterative feedback
- ✅ Gets better with guidance and context

**The right mindset:**
- Don't get frustrated by small mistakes — iterate and guide
- **Review all generated code** (you wouldn't merge a PR without review)
- Ask questions: "Why did you choose this approach?"
- Treat errors as learning opportunities for both you and the AI
- Don't be afraid to roll back to the beginning and start over if AI doubled down on a wrong path

:::{important} Don't expect perfection
You would never hire a developer and expect absolute perfection in their creative work. Apply the same perspective to AI. If it gets most things right and you refine the rest, that's a massive productivity win.
:::

### Understanding {term}`LLMs <LLM>` (large language models)

AI coding assistants are powered by **Large Language Models ({term}`LLMs <LLM>`)** — neural networks trained on vast amounts of text and code. These models can:
- Understand context from your codebase
- Generate code snippets based on natural language descriptions
- Explain existing code and suggest improvements
- Debug errors by analyzing stack traces and code patterns

### Where {term}`LLMs <LLM>` live: deployment models

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

**Mid-tier and efficient models (cloud or local):**
- **Examples:** Claude Haiku, Qwen3-30B-A3B (approaching GPT-4o performance), Mistral Small 3.2, Llama 3.3-70B
- **Deployment:** Can run on cloud APIs or self-hosted on consumer hardware
- **Pros:** Lower cost or free (if self-hosted), faster responses, good balance of capability and efficiency
- **Cons:** Less capable than frontier models, self-hosting requires GPU resources (typically 16GB+ {term}`VRAM`)

**Open-source & open-weight models (2025 state-of-the-art):**
- **Highly Recommended for Coding:**
  - **Qwen3-235B-A22B** (Apache 2.0 license): 235B params with 22B active, 262K context, exceptional reasoning
  - **GLM-4.5** (Open License): Strong coding and agentic abilities, runs on consumer hardware
  - **GLM-4.5 Air**: Optimized for 48GB RAM laptops when quantized
  - **Qwen3-Coder**: Specialized for code generation tasks
  - **DeepSeek-R1**: 671B params (37B active), MIT license, advanced reasoning (86.7% on AIME)
  - **OpenAI GPT-OSS-120B/20B** (Apache 2.0): Near o4-mini performance, consumer-friendly
- **Other Notable Models:** Llama 3.3-70B (Meta), Mistral Small 3.2, Gemma 3 (Google)
- **Licenses:** Vary from fully open (Apache 2.0, MIT) to restricted commercial use
- **Deployment:** Can be self-hosted using tools like [Ollama](https://ollama.com/), [LM Studio](https://lmstudio.ai/), or [vLLM](https://github.com/vllm-project/vllm)
- **Pros:** Full control, no API costs, data stays local, latest open models often outperform closed frontier ones
- **Cons:** Requires technical setup and adequate hardware (16GB+ VRAM for smaller models, 48GB+ for larger ones)

### Understanding AI coding tool categories

Not all AI coding tools are created equal. Understanding the different categories helps you choose the right tool and set appropriate expectations.

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

### AI IDEs and tools for extension development

Now that you understand the categories, here are the **agentic AI tools** you can use in this workshop, ranging from GUI-first to CLI-native:

#### 1. **Cursor** (popular UI-first IDE)
- **What it is:** A fork of VS Code with deep AI integration
- **LLM Options:**
  - Built-in models (Claude Sonnet 4.5, GPT-5, Gemini 2.5 Pro) with Cursor subscription (starting $20/month)
  - Bring your own API key (OpenAI, Anthropic, Google, or other providers)
  - Can be configured to use local models (Qwen3-Coder, GLM-4.5, DeepSeek-R1) via OpenAI-compatible APIs
- **Best for:** Developers who want a polished, GUI-driven experience
- **Download:** [cursor.com](https://cursor.com/)

#### 2. **Cline** (open-source VS Code extension)
- **What it is:** A VS Code extension (formerly Claude Dev) that provides AI-assisted coding
- **LLM Options:**
  - Bring your own API key (OpenAI, Anthropic, Google, etc.)
  - Supports OpenAI-compatible APIs (e.g., Ollama, LM Studio)
  - Fully open-source and community-driven
- **Best for:** Developers who prefer VS Code and want flexibility
- **Install:** Search "Cline" in VS Code extensions or visit [github.com/cline/cline](https://github.com/cline/cline)

#### 3. **Claude Code** (CLI for power users)
- **What it is:** Command-line interface for Claude, optimized for coding workflows
- **LLM Options:**
  - Requires Anthropic API key
  - Works with Claude Sonnet 4.5, Claude 4 Opus, and other Claude models
- **Best for:** CLI warriors who live in the terminal
- **Install:** Requires Node.js 18+, then `npm install --global @anthropic/claude-code`

:::{tip} Self-Hosting {term}`LLMs <LLM>`
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

### Choosing your tool

For this workshop:
- We will start with with **Cursor**
- We will run through the same steps again using **Claude Code**

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

## Getting started

### Repo
For this module, we will start with an existing extension that we built in chapter 2. If you are not caught up or just joining us for the afternoon session, please grab a reference implementation from [our demo repository](https://github.com/mfisher87/jupytercon2025-developingextensions-demo).

In {doc}`02-anatomy-of-extensions`, we started off by cloning an official [JupyterLab extension template](https://github.com/jupyterlab/extension-template).
This template was recently enhanced to include AI-specific configurations and rulesets.
Then, we built a JupyterLab extension that displays random images with captions from a curated collection.

Now, we'll use AI to extend this viewer with image editing capabilities.

### Option 1: Continue with your own extension

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

### Option 2: Clone the finished extension

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
   micromamba create -n jupytercon2025 python pip nodejs gh "copier~=9.2" jinja2-time
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

### AI tool

We will be using Cursor and Claude Code throughout this tutorial. Please, install them if you would like to follow.

You are totally welcome to use any AI tool you have installed on your computer! Many of them follow similar patterns and expose similar functionality.

## Getting started  (15 minutes)
- install cursor and claude code (existing installed tools are fine to use too)

## Exercise A (15 minutes): Understand AI rules
- Inspect AGENTS.md
- Familiarize yourself with UI of Cursor
- Choose AI model
- Ask AI chat questions to verify that it recognizes the rules

## Exercise B (30 minutes): Build it!
- Discuss our goal briefly (go from image viewer to image editor)
- Send a single prompt to Cursor
- Basic debugging
- Power and peril of one-shot prompts
- git restore . to undo the changes made by the one-shot prompt?

### Exercise C (20 minutes): Product manager framework
- Learn how to use structured approach to AI-assisted development
- User stories

###  Demo: AI from the command line (10 minutes)
- Demonstrate using Claude Code for development workflow
- Study hall is a good time to try it out

## Reflection and next steps

After completing this exercise (via either path), take a moment to reflect:

### Quick reflection (optional)

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

### Key takeaways

### Challenge extensions (optional)


:::{important} 💾 **Final Git commit and push!**
```bash
git add .
git commit -m "Complete Exercise 1: Image editing with AI assistance"
git push
```
:::

---

## What's next?

You've now experienced the complete AI-assisted development workflow:
- ✅ Used AI to generate code for new features
- ✅ Debugged issues by iterating on prompts
- ✅ Learned to provide effective context and constraints
- ✅ Understood when to accept AI suggestions vs. when to customize

### Continuing your journey

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
