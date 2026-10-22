# AGENTS.md - AI Grammar Corrector

> AI agents should read this file to understand project conventions, coding patterns, and development guidelines. **Always update this file when you discover new patterns or add significant features.**

## Quick Reference

| Item | Value |
|------|-------|
| Type | VS Code Extension |
| Language | TypeScript (strict mode) |
| Package Manager | npm |
| Node | 24.22.1 (`.nvmrc`, CI) |
| Build | `npm run compile` |
| Lint | `npm run lint` |
| Package | `npm run package` |

## Instruction

- After every task and changes, install the compiled extension to `antigravity` (not `code`). The version should be increased by 1 patch.
- Run `tree -I 'node_modules|out|.git' -L 3` to understand the project structure before making changes.

## Project Overview

**AI Grammar Corrector** is a VS Code extension that provides AI-powered grammar and spelling correction for Markdown and LaTeX files. It communicates with LLM APIs (OpenAI-compatible) to analyze text and provide inline suggestions.

### Tech Stack

| Component | Technology |
|-----------|------------|
| Language | TypeScript 5.3+ |
| Runtime | Node.js 24.22.1 (toolchain). The extension still runs in the editor host. |
| API Client | axios |
| Linter | ESLint with @typescript-eslint |
| Target | VS Code API 1.85+ |

### Tested Editors
- `code` (VS Code)
- `antigravity` (Google Antigravity)
- `cursor` (Cursor)

## Project Structure

```
ai-grammar-corrector/
├── src/                          # TypeScript source files
│   ├── extension.ts              # Main entry - activation, commands, event handlers
│   ├── llmService.ts             # LLM API communication, JSON parsing, fuzzy matching
│   ├── diagnosticsProvider.ts    # VS Code diagnostics, CodeActionProvider, HoverProvider
│   ├── prompts.ts                # System/user prompts, i18n messages (10 languages)
│   └── types.ts                  # TypeScript interfaces
├── out/                          # Compiled JavaScript (gitignored)
├── .vscode/                      # VS Code workspace settings
│   ├── launch.json               # Debug configurations (F5)
│   ├── tasks.json                # Build tasks (Ctrl+Shift+B)
│   └── settings.json             # Editor settings
├── .github/workflows/            # GitHub Actions
│   ├── ci.yml                    # Lint + Build on every push/PR
│   └── release.yml               # Publish to VS Code Marketplace
├── package.json                  # Extension manifest & npm scripts
├── tsconfig.json                 # TypeScript strict mode config
└── .eslintrc.json                # ESLint rules
```

### Key File Responsibilities

| File | Responsibility |
|------|----------------|
| `extension.ts` | Extension lifecycle (`activate`/`deactivate`), command registration, document change tracking, correction state management |
| `llmService.ts` | HTTP requests to LLM APIs, JSON response parsing, fuzzy text matching (`fuzzyFindPosition`), range refinement |
| `diagnosticsProvider.ts` | Creates VS Code diagnostics, implements `CodeActionProvider` (quick fixes), `HoverProvider` (tooltips), text decorations |
| `prompts.ts` | Multi-language system/user prompts, UI message translations, `getUIMessage()` helper |
| `types.ts` | Shared TypeScript interfaces: `CorrectionItem`, `LLMConfig`, `ExtensionConfig`, etc. |

## Development Workflow

### Quick Commands
```bash
npm install          # Install dependencies
npm run compile      # Compile TypeScript to out/
npm run watch        # Watch mode - auto-recompile on save
npm run lint         # Run ESLint checks
npm run package      # Create .vsix package file
```

### Debug (F5)
1. Press `F5` in VS Code to launch Extension Development Host
2. The extension auto-compiles via `npm: watch` task
3. Open any `.md` or `.tex` file to test

### Recommended Workflow
1. Run `npm run watch` in background (auto-compiles on save)
2. Press `F5` to debug in Extension Development Host
3. Before commit: `npm run lint && npm run compile`

## Coding Conventions

### TypeScript Rules
- **Strict mode enabled** (`"strict": true` in tsconfig.json)
- Use `const` over `let` unless reassignment is necessary
- Prefix unused parameters with underscore (`_token`, `_`)
- Explicit return types for public functions
- No `any` type without justification

### ESLint Rules
- Semi-colons required (warn level)
- Curly braces required for control statements
- `eqeqeq` enforced (use `===` over `==`)
- Unused variables with `_` prefix are allowed

### Naming Conventions
| Element | Convention | Example |
|---------|------------|---------|
| Files | camelCase | `llmService.ts` |
| Classes | PascalCase | `LLMService`, `GrammarCodeActionProvider` |
| Functions/Variables | camelCase | `checkGrammar`, `currentCorrections` |
| Constants | UPPER_SNAKE_CASE | `DIAGNOSTIC_SOURCE` |
| Interfaces | PascalCase | `CorrectionItem`, `LLMConfig` |

### Error Handling Pattern
```typescript
// Always return structured error responses
async function checkGrammar(text: string): Promise<CorrectionResponse> {
    if (!config.apiKey) {
        return {
            corrections: [],
            success: false,
            error: 'API key not configured'
        };
    }
    // ... implementation
}
```

### VS Code Extension Patterns

**Configuration Access:**
```typescript
const config = vscode.workspace.getConfiguration('aiGrammarCorrector');
const apiKey = config.get<string>('apiKey', '');
```

**Diagnostic Creation:**
```typescript
const diagnostic = new vscode.Diagnostic(range, message, severity);
diagnostic.source = 'AI Grammar Corrector';
diagnosticCollection.set(document.uri, diagnostics);
```

**Code Actions:**
- Implement `vscode.CodeActionProvider`
- Return `QuickFix` actions for corrections
- Mark first suggestion as `isPreferred = true`

## i18n Support

The extension supports 10 languages for UI messages:
- English, Chinese, Spanish, French, German
- Japanese, Korean, Portuguese, Russian, Italian

**Adding new UI messages:**
```typescript
// In prompts.ts
export const UI_MESSAGES: Record<string, Record<string, string>> = {
    'English': { checking: 'Checking grammar...', newKey: 'New message' },
    'Korean': { checking: '문법 검사 중...', newKey: '새 메시지' },
    // ... all 10 languages
};
```

## LLM Integration

### Supported Providers
The extension works with any OpenAI-compatible API:
- OpenAI (default)
- OpenRouter (with custom headers)
- Azure OpenAI
- Local models (Ollama, LM Studio)

### Expected LLM Response Format
```json
{
  "corrections": [
    {
      "original": "text with error",
      "suggestions": ["corrected text"],
      "type": "error",
      "explanation": "Why this is wrong"
    }
  ],
  "summary": "Brief assessment"
}
```

### OpenRouter Headers
When using OpenRouter, the extension adds custom headers:
```typescript
headers['HTTP-Referer'] = 'https://kargn.as';
headers['X-Title'] = 'Sangrak';
```

## Before Committing

### Checklist
1. `npm run lint` - must pass with no errors
2. `npm run compile` - must succeed
3. `git status` - review all changes, remove unnecessary code
4. Test in Extension Development Host (F5)

### Commit Message Format
```
type: Concise description

- Detailed content (bullet points)
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`

## DO NOT

- Commit `.env` files with real API keys
- Use `any` type without justification
- Leave `console.log` debugging statements (except in `activate()`)
- Modify user settings without explicit consent
- Make blocking API calls without timeout
- Create files unless absolutely necessary

## MUST DO

- Update AGENTS.md when adding new patterns or conventions
- Add i18n support for new UI messages (all 10 languages)
- Handle API errors gracefully with user-friendly messages
- Test with multiple LLM providers before release
- Run `npm run lint && npm run compile` before every commit
- Write human-readable code with clear variable names and comments

## Code Quality

### Before You Commit
1. Run `git status` and review all changes
2. Ask yourself: "Is there any unnecessary code?"
3. Run build to verify: `npm run lint && npm run compile`
4. Test the extension with F5

### Writing Clean Code
- Delete code, functions, or files if you're sure they're not needed (we have git)
- Use descriptive variable names
- Add comments explaining "why" not "what"
- Keep functions focused and small

## Testing

### Manual Testing
1. Press F5 to launch Extension Development Host
2. Open a `.md` or `.tex` file
3. Select text and run "AI Grammar: Check Selected Text" (Cmd/Ctrl+Shift+G)
4. Verify diagnostics appear and quick fixes work

### Automated Tests
Test infrastructure exists in `launch.json` but test files are not yet implemented. Future tests should:
- Mock axios for API calls
- Test `validateAndEnrichCorrections()` logic
- Test fuzzy matching in `fuzzyFindPosition()`

## Resources

- [VS Code Extension API](https://code.visualstudio.com/api)
- [VS Code Extension Samples](https://github.com/microsoft/vscode-extension-samples)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [OpenRouter API Docs](https://openrouter.ai/docs)
