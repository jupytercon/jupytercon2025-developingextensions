# 🐞 4 - Debugging: What to do when things go wrong?

:::{hint} Learning objectives
- Understand how to read and interpret JupyterLab extension error
- Learn to use browser developer tools for frontend debugging
- Identify and resolve common extension development issues
:::

:::{tip} Outcome
By the end of this module, you'll be able to debug JupyterLab extensions using
multiple tools and techniques.
:::

## Introduction

You might find yourself spending a significant portion of your development time debugging an extension that is not building, or some functionality that is now broken. With the right tools and approach,
debugging can be an experience that leaves you feeling accomplished rather than frustrated. In this section we will
go over some common issues that you might encounter when working on extensions, and a few ways in which you can
tackle them.

## 1. Understanding Error Messages

### Types of Errors
- Build-time errors vs runtime errors
- Python backend errors vs TypeScript frontend errors

### Reading Stack Traces
- Minified vs source-mapped traces
- JavaScript stack traces
- Identifying the source of the error

## 2. Terminal

### Terminal Logs
- Where to find JupyterLab server logs

## 3. Browser Developer Tools

### Console Tab
- Viewing runtime errors and warnings
- Using `console.log()`, `console.error()`, and `console.warn()`
    - Filtering console messages
- Preserving logs across reloads
- Structured logging for better debugging
- JupyterLab's internal logging <!-- TODO: brief, make reference to section 02 -->

### Network Tab
- Checking extension resource loading
- Monitoring API requests to JupyterLab backend
- Inspecting request/response payloads
- Debugging failed network requests

### Sources Tab with Source Maps
- Source maps are enabled by default if you're working from the extension-template
- Setting breakpoints
- Stepping through code execution

### Elements/Inspector Tab
- Inspecting DOM elements created by your extension
- Examining CSS styles and computed values
- Modifying elements for quick testing

## 4. Best Practices

### Debugging Workflow
- Start simple: `console.log` first
- Progress to browser DevTools
- Use browser debugger breakpoints for complex issues
- Keep development environment consistent

### Prevention is better
- TypeScript type checking
- Linting and formatting
- Unit testing critical functionality
- Clear error messages in your code

## 5. Hands-on Exercise

:::{exercise} Debug a Broken Extension
Practice debugging techniques by:
1. Identifying errors in a provided broken extension
2. Using console logs to trace execution
3. Setting breakpoints with browser DevTools
4. Fixing the issues step by step
:::

### Instructions
1. Copy the file at: TBD
2. Past the file into the file: TBD
TBD...

## Common Debugging Scenarios

### Extension Not Loading
- Checking build output
- Verifying installation
- Inspecting browser console for load errors

### UI Not Updating
- State management issues
- React re-rendering problems
- CSS specificity conflicts

### Backend Communication Issues
- Verify API endpoints
- CORS and authentication problems
- Request/response format mismatches

## Additional Resources
- [JupyterLab Developer Guide](https://jupyterlab.readthedocs.io/en/stable/extension/extension_dev.html)
- [Chrome DevTools Documentation](https://developer.chrome.com/docs/devtools/)
