# PROJECT KNOWLEDGE BASE

**Generated:** 2026-02-24
**Commit:** a4896eb
**Branch:** main

## OVERVIEW

Documentation-only repo containing AI coding rules for OpenCode. No executable code—only Markdown specifications for PHP, Go, and TypeScript development.

## STRUCTURE

```
./
├── README.md            # OpenCode config (JSON)
├── rules/guidelines.md  # Core execution protocol
├── rules/git.md         # Git workflow rules
├── rules/go/            # Golang spec
├── rules/php/           # PHP spec + Swagger
└── rules/typescript/    # TypeScript spec
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| PHP rules | rules/php/specification.md | Strong typing, exceptions |
| PHP Swagger | rules/php/swagger.md | Schema generation |
| Go rules | rules/go/specification.md | Comments, error handling |
| TS rules | rules/typescript/specification.md | Typing verification |
| Git workflow | rules/git.md | Commit format, commands |
| Core protocol | rules/guidelines.md | Communication language (CN) |

## ANTI-PATTERNS (THIS PROJECT)

- No code files—documentation only
- No build/CI configurations
- No test files

## CONVENTIONS

- All documentation in Chinese (unless specified)
- File naming: lowercase with dashes
- Each lang folder = self-contained spec

## COMMANDS

N/A—no build/test commands

## NOTES

- This repo serves as OpenCode instruction source via raw URLs
- No LSP, no tests needed
