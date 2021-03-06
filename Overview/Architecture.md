# Micro文档

## 架构
Micro为微服务提供了基本的构建模块。目标是简化分布式系统开发。因为微服务是一种架构模式，所以Micro通过工具在逻辑上拆分。

查看体系结构上的[博客文章](https://micro.mu/blog/2016/04/18/micro-architecture.html)，获取详细的概述。

这部分应该很多详解，解释Micro是如何构建的，以及各种lib/仓库之间是如何相互关联的。

## 工具包

### API
启用API作为一个网关或代理，来作为微服务访问的单一入口。它应该在您的基础架构的前端运行。它将HTTP请求转换为RPC并转发给相应的服务。

![](api.png)

### Web
UI是go-micro的web版本，允许通过UI交互访问环境。在未来，它也将是一种聚合Micro Web服务的方式。它包含一种Web应用程序的代理方式。将`/[name]`通过注册表路由到相应的服务。Web UI将前缀“go.micro.web。”（可以配置）添加到名称中，在注册表中查找它，然后将进行反向代理。

![](web.png)

### Sidecar
该Sidecar是go-micro的HTTP接口版本。这是将非Go应用程序集成到Micro环境中的一种方式。

![](car.png)

### Bot
Bot是一个Hubot风格的工具，位于您的微服务平台中，可以通过Slack，HipChat，XMPP等进行交互。它通过消息传递提供CLI的功能。可以添加其他命令来自动执行常用操作任务。

![](bot.png)

### CLI
Micro CLI是go-micro的命令行版本，它提供了一种观察和与运行环境交互的方式。

### Go Micro

Go-micro是微服务的独立RPC框架。它是该工具包的核心，并受到上述所有组件的影响。在这里，我们将看看go-micro的每个特征。

![](go-micro.png)

#### Registry
注册表提供可插入的服务发现库，来查找正在运行的服务。当前的实现是consul，etcd，内存和kubernetes。如果您的喜欢不一样，该接口很容易实现。

#### Selector
选择器通过选择提供负载均衡机制。当客户端向服务器发出请求时，它将首先查询服务的注册表。这通常会返回一个表示服务的正在运行的节点列表。选择器将选择这些节点中的一个用于查询。多次调用选择器将允许使用平衡算法。目前的方法是循环法，随机哈希和黑名单。

#### Broker
Broker是发布和订阅的可插入接口。微服务是一个事件驱动的架构，发布和订阅事件应该是一流的公民。目前的实现包括nats，rabbitmq和http（用于开发）。

#### Transport
传输是通过点对点传输消息的可插拔接口。目前的实现是http，rabbitmq和nats。通过提供这种抽象，运输可以无缝地换出。

#### Client
客户端提供了一种制作RPC查询的方法。它结合了注册表，选择器，代理和传输。它还提供重试，超时，使用上下文等。

#### Server
服务器是构建正在运行的微服务的接口。它提供了一种提供RPC请求的方法。

### Plugins
提供go-micro的[micro/go-plugins](https://github.com/micro/go-plugins)插件。
