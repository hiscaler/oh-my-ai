# PHP 代码规范

## 强类型约束

- 文件应该总是包含 `declare(strict_types=1)` 声明
- 方法参数总是有类型约束，返回值总是有类型
- 业务逻辑错误必须抛出异常，禁止直接返回 `false`

## 异常处理

- 业务逻辑错误必须抛出异常
- 禁止使用 `false`、`null`、`-1` 表示错误
- 异常类应继承自项目基础异常类或 `\Exception`

## 注释格式

参考如下格式：

```php
/**
 * @var 属性类型 属性描述（补充说明） 
 */
```

属性值为枚举值时，读取对应的 getter 方法所对应的返回值作为补充说明，补充说明使用如下形式撰写，示例如下：

```php
/**
 * @var 属性类型 属性描述（1: 激活、2: 禁止）
 */
```

## 命名规范

| 类型    | 规范                             | 示例                                  |
|-------|--------------------------------|-------------------------------------|
| 类名    | PascalCase                     | `UserService`, `OrderController`    |
| 接口    | I前缀 或 -able 后缀                 | `IUserRepository`, `Serializable`   |
| Trait | 使用                             | `Loggable`, `Timestampable`         |
| 方法    | camelCase                      | `getUserById`, `createOrder`        |
| 常量    | UPPER_SNAKE_CASE               | `MAX_RETRY_COUNT`, `DEFAULT_STATUS` |
| 属性    | camelCase，私有属性 `$_camelCase`   | `$userName`, `$_config`             |
| 枚举    | PascalCase，成员 UPPER_SNAKE_CASE | `OrderStatus::PENDING`              |

## PSR-12 代码风格

- 缩进：4 spaces（使用 Tab 而非空格缩进的团队需统一）
- 命名空间后空一行
- `use` 语句按字母排序分组（导入外部先包，再导入内部类）
- 大括号：类和函数使用开括号换行
- 方法链式调用每行一个方法
- 控制结构空格：`if ($condition)`，而非 `if($condition)`

```php
namespace App\Service;

use App\Entity\User;
use App\Repository\UserRepository;
use Symfony\Component\HttpFoundation\Response;

class UserService
{
    public function __construct(
        private readonly UserRepository $userRepository,
    ) {}

    public function getUserById(int $id): ?User
    {
        return $this->userRepository->find($id);
    }

    public function createUser(User $user): User
    {
        // ...
        return $user;
    }
}
```

## PHP 8 特性

### Attributes

优先使用 PHP 8 Attributes 替代 PHPDoc 注释：

```php
#[ORM\Entity]
#[ORM\Table(name: 'users')]
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    private ?int $id = null;
}
```

### Nullsafe Operator

```php
// 推荐
$country = $user?->getAddress()?->getCountry();

// 不推荐
$country = null;
if ($user !== null && $user->getAddress() !== null) {
    $country = $user->getAddress()->getCountry();
}
```

### Match 表达式

```php
// 推荐
$statusText = match ($status) {
    self::PENDING => '待处理',
    self::ACTIVE => '激活',
    self::DISABLED => '禁用',
};

// 不推荐
switch ($status) {
    case self::PENDING:
        $statusText = '待处理';
        break;
    // ...
}
```

### Named Arguments

```php
// 推荐（参数明确时）
$response = new Response(
    status: 200,
    headers: ['Content-Type' => 'application/json'],
);
```

## 枚举 (PHP 8.1+)

使用 PHP 原生 Enum 替代类常量：

```php
enum OrderStatus: int
{
    case PENDING = 1;
    case PROCESSING = 2;
    case COMPLETED = 3;
    case CANCELLED = 4;

    public function label(): string
    {
        return match ($this) {
            self::PENDING => '待处理',
            self::PROCESSING => '处理中',
            self::COMPLETED => '已完成',
            self::CANCELLED => '已取消',
        };
    }
}
```

### 枚举转换

```php
// 从值获取枚举
$status = OrderStatus::from(1);      // 抛出异常
$status = OrderStatus::tryFrom(1);  // 返回 null

// 枚举转值
$value = $status->value;

// 枚举转字符串
$name = $status->name;
```

## 返回类型

| 类型       | 使用场景             |
|----------|------------------|
| `void`   | 方法无返回值           |
| `never`  | 方法终止（throw/exit） |
| `static` | 返回实例本身           |
| `parent` | 返回父类实例           |

```php
public function process(): void
{
    // 无返回值
}

public function throwError(string $message): never
{
    throw new \RuntimeException($message);
}

public static function create(): static
{
    return new static();
}
```

## 类型声明最佳实践

- 使用 `mixed` 时需明确处理所有类型
- 优先使用 Union Types 而非 nullable
- 使用 `readonly` 修饰不可变属性（PHP 8.1+）
- 避免使用 `var` 声明

```php
// 推荐
private readonly string $name;
private int|null $age = null;

// 不推荐
var $name;
private $age;
```

## 私有方法设计

- 单一职责：每个方法只做一件事
- 方法长度：建议不超过 30 行
- 参数数量：建议不超过 4 个，超过则考虑拆分

## 接口与实现

- 接口方法参数使用基础类型或简单 DTO
- 避免在接口中暴露实体类
- 优先使用契约式编程

```php
interface UserRepositoryInterface
{
    public function find(int $id): ?User;
    public function save(User $user): void;
    public function delete(User $user): void;
}
```
