# 🤖 Developing extensions with AI assistance

::::{hint} Learning objectives
- Use an AI assistant (Cursor, Claude Code, etc.) to explore, understand, and modify JupyterLab extensions by prompting
- Write effective prompts with context, constraints, target files, and acceptance criteria; iterate intentionally
- Point AI to JupyterLab documentation and API reference: provide URLs or local paths, require citations to the exact section, and verify types against your installed version
- Use AI to locate extension points (commands, tokens, services, widgets) and read the relevant APIs
- Generate and refine TypeScript/React UI, commands, settings schemas, and Python server endpoints with AI; review and edit code as needed
- Run build, lint, and tests with AI guidance; interpret errors and request focused fixes
- Generate tests, docs, and PR text with AI aligned to repository conventions
- Use and customize a local AI ruleset (AGENTS.md) in your editor/tool of choice; understand how it works and how to adjust it
- Understand privacy, licensing, and safety considerations when sharing code/data with AI tools
::::

::::{tip} Outcome
After this module, you will have:
- Configured a local development environment with AI tools (Cursor, Claude Code, etc.) and the AGENTS.md ruleset
- Either created a new extension from the template with at least one user-visible feature (e.g., a new command plus a small widget or server endpoint), or contributed a meaningful enhancement to an existing extension
- A reproducible prompt checklist/playbook with doc/API citations that you can reuse for future extension work
- Either published your new extension to PyPI and GitHub with a proper README, or opened a pull request to an existing extension repository with a clear description and passing tests
- Confidence to continue exploring extension ideas primarily by prompting, while being able to understand and edit the generated code
::::
