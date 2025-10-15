# 🤖 Developing extensions with AI assistance

::::{hint} Learning objectives
- Use an AI assistant (Cursor, Claude Code, etc.) to build and modify JupyterLab extensions by writing effective prompts with context, constraints, and acceptance criteria; point AI to official docs/APIs and require citations
- Generate and refine extension code (TypeScript/React UI, commands, settings schemas, Python endpoints) with AI guidance; run build/lint/tests and interpret errors to request focused fixes
- Configure and customize a local AI ruleset (AGENTS.md) for your development workflow; understand privacy, licensing, and safety considerations when sharing code with AI tools
::::
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
- **Examples:** OpenAI GPT-4, Claude 3.5 Sonnet, Google Gemini
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
- **Already using VS Code?** Try **Cline** for seamless integration
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
