# AI Grammar Corrector

> AI-powered grammar and spelling correction for Markdown and LaTeX files in VS Code.

![Demo](https://raw.githubusercontent.com/zcyisiee/blog-images/main/科研/vscode_picgo_1765036241205.png)

## Features

- **Grammar & Spelling Check**: Detect errors and get AI-powered correction suggestions
- **Multi-language Support**: Check text in 10 languages with localized UI
- **Inline Suggestions**: Quick fixes appear directly in the editor
- **Customizable Prompts**: Adjust AI behavior with custom system/user prompts
- **Multiple LLM Providers**: Works with OpenAI, OpenRouter, Azure OpenAI, and local models

## Installation

### From VS Code Marketplace
Search for "AI Grammar Corrector" in the Extensions view (`Ctrl+Shift+X`).

### From VSIX
1. Download the `.vsix` file from [Releases](https://github.com/zcyisiee/grammar-corrector-extension/releases)
2. In VS Code: `Ctrl+Shift+P` → `Install from VSIX...`

### Build from Source
```bash
git clone https://github.com/zcyisiee/grammar-corrector-extension.git
cd grammar-corrector-extension
npm install
npm run package
```

## Configuration

Open VS Code Settings and search for `AI Grammar Corrector`:

| Setting | Description | Default |
|---------|-------------|---------|
| `apiUrl` | LLM API endpoint URL | OpenAI |
| `apiKey` | Your API key | - |
| `model` | Model name | `gpt-4o-mini` |
| `targetLanguage` | Language for corrections | `English` |
| `uiLanguage` | UI message language | `English` |
| `enableSuggestions` | Show style suggestions | `true` |

### Example Configuration

```json
{
  "aiGrammarCorrector.apiUrl": "https://api.openai.com/v1/chat/completions",
  "aiGrammarCorrector.apiKey": "sk-***",
  "aiGrammarCorrector.model": "gpt-4o-mini",
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

---

<details>
<summary>中文说明</summary>

### 功能
- 抓语法/拼写，顺手给风格建议
- 中英多语对话，提示词可自定义
- 快捷键或命令面板一键跑完整篇

### 配置
1. 打开 VS Code Settings 搜索 `AI Grammar Corrector`
2. 必填：`apiUrl`（或短名 `url`）+ `apiKey`
3. 模型：`model` 默认 `gpt-4o-mini`

### 用法
- 选中文本后 `Cmd/Ctrl+Shift+G`
- 命令面板搜 `AI Grammar: Check Entire Document`

</details>
