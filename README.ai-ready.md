# AI-Ready Development Guide

> **Purpose**: This guide helps you quickly set up an AI-ready development environment for AI agents like Codex, OpenCode, Cursor Agent, and similar tools. All instructions are designed for zero-configuration startup.

## Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      AI Grammar Corrector                                │
│                    VS Code Extension Project                             │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│   ┌────────────┐      ┌────────────┐      ┌────────────┐                │
│   │ TypeScript │  ──▶ │  Compile   │  ──▶ │  VS Code   │                │
│   │   Source   │      │   (tsc)    │      │ Extension  │                │
│   │  src/*.ts  │      │            │      │  out/*.js  │                │
│   └────────────┘      └────────────┘      └────────────┘                │
│         │                                        │                       │
│         ▼                                        ▼                       │
│   ┌────────────┐                          ┌────────────┐                │
│   │  ESLint    │                          │   VSIX     │                │
│   │   Check    │                          │  Package   │                │
│   └────────────┘                          └────────────┘                │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

## Requirements

| Requirement | Version | Note |
|-------------|---------|------|
| Node.js | 18+ | Pre-installed on most AI environments |
| npm | 8+ | Comes with Node.js |

**No external services required!** This is a VS Code extension - users configure their own API keys.

---

## Codex Cloud Setup

### Setup Script (runs once on workspace init)

```bash
#!/bin/bash
# Codex Cloud Setup Script - AI Grammar Corrector
# Ubuntu 24.04 based, no Docker available

# Install Node.js dependencies
npm install

# Copy AI-ready environment if .env doesn't exist
cp -n .env.ai-ready .env 2>/dev/null || true

# Compile TypeScript
npm run compile

# Verify with lint
npm run lint

echo "Setup complete!"
```

### Maintenance Script (after branch checkout)

```bash
#!/bin/bash
# Run after checking out a new branch

npm install
npm run compile
```

---

## Environment Configuration

### Required Secrets (for testing only)

| Secret | Description | Required |
|--------|-------------|----------|
| `OPENAI_API_KEY` | OpenAI API key for live testing | Optional |
| `OPENROUTER_API_KEY` | OpenRouter API key | Optional |

> The extension compiles and packages without any API keys. Keys are only needed for live grammar checking tests.

### Environment Files

| File | Purpose |
|------|---------|
| `.env.example` | Template for human developers |
| `.env.ai-ready` | Zero-config for AI agents |
| `.env` | Active config (gitignored) |

---

## Development Commands

```
┌─────────────────────────────────────────────────────────────┐
│  Command              │  Description                        │
├───────────────────────┼─────────────────────────────────────┤
│  npm install          │  Install dependencies               │
│  npm run compile      │  Compile TypeScript → out/          │
│  npm run watch        │  Watch mode (auto-recompile)        │
│  npm run lint         │  Run ESLint checks                  │
│  npm run package      │  Create .vsix package               │
└─────────────────────────────────────────────────────────────┘
```

---

## Project Structure

```
ai-grammar-corrector/
├── src/                      # TypeScript source
│   ├── extension.ts          # Main entry point
│   ├── llmService.ts         # LLM API communication
│   ├── diagnosticsProvider.ts # VS Code diagnostics
│   ├── prompts.ts            # LLM prompts & i18n
│   └── types.ts              # TypeScript interfaces
├── out/                      # Compiled JavaScript (gitignored)
├── package.json              # Extension manifest
├── tsconfig.json             # TypeScript config
├── .eslintrc.json            # ESLint config
└── .vscode/                  # Editor settings
```

---

## Testing

### Build Verification

```bash
# Quick verification that everything compiles
npm run lint && npm run compile
```

### Extension Testing

VS Code extensions require the Extension Host for testing:

1. **In VS Code**: Press `F5` to launch Extension Development Host
2. **In Codex/AI**: Run `npm run package` to create `.vsix`, then install manually

### Test Scenarios

| Scenario | Steps |
|----------|-------|
| Grammar Check | 1. Open `.md` file → 2. Select text → 3. `Cmd+Shift+G` |
| Quick Fix | 1. Click lightbulb → 2. Select suggestion |
| Clear | Command Palette → "AI Grammar: Clear All Diagnostics" |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Build fails | Run `npm install` first |
| Lint errors | Run `npm run lint` to see details |
| TypeScript errors | Check `tsconfig.json` compatibility |
| Extension not loading | Verify `out/` directory exists |

---

## Links

- [README.md](./README.md) - General project info
- [AGENTS.md](./AGENTS.md) - AI agent guidelines
- [VS Code Extension API](https://code.visualstudio.com/api)
