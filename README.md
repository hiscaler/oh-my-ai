# Oh My AI

## OpenCode

### 使用说明

1. [安装 OpenCode CLI](https://opencode.ai/docs/zh-cn/#%E5%AE%89%E8%A3%85)
2. 命令行输入 `opencode` 进入命令行模式
3. 使用 `/init` 初始化项目
4. 项目根目录下创建 `opencode.json` 文件，根据使用的开发语言将 Rules 内容复制到 `opencode.json` 文件中
5. 【**可选**】如果需要使用 commands 则创建 `.opencode` 目录，将 commands 文件夹复制进去

```text
OpenCode 使用请参考 https://opencode.ai/docs/zh-cn
```

### Rules

#### PHP

```json
{
    "$schema": "https://opencode.ai/config.json",
    "agent": {
        "build": {
            "pre_prompt": "始终参考 instructions 中的相关规范",
            "options": {
                "pre_prompt": "始终参考 instructions 中的相关规范"
            },
            "permission": {}
        }
    },
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
    "agent": {
        "build": {
            "pre_prompt": "始终参考 instructions 中的相关规范",
            "options": {
                "pre_prompt": "始终参考 instructions 中的相关规范"
            },
            "permission": {}
        }
    },
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
    "agent": {
        "build": {
            "pre_prompt": "始终参考 instructions 中的相关规范",
            "options": {
                "pre_prompt": "始终参考 instructions 中的相关规范"
            },
            "permission": {}
        }
    },
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