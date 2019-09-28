# gate
gate 服务。它的特征是监听一个 TCP 端口，接受连入的 TCP 连接，并把连接上获得的数据转发到 skynet 内部。Gate 可以用来消除外部数据包和 skynet 内部消息包的不一致性。外部 TCP 流的分包问题，是 Gate 实现上的约定。

## gateserver
gateserver.lua是skynet提供的通用模板 来启动一个网关服务器，通过 TCP 连接和客户端交换数据。

TCP 基于数据流，但一般我们需要以带长度信息的数据包的结构来做数据交换。gateserver 做的就是这个工作，把数据流切割成包的形式转发到可以处理它的地址。
# watchdog
由 gate 加上包头，同时处理控制信息和数据信息的所有数据
# agent

在启动脚本main.lua（一般情况下）启动了watchdog服务（而watchdog的启动函数内又启动了gate服务），并向watchdog发送了start命令，而watchdog对start命令的处理就是向gate发送open命令，其实是让gate服务开启监听端口，此时gate记录了watchdog的的地址。

当一个客户端连接过来的时候，首先是经过gate处理（handler.connect），先建立fd -> connection的映射，然后转发给watchdog（SOCKET.open）处理，在其中新建一个agent服务，然后建立fd->agent服务地址的映射，然后向agent服务发送start命令，agent在start命令的处理中又向gate发送forward命令，gate在forward的处理中是通过建立agent -> connection的映射来建立转发，此时的 connection包含agent服务地址，然后调用gateserver.openclient放行消息。以后在客户端发来消息时，如果已经建立的转发，则直接转发到对应的agent服务上，否则，则发送给watchdog处理。


1、服务：

     只需要把符合规范的 .lua 文件放在 skynet 可以找到的路径下就可以由其它服务启动。在 skynet 的配置文件里配置了服务查询路径，以及需要启动的第一个服务，而其它服务都是由该服务直接或间接启动的。每个服务拥有一个唯一的 32bit id ，skynet 把这个 id 称为服务地址，由 skynet 框架分配。即使服务退出，该地址也会尽可能长时间的保留，以避免当消息发向一个正准备退出的服务后，新启动的服务顶替该地址，而导致消息发向了错误的实体(摘自云风Github)

       每个服务分为三个运行阶段：

i).首先是服务加载阶段：这个阶段不可以调用任何有可能阻塞住该服务的 skynet api 。因为此时和服务配套的 skynet
       设置并没有初始化完毕。

ii).然后是服务初始化阶段：由 skynet.start 这个 api 注册的初始化函数执行。这个初始化函数理论上可以调用任何
   skynet api了，但启动该服务的 skynet.newservice 这个 api 会一直等待到初始化函数结束
   才会返回。

iii).最后是服务工作阶段：当你在初始化阶段注册了消息处理函数的话，只要有消息输入，就会触发注册的消息处理函
 数。这些消息都是 skynet 内部消息，外部的网络数据，定时器也会通过内部消息的形式表
 达出来。

Tips:

1. 和 erlang 不同，一个 skynet 服务在某个业务流程被挂起后，即使回应消息尚未收到，它还是可以处理其他的消息       的。

2. 如果一条用户线程永远不调用阻塞 API 让出控制权，那么它将永远占据系统工作线程。不过，skynet 框架也做了一     些监控工作，会在某个服务内的某个工作线程占据了太长时间后，以 log 的形式报告。



2、消息：

每条skynet消息由6部分构成：消息类型、session 、发起服务地址 、接收服务地址 、消息 C 指针、消息长度。skynet预定义了一组消息类型，需要开发者关心的有：回应消息、网络消息、调试消息、文本消息、Lua 消息、错误。另外，skynet 还约定，如果一个请求不需要回应（单向推送），就置 session 为 0 。



https://github.com/Chinaren-Wei/Blog/blob/master/skynet_client_%E6%B6%88%E6%81%AF%E5%A4%84%E7%90%86%E5%88%86%E6%9E%90.md

