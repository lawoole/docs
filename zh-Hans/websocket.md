# WebSocket

- [使用 WebSocket 服务](#use-websocket-server)
- [处理握手](#hand-shake)
- [连接处理器](#connection-handler)

<a name="use-websocket-server"></a>
## 使用 WebSocket 服务

开启 Http 的支持是使用 WebSocket 模块的前提，所以在使用 WebSocket 之前，我们先要确保已经把 `Lawoole\Http\HttpServiceProvider` 添加到了 `app.providers` 这个配置项中。

使用 `Lawoole\Websocket\WebsocketServerSocketHandler` 作为 `server.listens` 中的监听处理器能够让我们同时处理 Http 和 WebSocket 请求，所以我们能够在 Lawoole 中使用同一个端口处理两种不同的协议。

<a name="hand-shake"></a>
## 处理握手

握手是建立 WebSocket 连接的重要环节，也是通过鉴权等方式确定接受和拒绝连接的最佳时机，在 Lawoole 里，我们还将在这里指定对 WebSocket 连接的处理方式。

建立 WebSocket 连接的请求是一个 Http 请求，我们可以像普通 Http 请求一样来处理这个请求。我们需要在控制器或者其他请求处理方式里，通过 `HandShakeResponse::accept($request, $handler)` 或者 `HandShakeResponse::reject($request, $status)` 来选择接受或者拒绝这个 WebSocket 请求。

<a name="connection-handler"></a>
## 连接处理器

在接受 WebSocket 请求时，我们会通过 `accept($request, $handler)` 方法传递一个连接处理器对象。连接处理器需要实现 `Lawoole\Contracts\WebSocket\WebSocketHandler` 接口。

在连接处理器里，我们可以处理来自此连接的 WebSocket 请求，并能够通过 `$connection` 向客户端发送数据。如果采用相同的连接处理器处理多个请求，你可以通过 `$connection->getId()` 来区分它们。
