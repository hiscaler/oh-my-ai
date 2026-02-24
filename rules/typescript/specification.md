# TypeScript 代码规范

## 类型定义

### 基本原则

- 验证数据类型的 Typing 定义是否吻合
- 尽量避免 `any` 类型
- 如果修改某个类型的命名要同步修改其引用的相关文件

### interface vs type

- 使用 `interface` 定义对象类型（可扩展、可合并）
- 使用 `type` 定义联合类型、交叉类型、元组、函数类型

```typescript
// 推荐：interface 用于对象
interface User {
    id: number;
    name: string;
    email: string;
}

// interface 扩展
interface User extends Person {
    role: 'admin' | 'user' | 'guest';
}

// type 用于联合/交叉类型
type Status = 'pending' | 'success' | 'error';
type Result<T, E = Error> = { ok: true; value: T } | { ok: false; error: E };
type Callback = (data: string) => void;
```

### 泛型

```typescript
// 泛型约束
interface Repository<T extends { id: number }> {
    findById(id: number): Promise<T | null>;
    save(entity: T): Promise<void>;
}

// 泛型默认值
interface Response<T = unknown> {
    data: T;
    status: number;
}
```

### Utility Types

```typescript
// 常用 Utility Types
type PartialUser = Partial<User>;
type RequiredUser = Required<User>;
type ReadonlyUser = Readonly<User>;
type PickUser = Pick<User, 'id' | 'name'>;
type OmitUser = Omit<User, 'password'>;

// 进阶
type Nullable<T> = T | null;
type NonNullable<T> = T extends null | undefined ? never : T;
```

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 文件名 | kebab-case | `user-service.ts`, `use-user.ts` |
| 类名 | PascalCase | `UserService`, `HomePage` |
| 接口 | PascalCase | `User`, `UserRepository` |
| 类型 | PascalCase | `ResponseData`, `ApiError` |
| 函数组件 | PascalCase | `UserProfile`, `Header` |
| Hooks | use 前缀 | `useUser`, `useAuth` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| 枚举 | PascalCase，成员 UPPER_SNAKE_CASE | `OrderStatus::PENDING` |
| 布尔变量 | is/has/can 前缀 | `isLoading`, `hasPermission` |

## 函数

### 函数类型定义

```typescript
// 箭头函数类型
type Handler = (event: Event) => void;

// 方法重载
function parse(input: string): object;
function parse(input: string, options: Options): Result;
function parse(input: string, options?: Options): object | Result {
    // 实现
}
```

### 可选参数与默认参数

```typescript
// 可选参数（必须放在必需参数后面）
function createUser(name: string, age?: number): User {
    return { name, age: age ?? 18 };
}

// 默认参数
function fetchData(url: string, method: 'GET' | 'POST' = 'GET'): Promise<Response> {
    // ...
}
```

## 错误处理

### Try-Catch

```typescript
async function loadUser(id: number): Promise<User> {
    try {
        const response = await api.get(`/users/${id}`);
        return response.data;
    } catch (error) {
        if (error instanceof ApiError) {
            // 处理 API 错误
            throw new UserLoadError(error.message);
        }
        // 处理未知错误
        throw new Error('Failed to load user');
    }
}
```

### Result 模式

```typescript
type Result<T, E = Error> = 
    | { ok: true; value: T }
    | { ok: false; error: E };

async function getUser(id: number): Promise<Result<User>> {
    try {
        const user = await api.get(`/users/${id}`);
        return { ok: true, value: user };
    } catch (error) {
        return { ok: false, error: error as Error };
    }
}
```

## Null 和 Undefined

### 可选链与空值合并

```typescript
// 可选链
const city = user?.address?.city;
const first = items?.[0];

// 空值合并（??）vs 或运算符（||）
const value1 = null ?? 'default';  // 'default'
const value2 = '' ?? 'default';    // ''
const value3 = 0 ?? 'default';     // 0

const value4 = null || 'default';  // 'default'
const value5 = '' || 'default';    // 'default'
const value6 = 0 || 'default';     // 'default'
```

### 类型守卫

```typescript
// typeof
function process(value: string | number) {
    if (typeof value === 'string') {
        return value.toUpperCase();
    }
    return value.toFixed(2);
}

// instanceof
function log(error: Error | string) {
    if (error instanceof Error) {
        console.log(error.stack);
    } else {
        console.log(error);
    }
}

// 自定义类型守卫
function isUser(obj: unknown): obj is User {
    return (
        typeof obj === 'object' &&
        obj !== null &&
        'id' in obj &&
        'name' in obj
    );
}
```

## 模块组织

### 导入顺序

1. 外部库（React、Vue 等）
2. 第三方库
3. 项目内部模块
4. 相对路径模块
5. 类型导入

```typescript
// React
import { useState, useEffect } from 'react';

// 第三方
import { z } from 'zod from 'axios';

//';
import axios 项目内部
import { UserService } from '@/services/user';
import { useAuth } from '@/hooks/use-auth';

// 相对路径
import { Button } from './components/Button';
import type { User } from './types';
```

### 导出

```typescript
// 命名导出
export const UserService = class { };
export function getUser(id: number) { }
export type { User, UserProps };
export { UserRole };

// 默认导出（仅在单例导出时使用）
export default App;
```

## 禁止事项

- 禁止使用 `any`，使用 `unknown` 替代
- 禁止使用 `// @ts-ignore` 或 `// @ts-expect-error`（除非有明确注释说明原因）
- 禁止使用 `as any` 类型断言
- 禁止修改已有类型名称而不同步更新引用
- 禁止在 Hooks 中直接修改 props

## ESLint 规则建议

```json
{
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/prefer-optional-chain": "error",
    "@typescript-eslint/prefer-nullish-coalescing": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

## Enum 使用

### 常量枚举 vs 枚举

```typescript
// 推荐：const enum（编译时内联，无运行时开销）
const enum OrderStatus {
    PENDING = 'pending',
    PROCESSING = 'processing',
    COMPLETED = 'completed'
}

// 不推荐：普通 enum（保留运行时值）
enum OrderStatus {
    PENDING,
    PROCESSING,
    COMPLETED
}

// 替代方案：as const 对象
const OrderStatus = {
    PENDING: 'pending',
    PROCESSING: 'processing',
    COMPLETED: 'completed'
} as const;
```

## Readonly

```typescript
// readonly 属性
interface User {
    readonly id: number;
    readonly name: string;
    role: string; // 可修改
}

// 函数参数 readonly
function processUser(user: readonly User[]): void {
    // 不能修改数组元素
}

// 整个对象 readonly
function createUser(user: Readonly<User>): User {
    // 不能修改 user
}
```

## 类型断言

```typescript
// 不推荐：as
const user = data as User;

// 推荐：类型守卫
function isUser(obj: unknown): obj is User {
    return obj !== null && typeof obj === 'object' && 'id' in obj;
}

if (isUser(data)) {
    console.log(data.id); // TypeScript 已知是 User
}

// 推荐：类型断言工厂
function assertIsUser(val: unknown): asserts val is User {
    if (!isUser(val)) {
        throw new Error('Not a user');
    }
}
```
