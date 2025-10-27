# 🚀 Advanced AI Techniques for JupyterLab Extension Development

**Note:** This is advanced material for experienced developers who have completed the main AI tutorial. Not required for the workshop.

## Prerequisites

Before diving into these advanced techniques, you should have:
- ✅ Completed Exercise 0 (AI configuration)
- ✅ Completed Exercise 1 (Image editing with AI)
- ✅ Comfortable with AGENTS.md rules and documentation setup
- ✅ Experienced the basic AI workflow (plan → implement → test)

## 🎯 Exercise: Build Reusable AI Workflows with Slash Commands

**Time:** 30-45 minutes

### What are Slash Commands?

Slash commands let you create repeatable AI tasks as shortcuts. Instead of typing the same instructions repeatedly, you define them once and invoke with `/command-name`.

**Where they live:**
- Project-specific: `.cursor/commands/*.md`
- Global (all projects): `~/.cursor/commands/*.md`

**When to use them:**
- ✅ You find yourself giving the same instructions to AI repeatedly
- ✅ You want to standardize code review processes
- ✅ You're working with a team and want consistent AI interactions
- ✅ You want to build a library of debugging/testing workflows

**When NOT to use them:**
- ❌ During initial learning (adds complexity)
- ❌ For one-off tasks
- ❌ When you're still figuring out what prompts work

### Part 1: Create Your First Slash Command - Extension Health Check

**Goal:** Build a command that systematically checks extension configuration.

1. **Create the commands directory:**

   ```bash
   mkdir -p .cursor/commands
   ```

2. **Create `.cursor/commands/extension-check.md`:**

   ````markdown
   ---
   name: extension-check
   description: Verify JupyterLab extension is properly configured and built
   ---

   # JupyterLab Extension Health Check

   Review this JupyterLab extension and verify:

   ## 1. Build Configuration
   - `package.json` has proper `@jupyterlab/builder` setup
   - `pyproject.toml` includes correct metadata and dependencies
   - `tsconfig.json` is configured with strict mode
   - All required dev dependencies are listed

   ## 2. Development Install Status
   - Python package is editable-installed (`pip install -e`)
   - Extension is linked with `jupyter labextension develop`
   - Server extension is enabled (if applicable)
   - All dependencies are installed

   ## 3. Code Quality
   - Run `jlpm build` and report any TypeScript errors
   - Check for Python linting issues
   - Look for console warnings in code
   - Verify proper error handling in async functions

   ## 4. Extension Registration
   - Plugin properly exports `JupyterFrontEndPlugin`
   - All commands are registered in `src/index.ts`
   - Server extension handlers are registered (if backend exists)
   - Widget lifecycle methods implemented (dispose, etc.)

   ## 5. Testing
   - Test files exist and follow naming conventions
   - Run tests if possible and report results
   - Check test coverage if available

   Provide a summary with:
   - ✅ What's working well
   - ⚠️ Warnings or potential issues
   - ❌ Critical problems that need fixing
   - 📝 Recommendations for improvement

   Do not make changes, just report findings.
   ````

3. **Test the command:**

   In Cursor chat, type:
   ```
   /extension-check
   ```

   AI will systematically verify your extension without you typing the whole checklist.

4. **Commit it:**

   ```bash
   git add .cursor/commands/extension-check.md
   git commit -m "Add extension health check slash command"
   ```

### Part 2: Build a Test Review Command

Create `.cursor/commands/test-review.md`:

````markdown
---
name: test-review
description: Review test coverage and quality
---

# Test Suite Review

Act as a software quality engineer reviewing our test suite:

1. **Coverage Analysis:**
   - Run `jlpm test` (if tests exist)
   - Identify untested code paths
   - Highlight critical features without tests

2. **Test Quality:**
   - Check for proper mocking of external dependencies
   - Verify both success and error paths are tested
   - Look for flaky tests (time-dependent, order-dependent)
   - Ensure tests are independent (can run in any order)

3. **Best Practices:**
   - Tests follow naming conventions (`test_*.py`, `*.spec.ts`)
   - Test data is isolated (no shared state)
   - Assertions are specific (not just "truthy" checks)
   - Edge cases are covered (empty input, null, undefined)

4. **Recommendations:**
   - List top 3 areas where tests would add most value
   - Suggest specific test cases to add
   - Identify over-tested areas (diminishing returns)

Provide actionable summary. Do not write tests, just analyze.
````

**Usage:**
```
/test-review

Then ask follow-up: "What's our biggest testing gap?"
```

### Part 3: Create a Debug Helper Command

Create `.cursor/commands/debug-help.md`:

````markdown
---
name: debug-help
description: Analyze build or runtime errors systematically
---

# Debug Assistant

Help me debug an issue. Please:

1. **Understand the Error:**
   - What type of error is this? (TypeScript, Python, Runtime, etc.)
   - What's the root cause vs. symptoms?
   - What part of the system is affected?

2. **Analyze Context:**
   - Check related code for obvious issues
   - Look for recent changes that might have caused this
   - Identify missing imports, type errors, or async issues

3. **Propose Solutions:**
   - Suggest 2-3 possible fixes, ranked by likelihood
   - Explain trade-offs of each approach
   - Cite relevant documentation if applicable

4. **Prevention:**
   - How can we prevent this in the future?
   - Should we add tests? Linting rules? Type guards?

After analysis, ask if I want you to implement the fix.
````

**Usage:**
```
/debug-help

[Paste error message]

AI will systematically diagnose and propose fixes
```

### Part 4: API Usage Checker

Create `.cursor/commands/api-usage.md`:

````markdown
---
name: api-usage
description: Verify correct use of JupyterLab APIs
---

# JupyterLab API Usage Review

Review the code for proper use of JupyterLab APIs:

1. **Widget Lifecycle:**
   - Widgets properly dispose resources in `dispose()`
   - No memory leaks from event listeners
   - State is saved/restored correctly

2. **Command Registry:**
   - Commands use unique IDs (namespaced)
   - Command handlers return Promises where appropriate
   - Commands are properly toggled/enabled/disabled

3. **REST API Calls:**
   - Using `ServerConnection.makeSettings()` not hardcoded URLs
   - Proper error handling on all requests
   - Request/response types are defined
   - CORS is handled correctly

4. **State Management:**
   - Using `IStateDB` for persistence (not localStorage)
   - State keys are unique and namespaced
   - State is cleaned up when widget is disposed

5. **Styling:**
   - Using JupyterLab CSS variables (not hardcoded colors)
   - Proper use of lumino CSS classes
   - Dark mode compatibility

Cite official JupyterLab documentation for recommendations.
Report findings without making changes.
````

### Part 5: Composing Commands

You can use multiple slash commands in one chat:

```
/extension-check

[Review results]

/test-review

[Review test analysis]

Based on both reviews, what are the top 3 priorities to fix?
```

This creates a systematic code review workflow.

### Part 6: Share with Your Team

**Commit all commands:**

```bash
git add .cursor/commands/
git commit -m "Add reusable AI slash commands for extension development"
git push
```

Now your entire team can use these workflows!

### Advanced: Build Your Command Library

Over time, create slash commands for:

- `/refactor-suggestion` - Identify code smells and suggest refactoring
- `/performance-review` - Find optimization opportunities
- `/security-check` - Look for security vulnerabilities
- `/accessibility-audit` - Check UI accessibility compliance
- `/docs-update` - Update README/docstrings after code changes
- `/breaking-changes` - Analyze API changes for backward compatibility

These become your personal AI-powered code review team.

### Best Practices for Slash Commands

**DO:**
- ✅ Start with commands you use repeatedly
- ✅ Make them focused (one clear purpose)
- ✅ Include clear success criteria
- ✅ Ask AI to report without changing (for review commands)
- ✅ Share with team via Git

**DON'T:**
- ❌ Create commands before understanding the workflow
- ❌ Make overly complex commands (break into smaller ones)
- ❌ Duplicate what AI already knows (use AGENTS.md instead)
- ❌ Create commands for one-off tasks

## Key Takeaways

✅ **You've learned to:**
- Create reusable AI workflows with slash commands
- Build a systematic code review process
- Share AI workflows across your team
- Compose multiple commands for comprehensive reviews

⚠️ **Remember:**
- Slash commands are for repeated workflows, not initial learning
- Start simple (extension-check) before building complex commands
- Commands should complement AGENTS.md rules, not duplicate them
- The best commands emerge from real repeated needs

🎯 **When to use this:**
- You're working on multiple JupyterLab extensions
- You want consistent code review standards
- You're onboarding new team members
- You've established patterns you want to enforce

---

**This advanced material is not required for the workshop. It's provided for reference when you're ready to optimize your AI workflows in production environments.**

