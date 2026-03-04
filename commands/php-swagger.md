---
description: 撰写 Swagger 文档
agent: build
---

ulw 根据以下规则来撰写 Swagger 文档

## Swagger 整理要求和书写规范

注意：如果返回值是集合，必须在注释中通过 `@OA\Schema(type="array", @OA\Items(ref="#/components/schemas/xxx"))` 明确引用。

### PHP

一般来说，一个小的模块包含两个 Swagger 定义，一个是模块数据，比如 src/Activity/ActivityFormatter 中的 Activity schema 定义，另外一个是 src/Activity/EditActivityRequest 中的 EditActivityRequest schema 定义，为方便沟通，我们后续的要求中直接使用 Model 和 Request 来代替两者。

#### Model 定义

Model 的 Swagger 定义值来自于其类内部的 format 函数输出结果，format 原型定义大体如下
format(Model $model): array其中 Model 根据不同模块名称不同，format 返回值为一个数组, swagger 定义取值规范如下：

- key 作为 swagger 的 property
- type 取  $model->getter 的值类型
- nullable 则根据 $model->getter 所对应的属性值是否可以为 null 来确定
- default 获取 $model->getter 的默认值，如果没有定义默认值，int, bool, float，array 默认情况下取类型的默认值，非string 类型根据属性的注释提取关键词设置默认值，如果是 DateTimeImmutable 类型的话，非 null 的情况下统一为 1767196800, id nullable 的值固定设置为 false, default 值固定设置为 1，返回值是 enum 的，取 enum 中定义的第一个 value
- title 的值必须有
- description 一般只出现在类型 enum 作为说明
- 另外如果 $model->getter 返回的是一个 enum 类型的话，需要进一步读取 enum 的定义，获取 value 设置为 swagger 中的 enum，获取 enum 定义类中的 label 方法来填写 enumNames 值，如果没有定义 label 方法，则获取注释内容作为 swagger 的 enumNames，并且将 value, label 组合后书写到 swagger 的 description 中，组合格式示例“0: 待处理、1: 处理中、2: 已处理”需要注意的是 enum, enumNames, description 必须是同时出现的，且缺一不可

#### Request 定义

Request 的定义和上述 Model 的定义基本一致，只有一个不同点是他的 property 来自于 Request 类的属性定义, type, nullabel, enum, enumNames, default, title, description 取值来自于 Request 类的属性和相关 getter 方法的结果类型，其他规范保持和 Model 规范一致

#### Swagger 书写顺序

Swagger 书写顺序为：property, type，@OA\Items, nullable, enum, enumNames, default, title, description

#### Swagger 检测任务

- 检查 Model 相关的 schema 定义是否符合规范
- 检查 Request 相关的 schema 定义是否符合规范
- 检查 controller 中 schema 引用是否正确，是否有请求体和返回值的 schema 定义
