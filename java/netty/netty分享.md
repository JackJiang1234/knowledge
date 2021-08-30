### 缘由

### 基本介绍

netty是基于异步和事件驱动网络编程框架，同时支持客户端与服务端编程。提供优雅一致的编程模型和多种协议网络程序基础设施支持，方便用户快速开发高性能高可靠的网络应用程序。这段介绍比较"官方"，让我们回到原点，看看一个典型的服务器网络程序结构应该是怎么样的:

![image-20210830235627613](D:\person\knowledge\java\netty\network_basic_struct.png)

 标记浅绿色的模块是属于技术领域，我们希望通过框架简单配置或声明就能达到我们的需要，黄色模块属于业务领域，我们希望业务逻辑可以单独开发测试，并能轻松组装，netty对以上都提供了完善的支持，按软件工程角度，分离技术复杂度和业务复杂度关注点，模块松藕后设计

连接 C10K, C10M

#### 典型网络程序结构

![img](https://netty.io/images/components.png)

### 网络IO模型演进

### Netty组件与原理

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F6099975-a745f8dbeb3a597c.png&refer=http%3A%2F%2Fupload-images.jianshu.io&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1632869478&t=41a3d60424dbe1b7bbe5298ae0f7fb33)

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.yasinshaw.com%2F201907%2F11%2F4036B5EE5F8D.jpg&refer=http%3A%2F%2Ffile.yasinshaw.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1632869447&t=8c7a16a87aeaef2f3c5357b22d7b9bf1)

优点与注意示项

### 应用示例

### 总结

### 参考

https://netty.io/

https://github.com/Azure/DotNetty



