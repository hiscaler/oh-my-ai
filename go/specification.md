# Golang 代码规范

## 注释规范

每个方法都应该有注释以及参数说明，没有明确说明的情况下使用中文。

- 导出函数/类型/常量必须有注释
- 注释以被注释对象的名称开头
- 保持注释简洁明了

```go
// UserService 处理用户相关业务逻辑
type UserService struct {
    // userRepo 用户仓储接口
    userRepo UserRepository
}

// GetUserByID 根据 ID 获取用户信息
// 参数:
//   - id: 用户 ID
func (s *UserService) GetUserByID(ctx context.Context, id int64) (*User, error) {
    // ...
}
```

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 包名 | 小写简短 | `user`, `service`, `handler` |
| 变量/函数 | 驼峰命名 | `userName`, `GetUserByID` |
| 常量 | 驼峰或全大写下划线 | `MaxRetryCount` 或 `MAX_RETRY_COUNT` |
| 接口 | 以 er 结尾 | `Reader`, `Writer`, `UserRepository` |
| 文件名 | 小写下划线 | `user_service.go`, `user_repo.go` |

## 导入规范

- 标准库排在前面
- 第三方包排在中间
- 项目内部包排在最后
- 各组之间用空行分隔
- 导入必须使用

```go
import (
    "context"
    "errors"
    "fmt"
    "time"

    "github.com/pkg/errors"
    "github.com/spf13/viper"

    "myproject/internal/model"
    "myproject/internal/repository"
)
```

## 错误处理

### 基本原则

- 错误处理必须包含上下文信息
- 使用 `fmt.Errorf("context: %w", err)` 保留堆栈信息
- 不要忽略错误（使用 `_` 需注释说明原因）

```go
// 推荐
if err != nil {
    return fmt.Errorf("failed to get user %d: %w", id, err)
}

// 不推荐
if err != nil {
    return err
}
```

### 自定义错误类型

```go
// 预定义错误变量
var (
    ErrNotFound     = errors.New("resource not found")
    ErrUnauthorized = errors.New("unauthorized")
)

// 自定义错误类型
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error: %s - %s", e.Field, e.Message)
}

// 使用 errors.Is 和 errors.As
if errors.Is(err, ErrNotFound) {
    // 处理资源未找到
}

var valErr *ValidationError
if errors.As(err, &valErr) {
    // 处理验证错误
}
```

### 错误包装链

```go
func processData(ctx context.Context, id int64) error {
    user, err := getUser(ctx, id)
    if err != nil {
        return fmt.Errorf("processData failed: %w", err)
    }
    // ...
}

func getUser(ctx context.Context, id int64) (*User, error) {
    user, err := fetchUser(ctx, id)
    if err != nil {
        return nil, fmt.Errorf("getUser failed: %w", err)
    }
    return user, nil
}
```

## Context 使用

- Context 作为第一个参数传递
- 超时设置使用 `context.WithTimeout`
- 取消信号需要传播

```go
func fetchData(ctx context.Context, id int64) (*Data, error) {
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    
    // 实际请求
    return client.Fetch(ctx, id)
}
```

## 接口设计

- 接口应该小而专注
- 优先依赖接口而非具体实现
- 接口放在使用方

```go
// 推荐：小接口
type Reader interface {
    Read(ctx context.Context, id int64) (*Entity, error)
}

type Writer interface {
    Write(ctx context.Context, entity *Entity) error
}

// 组合接口
type Repository interface {
    Reader
    Writer
}
```

## 资源管理

### Defer

```go
func processFile(filename string) error {
    file, err := os.Open(filename)
    if err != nil {
        return fmt.Errorf("open file failed: %w", err)
    }
    defer file.Close()
    
    // 处理文件...
    
    // 检查关闭错误
    if err := file.Close(); err != nil {
        return fmt.Errorf("close file failed: %w", err)
    }
    return nil
}
```

### 互斥锁

```go
type Cache struct {
    mu    sync.RWMutex
    items map[string]interface{}
}

func (c *Cache) Get(key string) (interface{}, bool) {
    c.mu.RLock()
    defer c.mu.RUnlock()
    val, ok := c.items[key]
    return val, ok
}

func (c *Cache) Set(key string, value interface{}) {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.items[key] = value
}
```

## 依赖管理

- 尽量避免引入新库
- 如果一定要引入，引入前应该提示用户，说明引入的作用，待用户确认后再引入
- 引入后执行 `go get` 和 `go mod tidy` 命令

```bash
go get github.com/pkg/errors
go mod tidy
```

## 测试规范

### Table-Driven Tests

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive", 1, 2, 3},
        {"negative", -1, -2, -3},
        {"zero", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### 模拟接口

```go
type MockUserRepository struct {
    mock.Mock
}

func (m *MockUserRepository) GetByID(ctx context.Context, id int64) (*User, error) {
    args := m.Called(ctx, id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}

func TestGetUser(t *testing.T) {
    mockRepo := new(MockUserRepository)
    mockRepo.On("GetByID", mock.Anything, int64(1)).Return(&User{ID: 1, Name: "test"}, nil)
    
    service := NewUserService(mockRepo)
    user, err := service.GetUserByID(context.Background(), 1)
    
    assert.NoError(t, err)
    assert.Equal(t, "test", user.Name)
    mockRepo.AssertExpectations(t)
}
```

## 代码组织

```
project/
├── cmd/
│   └── app/
│       └── main.go
├── internal/
│   ├── handler/
│   ├── service/
│   ├── repository/
│   └── model/
├── pkg/
│   └── utils/
├── go.mod
└── go.sum
```

### 内部包规则

- `internal` 包只能被根目录下的直接子包导入
- 避免循环依赖

## 性能建议

- 避免在循环中分配内存
- 使用 `sync.Pool` 复用对象
- 字符串拼接使用 `strings.Builder`
- 切片预分配容量

```go
// 预分配
slice := make([]int, 0, 100)

// strings.Builder
var builder strings.Builder
for i := 0; i < 100; i++ {
    builder.WriteString("item")
}
result := builder.String()
```

## 禁止事项

- 禁止使用 `panic()` 处理业务错误
- 禁止忽略 `defer` 关闭错误
- 禁止使用裸 `return`
- 禁止在生产代码中使用 `fmt.Print*` 调试
