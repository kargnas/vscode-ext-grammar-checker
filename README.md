# AI Grammar Corrector

> AI-powered grammar and spelling correction for Markdown and LaTeX files in VS Code.

![Demo](/assets/demo.png)

## Features

- **Grammar & Spelling Check**: Detect errors and get AI-powered correction suggestions
- **Multi-language Support**: Check text in 10 language with localized UI
- **Inline Suggestions**: Quick fixes appear directly in the editor
- **Customizable Prompts**: Adjust AI behavior with custom system/user prompts
- **Multiple LLM Providers**: Works with OpenAI, OpenRouter, Azure OpenAI, and local models

## Installation

### From VS Code Marketplace
Search for "AI Grammar Corrector" in the Extensions view (`Ctrl+Shift+X`).

### From VSIX
1. Download the `.vsix` file from [Releases](https://github.com/kargnas/vscode-ext-grammar-checker/releases)
2. In VS Code: `Ctrl+Shift+P` → `Install from VSIX...`

### Build from Source
```bash
git clone https://github.com/kargnas/vscode-ext-grammar-checker.git
cd grammar-corrector-extension
npm install
npm run package
```

## Configuration

Open VS Code Settings and search for `AI Grammar Corrector`:

| Setting | Description | Default |
|---------|-------------|---------|
| `apiUrl` | LLM API endpoint URL | OpenRouter |
| `apiKey` | Your API key | - |
| `model` | Model name | `google/gemini-2.5-flash-lite` |
| `targetLanguage` | Language for corrections | `English` |
| `uiLanguage` | UI message language | `English` |
| `enableSuggestions` | Show style suggestions | `true` |

### Example Configuration

```json
{
  "aiGrammarCorrector.apiUrl": "https://openrouter.ai/api/v1/chat/completions",
  "aiGrammarCorrector.apiKey": "sk-***",
  "aiGrammarCorrector.model": "google/gemini-2.5-flash-lite",
  "aiGrammarCorrector.uiLanguage": "English"
}
```

## Usage

| Action | Shortcut |
|--------|----------|
| Check selected text | `Cmd/Ctrl+Shift+G` |
| Check entire document | Command Palette → `AI Grammar: Check Entire Document` |
| Clear diagnostics | Command Palette → `AI Grammar: Clear All Diagnostics` |
| Open settings | Command Palette → `AI Grammar: Open Settings` |

## Development

### Prerequisites
- Node.js 18+
- VS Code 1.85+

### Setup
```bash
npm install
npm run compile
```

### Debug
Press `F5` in VS Code to launch the Extension Development Host.

### Commands
```bash
npm run lint      # Run ESLint
npm run compile   # Compile TypeScript
npm run watch     # Watch mode
npm run package   # Create .vsix
```

### Project Structure
```
src/
├── extension.ts          # Main entry point
├── llmService.ts         # LLM API communication
├── diagnosticsProvider.ts # Diagnostics & code actions
├── prompts.ts            # Prompts & i18n
└── types.ts              # TypeScript interfaces
```

For AI agents, see [AGENTS.md](./AGENTS.md) for detailed guidelines.  
For AI-ready development environment setup, see [README.ai-ready.md](./README.ai-ready.md).

## Contributing

1. Clone the repository
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Make changes and test
4. Run `npm run lint && npm run compile`
5. Commit with [Conventional Commits](https://www.conventionalcommits.org/) format
6. Open a Pull Request

## License

MIT License - see [LICENSE](./LICENSE)