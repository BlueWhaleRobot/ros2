# ROS2 接口中的新功能

**尚未完成**

ROS2的接口定义语言和ROS1的非常接近。
基本上所有的ROS1的`.msg`和`.srv`文件都可以在ROS2里面重用。
ROS2在1的基础上有新增加了一些功能即

* **bounded arrays**： ROS1只支持没有限制的数组（比如`int32[] foo`）和固定大小的数据（比如`int32[5] bar`）, ROS2 支持有限制的数据(`int32[<5] bat`)
在有些应用场景下是可以给数组的大小定一个上界的，这样可以节省大量的空间。
* **bounded strings**: 同样ROS1也只支持没有限制的字符串。ROS2可以支持有限制的字符串。（string<=5 bar）
* **default values**: ROS1中支持常量(`int32 X=123`)但没有默认值, ROS 2 支持默认值(`int32 X 123`)
当创建一个消息或服务时，如果没有额外设置，则会采用默认值。如果设值则默认值会被覆盖。

注意： 在 beta 1版本中默认值只支持数字类型，数字数组类型，和字符串类型（没有经过转义和编码）。
