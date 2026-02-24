# PROJECT KNOWLEDGE BASE

**Generated:** 2026-02-24
**Commit:** dc2083c
**Branch:** main

## OVERVIEW

Documentation-only repo containing AI coding rules for OpenCode. No executable code—only Markdown specifications for PHP, Go, and TypeScript development.

## STRUCTURE

```
./
├── README.md      # OpenCode config (JSON)
├── guidelines.md  # Core execution protocol
├── git.md         # Git workflow rules
├── go/            # Golang spec
├── php/           # PHP spec + Swagger
└── typescript/    # TypeScript spec
```

## WHERE TO LOOK

| Task | Location | Notes |
|------|----------|-------|
| PHP rules | php/specification.md | Strong typing, exceptions |
| PHP Swagger | php/swagger.md | Schema generation |
| Go rules | go/specification.md | Comments, error handling |
| TS rules | typescript/specification.md | Typing verification |
| Git workflow | git.md | Commit format, commands |
| Core protocol | guidelines.md | Communication language (CN) |

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
