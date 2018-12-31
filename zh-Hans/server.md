# Server

- [介绍](#introduction)
- [配置服务](#configure-server)

<a name="introduction"></a>
## 介绍

Server 是 Lawoole 提供网络服务的模块，其核心就是 Swoole 的 Server。

当我们通过 `php artisan server:start` 启动 Lawoole 项目时，其实启动的就是以 Server 为基础的一个服务。令人愉悦的是，Lawoole 对 Swoole Server 进行了非常友好的封装，可以让大家在极其愉悦的编程体验中使用到它们。

<a name="configure-server"></a>
## 配置服务

对于 Server 相关的配置是通过 `config` 目录下的 `server.php` 这个配置文件来完成的。

在这个文件中，我们可以配置：

### 服务驱动类型

通过 `server.driver` 来指定 Server 的基础驱动，目前支持的驱动有：

- **tcp** 能够支持 TCP 协议连接
- **http** 能够支持 TCP 协议和 Http 协议连接
- **websocket** 能够支持 TCP、Http 和 WebSocket 协议连接

### 服务器选项

通过 `server.options` 能够对 Server 进行配置，这里的配置项继承于 Swoole 对 Server 的服务选项。

常见的配置有：

- **worker_num** 工作进程数
- **task_worker_num** 任务工作进程数

更多完整配置选项，可以参阅 (Swoole Server 配置选项)[https://wiki.swoole.com/wiki/page/274.html]

### 端口监听

通过 `server.listens` 可以指定 Server 对端口的监听和协议请求的处理。这个配置项是一个数组，我们能够同时设置多个端口监听，并处理不同的协议请求。这些请求的处理，是共享工作进程的。

对于每个监听来说，我们需要配置：

- **protocol** 处理协议
- **host** 监听主机
- **port** 监听端口
- **handler** 事件处理器

> {tip} 监听主机 `host` 指的是机器的网卡 IP，如果对此不够熟悉，可以使用 0.0.0.0 来监听所有网卡。

事件处理器是监听中的关键，我们正是通过它来处理网络事件。不过，如果只是普通用户，你无须关心它们的实现，因为 Lawoole 已经包含了 Http 协议、WebSocket 协议以及 Homer 相关的事件处理器，你只需要直接使用它们即可。
