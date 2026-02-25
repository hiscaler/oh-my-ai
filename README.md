# Oh My AI

## OpenCode

### Rules

#### PHP

```json
{
    "$schema": "https://opencode.ai/config.json",
    "instructions": [
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/git.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/guidelines.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/php/AGENTS.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/php/specification.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/php/swagger.md"
    ]
}
```

#### Golang

```json
{
    "$schema": "https://opencode.ai/config.json",
    "instructions": [
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/git.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/guidelines.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/go/AGENTS.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/go/specification.md"
    ]
}
```

#### TypeScript

```json
{
    "$schema": "https://opencode.ai/config.json",
    "instructions": [
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/git.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/guidelines.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/typescript/AGENTS.md",
        "https://raw.githubusercontent.com/hiscaler/oh-my-ai/refs/heads/main/rules/typescript/specification.md"
    ]
}
```

### Skills

- git-release-notes 根据 Git commit 提交历史自动撰写结构化、专业化的发布日志（Release Notes）

### Commands

复制 `commands` 文件夹到项目的 `.opencode` 文件夹中并重启 UI