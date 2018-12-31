# Http

- [使用 Http 服务](#use-http-server)
- [定义路由](#define-router)
- [中间件](#middleware)

<a name="use-http-server"></a>
## 使用 Http 服务

如果需要开启应用对 Http 请求处理的支持，可以在配置项 `app.providers` 中增加 `Lawoole\Http\HttpServiceProvider`，这会让与 Http 相关的路由机制等被载入到 Lawoole 中。

接下来，我们需要在 `server.listens` 的监听中，增加对 Http 协议的监听，在此监听中，请使用 `Lawoole\Http\HttpServerSocketHandler` 作为监听处理器。

<a name="define-router"></a>
## 定义路由

与 Laravel 一致，在 Lawoole 中对 Http 路由的定义是完全一致的，路由文件我们可以存放于 routes 这个目录中。

除了定义路由文件外，我们还需要做的是对路由入口的配置。在 Laravel 中，我们需要通过继承 RouteServiceProvider 来实现这个功能，而在 Lawoole 中，我们只需要通过 `http.php` 这个配置文件就可以实现。

通过 `http.namespace` 这个配置项，我们能够设置全局的控制器命名空间前缀。

在 `http.routes` 里，我们能够引入路由规则文件。这个配置项是一个数组，我们可以简单的将文件通过 `route_path('web.php')` 的形式直接放置在其中，实现对路由定义的引入。

<a name="middleware"></a>
## 中间件

在 Laravel 中，定义中间件需要继承 Http Kernel 来完成，这显然太过繁琐。在 Lawoole 简化了这个过程，我们依然能够通过 `http.php` 这个配置文件来完成对中间件的配置。

在 `http.php` 中，我们可以通过 `middleware`、`route_middleware` 和 `middleware_groups` 来定义中间件，其定义形式与 Laravel 中在 Http Kernel 里定义中间件的方式是一致的。
