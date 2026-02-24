# Go 规范

## OVERVIEW

Go 编码规范，包含注释、命名、导入、错误处理、Context、接口、测试等。

## FILES

| File | Purpose |
|------|---------|
| specification.md | 完整规范 (325 行) |

## KEY RULES

### 注释与命名
- 方法必须有注释和参数说明（中文）
- 包名小写简短，变量/函数驼峰，接口以 er 结尾
- 文件名小写下划线

### 导入
- 标准库 → 第三方 → 项目内部
- 各组之间用空行分隔
- 导入必须使用

### 错误处理
- `fmt.Errorf("context: %w", err)` 保留堆栈
- 自定义错误类型，errors.Is/As
- 禁止忽略错误

### Context
- 第一个参数传递
- 使用 WithTimeout
- 传播取消信号

### 接口
- 小而专注
- 依赖接口而非实现
- 接口放使用方

### 测试
- Table-Driven Tests
- Mock 接口

### 禁止
- 禁止 panic() 业务错误
- 禁止忽略 defer 关闭错误
- 禁止裸 return

## NOTES

- internal 包只能被根目录下直接子包导入
- 使用 sync.Pool 复用对象
- 字符串拼接使用 strings.Builder
