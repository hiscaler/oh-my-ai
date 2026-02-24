# TypeScript 规范

## OVERVIEW

TypeScript 编码规范，包含类型定义、命名、函数、错误处理、模块组织等。

## FILES

| File | Purpose |
|------|---------|
| specification.md | 完整规范 (329 行) |

## KEY RULES

### 类型
- 验证 Typing 定义是否吻合
- 避免 any，使用 unknown 替代
- 修改类型名需同步更新引用

### interface vs type
- interface：对象类型（可扩展、可合并）
- type：联合、交叉、元组、函数

### 命名
- 文件：kebab-case
- 类/接口/类型：PascalCase
- Hooks：use 前缀
- 常量：UPPER_SNAKE_CASE
- 布尔：is/has/can 前缀

### 函数
- 函数类型定义
- 可选参数放必需参数后
- 泛型约束与默认值

### 错误处理
- Try-Catch
- Result 模式

### Null/Undefined
- 可选链 `?.`
- 空值合并 `??`（非 `||`）
- 类型守卫

### 模块组织
- 导入顺序：外部 → 第三方 → 项目内部 → 相对路径
- 禁止 any / as any
- const enum 优先

## NOTES

- 使用 `readonly` 不可变属性
- 类型守卫替代 as 断言
- ESLint: no-explicit-any, prefer-optional-chain
