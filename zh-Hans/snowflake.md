# Snowflake

- [介绍](#introduction)
- [基础使用](#basic-usage)

<a name="introduction"></a>
## 介绍

Snowflake 算法是 Twitter 设计的一个分布式 ID 生成算法，Snowflake 模块基于 Swoole 对 Snowflake 算法进行了优化，实现了分布式 ID 的生成功能。

Lawoole 的 Snowflake 算法在 Twitter 原始的 Snowflake 算法基础上进行了一定的修改，使其更适应于在 Swoole 环境下生成 ID。
在 Lawoole 的 Snowflake 算法中，生成 ID 的位信息含义如下：

 0 - 31 | 32 - 36 | 37 - 41 | 42 - 52 | 53 - 63
:------:|:-------:|:-------:|:-------:|:-------:|
 秒序 | 机器编号 | 随机数 | 进程号 | 自增数

<a name="basic-usage"></a>
## 基础使用

你可以很轻松的使用 `Snowflake` 外观来生成一个新的 Snowflake ID：

```php
$id = Snowflake::nextId();
```

#### 修改机器编码

在默认情况下，Snowflake 会基于本地的 MAC 网络地址生成一个机器编码，如果你希望对其进行修改，可以使用 `setMachineId` 方法：

```php
Snowflake::setMachineId(6);
```
