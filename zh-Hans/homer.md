# Homer

- [介绍](#introduction)
- [基础使用](#basic-usage)
- [定义接口](#define-interface)
- [中间件](#middleware)

<a name="introduction"></a>
## 介绍

Homer 是 Lawoole 中的重量级模块，其提供了非常方便的 RPC 功能。

与其他 PHP RPC 框架不同，通过 Homer 实现远程调用是无感知的，你甚至可以在不修改现有代码的前提下，实现本地调用到远程调用的切换。

<a name="basic-usage"></a>
## 基础使用

使用 Homer 前，需要把 `Lawoole\Homer\HomerServiceProvider` 引入到 `app.providers` 这个配置项中。

如果你是以服务方来运行，以期望 Homer 向外提供服务时，还需要在 `server.listens` 里添加服务监听。Lawoole Homer 支持多种传输协议，只需要在 `server.listens` 指定它们以及关联的事件处理器即可。

- **whisper** Lawoole\Homer\Transport\Whisper\WhisperServerSocketHandler
- **http** Lawoole\Homer\Transport\Http\HttpServerSocketHandler

如果你作为客户端，希望连接到指定的服务器，则可以通过 `homer.php` 中的 `clients` 这个配置来定义服务器的参数。这里是一个简单的例子，我们通过 `url` 指定了远程的连接地址（包括协议、主机、端口），另外还有超时时间和连接丢失重试次数等。

```php
'clients' => [

    'remote' => [
        'url'         => 'whisper://127.0.0.1:8081',
        'timeout'     => 5000,
        'retry_times' => 1,
    ],

],
```

<a name="define-interface"></a>
## 定义接口

在开发前先定义接口是非常好的习惯，而在 Homer 中，服务器与客户端调用就是通过接口来约定的。服务端定义的接口可以拷贝到客户端中，但我们更期望大家能够通过 composer 来管理接口以及它们的更新。

```php
interface EchoServiceInterface
{
    /**
     * Ping the server and get pong.
     *
     * @return \DateTime
     */
    public function ping();
}
```

对于定义好的接口，我们可以通过 `homer.php` 来申明，以便 Homer 识别和使用它们。

在服务端，我们需要通过 `homer.services` 来指定接口和它们的实现。通过这个绑定，当客户端调用这个接口时，Homer 会自动采用对应的实现类来处理调用并返回结果。

```php
'services' => [
    [
        'interface'  => App\Services\EchoServiceInterface::class,
        'refer'      => App\Services\EchoService::class,
    ],
],
```

在客户端，我们则通过 `homer.references` 来引用远程服务。在这个配置里，我们可以指定接口以及远程服务的配置名，具体的远程服务会采用 `homer.clients` 中的定义。

```php
'references' => [
    [
        'interface' => App\Services\EchoServiceInterface::class,
        'client'    => 'remote',
    ],
],
```

在客户端中定义的接口，会在 Lawoole 启动后注册到容器中，所以我们可以忽略与它们相关的所有内部实现，直接通过容器来调用它们。

```php
$pong = app(EchoServiceInterface::class)->ping();
```

上面这段代码看上去与我们通常编写的本地调用代码并无差别，但其实它已经通过 Homer 形成了对远程服务的调用。

<a name="middleware"></a>
## 中间件

在 Homer 里，也存在于 Http 服务相类似的中间件机制，而且我们能够在客户端和服务端分别设置和使用。通过中间件，我们可以完成服务熔断、调用统计等功能。

中间件的定义可以通过 `homer.middleware`、`homer.name_middleware` 和 `homer.middleware_groups` 来完成，它们的含义与 Http 的中间件相类似。
