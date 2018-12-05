# 版本更新记录

- [版本定义](#versioning-scheme)
- [依赖](#dependencies)
- [Lawoole 1.0](#lawoole-1.0)

<a name="versioning-scheme"></a>
## 版本定义

Lawoole 框架定义版本号的方式与 Laravel 近似，采用 `paradigm.major.minor` 这种形式。

<a name="dependencies"></a>
## 依赖

Lawoole 是基于 Laravel 框架以及 Swoole 扩展的，在不同版本的 Lawoole 中，对这两者的依赖也有一定的区别。

 **Lawoole 版本** | **发布时间** | **最低 Laravel 版本** | **最低 Swoole 版本**
:---|:---|:---|:---|
 0.5 | 2018-06-20 | 5.6 | 1.10.5
 1.0 | 2019-01-01 | 5.7 | 1.10.5
 
> {notice} 由于 Laravel 的大部分逻辑设计与 Swoole 协程不相兼容，故 Lawoole 不针对协程模式做任何适配，强烈建议在使用 Lawoole 时关闭 Swoole 协程模式，除非你能很好的处理所有的副作用。

<a name="lawoole-1.0"></a>
## Lawoole 1.0

