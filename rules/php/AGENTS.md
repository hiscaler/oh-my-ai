# PHP 规范

## OVERVIEW

PHP 编码规范，包含强类型、异常处理、命名规范、PSR-12 风格、PHP 8+ 特性、枚举等。

## FILES

| File | Purpose |
|------|---------|
| specification.md | 完整规范 (239 行) |
| swagger.md | Swagger schema 生成规范 |

## KEY RULES

### 强类型与异常
- `declare(strict_types=1)` 必须声明
- 方法参数/返回值必须有类型约束
- 业务错误必须抛出异常，禁止返回 `false`

### 命名规范
- 类名: PascalCase | 接口: I前缀/-able
- 方法: camelCase | 常量: UPPER_SNAKE_CASE
- 属性: camelCase，私有 `$_camelCase`

### PSR-12
- 缩进 4 spaces
- use 语句按字母排序
- `if ($condition)` 空格

### PHP 8+ 特性
- Attributes 替代 PHPDoc
- `?->` Nullsafe
- `match` 替代 `switch`
- 原生 Enum
- `readonly` 属性
- `void`/`never`/`static` 返回类型

## SWAGGER (见 swagger.md)

## NOTES

- 枚举需包含 `label()` 方法
- 私有方法建议 ≤30 行，参数 ≤4 个
- 接口避免暴露实体类
