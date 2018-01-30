## 简介
客户端程序库是用户在写自己的ROS程序时使用到的ROS API程序。
他们就是用户来访问ROS的基本概念比如节点，主题，服务等等时所使用的程序。
客户端程序库有很多不同语言的实现，这样用户就可以根据他们自己的使用场景灵活的选择最合适的语言。
例如你也许想用Python来写图形程序，因为写起来很方便。但是对于对性能要求比较高的地方，这些程序最好还是用C++来写。

使用不同语言编写的程序之间可以自由的共享信息。因为所有的客户端程序库都提供了代码生成程序，这些程序为用户提供了能够处理ROS的接口文件的能力。

客户端程序除了提供不同语言特定的通信工具之外，还为用户提供了一些使得ROS更加ROS化的核心功能。
比如，这些就是可以通过客户端程序库操作的核心功能
- 名称和命名空间
- 时间（实际的或者模拟的）
- 参数
- 终端输出
- 线程模型
- 跨进程通信

## 支持的客户端程序库

C++ 客户端程序库(rclcpp)和Python客户端程序库(rclpy)都提供了RCL的常用功能。

C++和Python的客户端程序库由 ROS 2 团队维护，ROS2的社区成员同时也支持了以下额外的客户端程序库：

- [JVM 和 Android](https://github.com/esteve/ros2_java)
- [Objective C 和 iOS](https://github.com/esteve/ros2_objc)
- [C#](https://github.com/firesurfer/rclcs)
- [Swift](https://github.com/younata/rclSwift)
- [Node.js](https://www.npmjs.com/package/rclnodejs)

## 通用的功能：RCL
大部分客户端程序库提供的功能并不只是在特定的语言里才有。比如参数的效果，命名空间的逻辑，在理想的情况下应该在所有的语言下保持一致。
正是因为这一点，与其为每种语言从零开始实现一遍，倒不如使用一个通用的核心ROS客户端程序库接口。这个接口实现了程序逻辑和ROS中语言无关的功能。这样就使得ROS客户端程序库更小也更容易开发。
由于这个原因，RCL通用功能暴露出了C的程序接口。因为C是其他程序语言最容易进行包装的语言。

使用通用的核心库的优点不止可以使得客户端程序更加小型化，同时也使得不同语言的客户端程序库能够保持高度的一致性。
如果通用核心库内的任何功能发生了变化，比如说命名空间，那么所有的客户端程序库的这个功能都会跟着变化。
再者使用通用的核心程序库也意味着当修复bug的时候维护多个客户端程序会更加方便。

[RCL 的 API文档可以看这里](http://docs.ros2.org/beta1/api/rcl/)

## 和语言相关的功能
对于和语言相关的功能，并没有在RCL中实现，而是在各个语言的客户端程序库中去实现。
例如，在spin中使用到的线程模型，完全由各语言客户端程序自己实现。


## 例子
对于一个使用rclpy的发布者和一个使用rclcpp的订阅者之间的消息传递的例子，我们建议你去看这个视频[this ROSCon talk](https://vimeo.com/187696091)的17:25处。这有一些相关的[幻灯片](http://roscon.ros.org/2016/presentations/ROSCon%202016%20-%20ROS%202%20Update.pdf)

## 与ROS1相比
在ROS1中所有的客户端程序都是从零开始编写的。
这样可以使Python的客户端程序库完全由Python编写。这样就有不用编译源代码的好处。
但是命名规则和其他一些特性并不能和其他客户端程序库保持一致。bug的修复需要在不同的地方重复很多遍。而且有很多功能只有在特定语言的客户端程序库中才实现。

## 总结
通过把ROS通用客户端核心程序库抽出来，不同的语言的客户端程序变得更加容易开发也更能保证功能的一致性。