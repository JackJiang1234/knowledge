### 缘由

### 基本介绍

netty是基于异步和事件驱动网络编程框架，同时支持客户端与服务端编程。提供优雅一致的编程模型和多种协议网络程序基础设施支持，方便用户快速开发高性能高可靠的网络应用程序。这段介绍比较"官方"，让我们回到原点，看看一个典型的服务器网络程序结构应该是怎么样的:

![image-20210830235627613](D:\person\knowledge\java\netty\network_basic_struct.png)

 标记浅绿色的模块是属于技术领域，我们希望通过框架简单配置或声明就能达到我们的需要，黄色模块属于业务领域，我们希望业务逻辑可以单独开发测试，并能轻松组装，netty对以上都提供了完善的支持，按软件工程角度，分离技术复杂度和业务复杂度关注点，模块松藕后设计

连接 C10K, C10M

#### 典型网络程序结构

![img](https://netty.io/images/components.png)

### 网络IO模型演进

为什么网络IO模型，本质上怎么协调计算机资源进行网络请求处理，这里的"资源"比较关键是线程，我们发现模型的演进实质都是线程与网络请求关系变化。

#### BIO模型(Blocking I/O)

![img](https://static001.geekbang.org/resource/image/e7/e2/e712c37ea0483e9dde0d6efe76e687e2.png)

先来一段代码示例

```java
ServerSocket serverSocket = new ServerSocket(8080);        
while(true) {                                              
    Socket socket = serverSocket.accept();                 
    new Thread(()->{
        BufferedReader input = new BufferedReader(nwe InputStreamReader(socket.getInputStream()));
    	// 数据读取业务处理 input.readLine()
    
    	// 进行业务处理
    
    	// 写入结果
    	PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
    	out.println("result");   
    }).start();                          
}
```

在这其中网络操作连接Accept, Read和Write读写都是阻塞的

BIO模型本质网络请求和线程是一对一的关系，

伸缩性 

可靠性比较差,  资源耗尽可以OM,  网络风险

存在的问题并发数量较大，需要**创建大量线程来处理连接**，系统资源占用较大, 连接建立后，如果当前线程没有数据可读，则线程就阻塞在相应的操作，造成线程资源浪费,  聪明的同学发现可以使用线程池优化，使用线程池确实可以提高资源利用率，但没有改变问题的本质

我们期望的理想效果是一个线程可以处理N个请求，N当然是越多越好，问题的关键在那里？现有的阻塞API本质请求处理模型是网络操作都是one by one, 不能实现我们想要的效果。我们再将网络相关的处理再拆解一下，就是网络连接和读写操作，现实中最高效的通信模型是广播通知机制，下面我们来看一下NIO模型

![img](https://static001.geekbang.org/resource/image/ea/1f/eafed0787b82b0b428e1ec0927029f1f.png)

#### NIO (Non Blocking IO)

```java
public class NioServerTest {
    public static void main(String[] args) throws Exception{
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress(8888), 1024);
        serverSocketChannel.configureBlocking(false);

        Selector selector = Selector.open();
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            selector.select(1000);
            Set<SelectionKey> selectionKeySet = selector.selectedKeys();
            for (SelectionKey selectionKey : selectionKeySet) {
                if(!selectionKey.isValid()) {
                    continue;
                }
                if (selectionKey.isAcceptable()) {
                    accept(selectionKey, selector);
                }
                if (selectionKey.isReadable()) {
                    read(selectionKey);
                }
            }
            selectionKeySet.clear();
        }
    }

    private static void accept(SelectionKey selectionKey, Selector selector) throws Exception{
        ServerSocketChannel ssc = ((ServerSocketChannel) selectionKey.channel());
        SocketChannel socketChannel = ssc.accept();
        socketChannel.configureBlocking(false);
        selectionKey.interestOps(SelectionKey.OP_READ)
    }

    private static String read(SelectionKey selectionKey) throws Exception{
        SocketChannel socketChannel = ((SocketChannel) selectionKey.channel());
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        int len = socketChannel.read(byteBuffer);
        if (len < 0) {
            socketChannel.close();
            selectionKey.cancel();
            return "";
        } else if(len == 0) { 
            return "";
        }
        byteBuffer.flip();
        selectionKey.interestOps(SelectionKey.OP_WRITE)
        doWrite(selectionKey, "Hello Nio");
        return new String(byteBuffer.array(), 0, len);
    }

    private static void doWrite(SelectionKey selectionKey, String responseMessage) throws Exception{
        SocketChannel socketChannel = ((SocketChannel) selectionKey.channel());
        ByteBuffer byteBuffer = ByteBuffer.allocate(responseMessage.getBytes().length);
        byteBuffer.put(responseMessage.getBytes());
        byteBuffer.flip();
        socketChannel.write(byteBuffer);
    }
}

```

![img](https://img-blog.csdnimg.cn/20210304115743765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RlY2VtYmV0aW9u,size_16,color_FFFFFF,t_70#pic_center)

#### Netty IO模型

![img](https://static001.geekbang.org/resource/image/03/04/034756f1d76bb3af09e125de9f3c2f04.png)

### Netty组件与原理

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F6099975-a745f8dbeb3a597c.png&refer=http%3A%2F%2Fupload-images.jianshu.io&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1632869478&t=41a3d60424dbe1b7bbe5298ae0f7fb33)

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.yasinshaw.com%2F201907%2F11%2F4036B5EE5F8D.jpg&refer=http%3A%2F%2Ffile.yasinshaw.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1632869447&t=8c7a16a87aeaef2f3c5357b22d7b9bf1)

优点与注意示项

### 应用示例

### 总结

### 参考

[Netty 官网](https://netty.io/)

[DotNetty](https://github.com/Azure/DotNett)

[Scalable IO in Java](http://gee.cs.oswego.edu/dl/cpjslides/nio.pdf)

