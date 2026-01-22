# AGENTS.md - AI Grammar Corrector

> AI agents should read this file to understand project conventions, coding patterns, and development guidelines.

## Instruction
- After every task and changes, install the compiled extension to `antigravity` (not `code`). The version should be increased by 1 patch.

## Project Overview

**AI Grammar Corrector** is a VS Code extension that provides AI-powered grammar and spelling correction for Markdown and LaTeX files. It communicates with LLM APIs (OpenAI-compatible) to analyze text and provide inline suggestions.

| Attribute | Value |
|-----------|-------|
| Type | VS Code Extension |
| Language | TypeScript |
| Package Manager | npm |
| Build Tool | tsc (TypeScript Compiler) |
| Linter | ESLint |

## Tested VS Code
- `code` (VS Code)
- `antigravity` (Google Antigraity)
- `cursor` (Cursor)

## Quick Commands

```bash
npm install          # Install dependencies
npm run compile      # Compile TypeScript
npm run watch        # Watch mode for development
npm run lint         # Run ESLint
npm run package      # Create .vsix package
```

## Project Structure

```
src/
├── extension.ts         # Main entry - activation, commands, event handlers
├── llmService.ts        # LLM API communication, response parsing
├── diagnosticsProvider.ts # VS Code diagnostics, code actions, hover
├── prompts.ts           # System/user prompts, i18n messages
└── types.ts             # TypeScript interfaces
```

### Key Responsibilities

| File | Purpose |
|------|---------|
| `extension.ts` | Extension lifecycle, command registration, document change tracking |
| `llmService.ts` | API calls, JSON parsing, fuzzy text matching for corrections |
| `diagnosticsProvider.ts` | Diagnostic creation, CodeActionProvider, HoverProvider, decorations |
| `prompts.ts` | Multi-language prompts (10 languages), UI message translations |
| `types.ts` | Shared TypeScript interfaces (CorrectionItem, LLMConfig, etc.) |

## Coding Conventions

### TypeScript

- **Strict mode enabled** (`"strict": true` in tsconfig.json)
- Use `const` over `let` unless reassignment is necessary
- Prefix unused parameters with underscore (`_token`)
- Explicit return types for public functions

### ESLint Rules

- Semi-colons required (warn level)
- Curly braces required for control statements
- `eqeqeq` enforced (use `===` over `==`)
- Unused variables with `_` prefix are allowed

### Naming

- **Files**: camelCase (e.g., `llmService.ts`)
- **Classes**: PascalCase (e.g., `LLMService`, `GrammarCodeActionProvider`)
- **Functions/Variables**: camelCase
- **Constants**: UPPER_SNAKE_CASE for module-level constants

### Error Handling

- Always return structured error responses from async functions
- Use `CorrectionResponse.success` flag pattern for API calls
- Provide user-friendly error messages via `getUIMessage()`

## VS Code Extension Patterns

### Configuration Access

```typescript
const config = vscode.workspace.getConfiguration('aiGrammarCorrector');
const apiKey = config.get<string>('apiKey', '');
```

### Diagnostic Creation

```typescript
const diagnostic = new vscode.Diagnostic(range, message, severity);
diagnostic.source = 'AI Grammar Corrector';
diagnosticCollection.set(document.uri, diagnostics);
```

### Code Actions

- Implement `vscode.CodeActionProvider`
- Return `QuickFix` actions for corrections
- Mark first suggestion as `isPreferred = true`

## i18n Support

The extension supports 10 languages for UI messages:
- English, Chinese, Spanish, French, German
- Japanese, Korean, Portuguese, Russian, Italian

Add translations in `prompts.ts`:
```typescript
export const UI_MESSAGES: Record<string, Record<string, string>> = {
    'English': { checking: 'Checking grammar...', ... },
    'Korean': { checking: '문법 검사 중...', ... },
    ...
};
```

## LLM Integration

### Supported Providers

The extension works with any OpenAI-compatible API:
- OpenAI (default)
- OpenRouter
- Azure OpenAI
- Local models (Ollama, LM Studio)

### Response Format

LLM must return JSON:
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

## Before Committing

1. **Run lint**: `npm run lint` - must pass with no errors
2. **Run compile**: `npm run compile` - must succeed
3. **Check git status**: Review all changes, remove unnecessary code
4. **Commit message**: Use Korean, Conventional Commits format

### Commit Message Format

```
type: 간결한 설명 (Korean)

- 상세 내용 (bullet points)

Co-authored-by: opencode (oMo) <no-reply@opencode.ai>
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`

## VSCode Development Workflow

### One-Click Debug (F5)
Press `F5` to launch the Extension Development Host with the extension pre-compiled. The default configuration:
1. Compiles TypeScript via `npm: watch` task
2. Launches a new VS Code window with the extension loaded
3. Attaches debugger for breakpoints in `src/*.ts`

### Launch Configurations

| Name | Purpose | Shortcut |
|------|---------|----------|
| 🚀 Run Extension | Launch extension in debug mode | F5 |
| Extension Tests | Run test suite (when implemented) | - |

### Tasks (Ctrl+Shift+B)

| Task | Description |
|------|-------------|
| 🔧 Install Dependencies | `npm install` |
| 🧹 Clean & Build | Remove `out/` and recompile |
| 📦 Package VSIX | Create distributable `.vsix` file |
| ✅ Lint & Compile | Run ESLint then compile (pre-commit check) |

### Recommended Workflow
1. **Start Watch Mode**: Run default build task (`Ctrl+Shift+B`) - auto-compiles on save
2. **Debug**: Press `F5` to test changes in Extension Development Host
3. **Before Commit**: Run "✅ Lint & Compile" task to validate

## Testing

### Manual Testing

1. Press F5 in VS Code to launch Extension Development Host
2. Open a `.md` or `.tex` file
3. Select text and run "AI Grammar: Check Selected Text"

### Automated Tests

Test infrastructure exists in `launch.json` but test files are not implemented yet. Future tests should:
- Mock axios for API calls
- Test `validateAndEnrichCorrections()` logic
- Test fuzzy matching in `fuzzyFindPosition()`

## DO NOT

- ❌ Commit `.env` files with real API keys
- ❌ Use `any` type without justification
- ❌ Leave `console.log` debugging statements
- ❌ Modify user settings without explicit consent
- ❌ Make blocking API calls without timeout

## MUST DO

- ✅ Update AGENTS.md when adding new patterns or conventions
- ✅ Add i18n support for new UI messages
- ✅ Handle API errors gracefully with user-friendly messages
- ✅ Test with multiple LLM providers before release
- ✅ Run `npm run lint && npm run compile` before every commit

## Resources

- [VS Code Extension API](https://code.visualstudio.com/api)
- [VS Code Extension Samples](https://github.com/microsoft/vscode-extension-samples)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
