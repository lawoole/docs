# 升级与迁移

- [从 Laravel 迁移项目](#migrate-from-laravel)

<a name="migrate-from-laravel"></a>
## 从 Laravel 迁移项目

Lawoole 相比较于其他使用 Swoole 提速 Laravel 的项目，最大的优势在于 Lawoole 是基于 Laravel 的完整框架而非一个组件。得益于此，Lawoole 能够很轻松的兼容所有 Laravel 及周边的软件或功能包，而不能直接兼容的包，在进行一定针对性的适配后，也能够正常使用。所以，你可以很轻松的将项目从 Laravel 迁移至 Lawoole 中。如果你之前编写了全面的单元测试，那么检查迁移后的程序运行情况将非常方便。