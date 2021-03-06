# ROS2的基本概念

ROS是一个用于在不同进程间匿名的发布、订阅、传递信息的中间件。
ROS2系统的核心部分是ROS网络(ROS Graph)。ROS网络是指在ROS系统中不同的节点间相互通信的连接关系。
*ROS Graph这里翻译成了ROS网络，因为我觉得Graph更加抽象，而网络的概念更容易帮助理解其内涵。*

## ROS网络(ROS Graph)概念简介
- 节点(Nodes)： 一个节点是一个利用ROS系统和其他节点通信的实体
- 消息(Messages)： ROS中在订阅和发布主题时所用到的数据结构
- 主题(Topics): 节点可以发布信息到一个主题，同样也可订阅主题来接收消息
- 发现(Discovery): 一个自动运行的进程，通过这个进程不同的节点相互发现，建立连接

## 节点(Nodes)
一个节点就是一个在ROS网络中的参与者。
ROS节点通过ROS客户端程序库(ROS client library)来和其他节点进行通信。
节点可以发布或者订阅主题
节点也可以提供ROS服务(Service)。
节点有很多可以配置的相关参数。
节点间的连接时通过一个分布式发现进程来建立的（即上面所说的发现）。
不同的节点可以在同一个进程里面，也可以在不同的进程里面，甚至可以在不同的机器上。

## 客户端程序库
ROS客户端程序库可以让不同的语言编写的节点进行通信。
在不同的编程语言中都有对应的ROS客户端程序库(RCL)，这个程序库实现了ROS的基本API。
这样就确保了不同的编程语言的客户端更加容易编写，也保证了其行为更加一致。

下面的客户端程序库是由ROS2团队维护的
- rclcpp = C++ 客户端程序库
- rclpy = Python 客户端程序库

另外其他客户端程序也已经有ROS社区开发出来。
可以看[[ROS 客户端程序库]]来了解详细信息

## 发现
节点之间的互相发现是通过ROS2底层的中间件实现的。
过程总结如下
1. 当一个节点启动后， 它会向其他拥有相同ROS域名(ROS domain， 可以通过设置ROS_DOMAIN_ID环境变量来设置)的节点进行广播，说明它已经上线。
其他节点在收到广播后返回自己的相关信息，这样节点间的连接就可以建立了，之后就可以通信了。
2. 节点会定时广播它的信息，这样即使它已经错过了最初的发现过程，它也可以和新上线的节点进行连接。
3. 节点在下线前它也会广播其他节点自己要下线了。

节点只会和具有相兼容的[[服务质量]](Quality of Service)设置的节点进行通信。

## 例子：发布和接收
在一个终端，启动一个节点(用C++编写)，这个节点会向一个主题发布消息
```
ros2 run demo_nodes_cpp talker
```

在另一个终端，同样启动一个节点(用Python编写)，这个节点会订阅和上个节点相同的主题来接收消息。
```
ros2 run demo_nodes_py listener
```

你会看到节点自动发现了对方，然后开始互相通信。
你也可以在不同的电脑上启动节点，你也会发现，节点自动建立了它们的连接，然后开始通信。
