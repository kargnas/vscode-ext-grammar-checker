# AI-Ready Development Guide

> **Purpose**: This guide helps you quickly set up an AI-ready development environment for AI agents like Codex, OpenCode, Cursor Agent, and similar tools.

## Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                    AI Grammar Corrector                              │
│                  VS Code Extension Project                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌──────────────┐     ┌──────────────┐     ┌──────────────┐        │
│   │  TypeScript  │ ──▶ │   Compile    │ ──▶ │   VS Code    │        │
│   │    Source    │     │   (tsc)      │     │  Extension   │        │
│   └──────────────┘     └──────────────┘     └──────────────┘        │
│         │                                                            │
│         ▼                                                            │
│   ┌──────────────┐     ┌──────────────┐                             │
│   │   ESLint     │     │    Test      │                             │
│   │   (lint)     │     │  (optional)  │                             │
│   └──────────────┘     └──────────────┘                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## Quick Start for AI Agents

### 1. Environment Setup

**Requirements:**
- Node.js 18+ (pre-installed on most AI agent environments)
- npm (comes with Node.js)

**No external services required!** This is a VS Code extension that uses user-configured API keys.

### 2. Codex Cloud Setup Script

Copy the following to your Codex Cloud setup script:

```bash
#!/bin/bash
# Codex Cloud Setup Script for AI Grammar Corrector

# Install dependencies
npm install

# Copy AI-ready environment
cp -n .env.ai-ready .env 2>/dev/null || true

# Compile TypeScript
npm run compile

# Run lint to verify code quality
npm run lint
```

### 3. Maintenance Script (After Branch Checkout)

```bash
#!/bin/bash
# Run after checking out a new branch

npm install
npm run compile
```

## Environment Secrets

For AI agents that need to **test** the extension with real API calls:

| Secret Name | Description | Required |
|-------------|-------------|----------|
| `OPENAI_API_KEY` | OpenAI API key for testing | Optional* |

> *Testing is optional. The extension compiles and packages without API keys.

## Development Commands

| Command | Description |
|---------|-------------|
| `npm install` | Install dependencies |
| `npm run compile` | Compile TypeScript to JavaScript |
| `npm run watch` | Watch mode - auto-recompile on changes |
| `npm run lint` | Run ESLint checks |
| `npm run package` | Create .vsix package file |

## Project Structure

```
ai-grammar-corrector/
├── src/                     # TypeScript source files
│   ├── extension.ts         # Main entry point
│   ├── llmService.ts        # LLM API communication
│   ├── diagnosticsProvider.ts # VS Code diagnostics
│   ├── prompts.ts           # LLM prompts & i18n
│   └── types.ts             # TypeScript interfaces
├── out/                     # Compiled JavaScript (gitignored)
├── package.json             # Extension manifest & dependencies
├── tsconfig.json            # TypeScript configuration
├── .eslintrc.json           # ESLint configuration
└── .vscode/                 # VS Code workspace settings
```

## Testing the Extension

Since this is a VS Code extension, testing requires either:

1. **VS Code Extension Host** (press F5 in VS Code)
2. **Manual VSIX install**: `npm run package` then install in VS Code

For automated testing, the project has test scaffolding in `launch.json`, but actual test files are not yet implemented.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Build fails | Run `npm install` first |
| Lint errors | Run `npm run lint` to see details |
| TypeScript errors | Check `tsconfig.json` compatibility |

---

**See also:** [README.md](./README.md) for general project information.
