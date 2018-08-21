# 配置

- [介绍](#introduction)
- [环境配置](#environment-configuration)
    - [环境配置类型](#environment-variable-types)
    - [配置覆盖规则](#override-rules)

<a name="introduction"></a>
## 介绍

与 Laravel 相同，在 Lawoole 中，配置文件被存放在 `config` 这个文件夹中，而且格式也是相同的。也就是说，你可以毫不费劲的从 Laravel 项目中迁移你的配置到 Lawoole 项目里，不需要做任何改动。

<a name="environment-configuration"></a>
## 环境配置

因为与环境有关的配置可以放置在项目之外的独立文件中，所以基于 Laravel 的程序能够很容易的运行在不同的环境中。

在 Laravel 中，通过 Vance Lucas 的 [DotEnv](https://github.com/vlucas/phpdotenv) 工具库，我们可以在 `.env` 文件里配置这些环境配置，并通过 `env()` 函数引入它们。这是一个很好的方案，但却也蕴藏着很多限。在 Lawoole 中，`.env` 中的配置不会再被读取，取而代之的独立的环境配置文件。

所有环境配置，都存放在 `config` 文件夹下的 `environment` 的文件夹中。环境配置文件的命名，与 `config` 中配置文件的命名是一致且一一对应的。

举个例子，如果你想要修改 `cache.php` 配置中的 `default` 这一配置以达到使用不同的默认缓存驱动的目的，那么你可以在 `config/environment` 创建一个同名的 `cache.php` 环境配置文件。

```php
return [

    'default' => 'redis',
    
    // ...

];
```

<a name="environment-variable-types"></a>
### 环境配置类型

得益于 Lawoole 中的环境配置采用了 PHP 语法，所以其支持的变量类型比 `.env` 结构的定义方式丰富很多，我们可以通过下表进行比较。

数据类型 | Laravel | Lawoole
:---- | :---- | :----
string | 支持 | 支持
integer | | 支持
float | | 支持
boolean | 支持 | 支持
null | 支持 | 支持
array | | 支持
object | | 支持

<a name="override-rules"></a>
### 配置覆盖规则

与 Laravel 相比，在 Lawoole 中使用环境配置替换默认配置，你将得到更多的灵活性。

在 Lawoole 的环境配置文件中，支持 `.` 深层替换方法，也就是说，你可以选择覆盖原有配置中任何一个层级的内容。下面这个示例可以帮助你更好的理解这种配置覆盖方法。

```php
return [

    // 仅更改 MySQL 的 host 配置
    
    'connections.mysql.host' => '...',
    
    // 覆盖全部 SQLite 的配置
    
    'connections.sqlite' => [
        'driver'   => 'sqlite',
        'database' => storage_path('database/lawoole.sqlite'),
    ],
    
    // 更新全部链接中的 password 配置
    
    'connections' => function ($connections) {
        return array_map(function ($connection) {
            $connection['password'] = '...';
        
            return $connection;
        }, $connections);
    }
    
];
```

> {tip} 虽然 `.env` 文件不会被读取，但并不意味着 `env()` 函数是无效的，你仍然可以通过它获得来自控制台或者系统的环境配置。

