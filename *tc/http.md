---
网络基础
---

1.在浏览器输入一个 URL 的整体过程是怎么样的

-   [从输入url到页面展示到底发生了什么](https://mp.weixin.qq.com/s/7eY3XIhLXeCMqBYIQh6WwA)

3.描述一下浏览器缓存

-   [《浏览》器缓存机制浅析](https://mp.weixin.qq.com/s/F5gvzdi6MTwCFXV9LKs9NQ)

```text
非HTTP协议定义的缓存机制
    <META HTTP-EQUIV="Pragma" CONTENT="no-cache">  // 自己试的谷歌浏览器不支持
Expires1.0与Cache-Control1.1(max-age) 强缓存 http status 200(from memory cache)
    Cache-Control
        no-cache 200 正常请求
        max-age=0  走协商缓存
Last-Modified1.0,Etag1.1 协商缓存 http status 304
区别 1.Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间
    2.如果某些文件会被定期生成，但有时内容并没有任何变化（仅仅改变了时间），但Last-Modified却改变了，导致文件没法使用缓存
    3.有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形

post无法缓存

```

4.http

-   [《HTTP 协议》](https://juejin.im/post/5cd0438c6fb9a031ec6d3ab2#heading-0)_`外链`_

```text
HTTP2.0的多路复用和HTTP1.X中的长连接复用有什么区别？
    HTTP/1.* 一次请求-响应，建立一个连接，用完关闭；每一个请求都要建立一个连接；

    HTTP/1.1 pipelining解决方式为，若干个请求排队串行化单线程处理，后面的请求等待前面请求的返回才能获得执行机会，
    一旦有某请求超时等，后续请求只能被阻塞，毫无办法，也就是人们常说的线头阻塞；
    虽然管道化，可以一次发送多个请求，但是响应仍是顺序返回，仍然无法解决队头阻塞的问题

    HTTP/2多个请求可同时在一个连接上并行执行。某个请求任务耗时严重，不会影响到其它连接的正常执行；

http1.1 长连接\ eTag \管道机制(pipelining) \支持断点续传，通过使用请求头中的 Range 来实现
http2.0 多路服务 \头部压缩(字典) \服务端推送\ 新的二进制格式
http3.0 避免包阻塞 基于UDP的QUIC协议
    快速重启会话： 普通基于tcp的连接，是基于两端的ip和端口和协议来建立的。在网络切换场景，例如手机端切换了无线网，使用4G网络，
    会改变本身的ip，这就导致tcp连接必须重新创建。而QUIC协议使用特有的UUID来标记每一次连接，在网络环境发生变化的时候，只要UUID不变，
    就能不需要握手，继续传输数据。

http报文
    请求报文 是由请求行（请求方法、协议版本）、请求首部（请求URI、客户端信息等）和内容实体（用户信息和资源信息等，可为空）构成。

    响应报文 是由状态行（协议版本、状态码）、响应首部（服务器名称、资源标识等）和内容实体（服务端返回的资源信息）构成。
```

13.为什么 HTTP1.1 不能实现多路复用

```text
HTTP/1.1 不是二进制传输，而是通过文本进行传输。由于没有流的概念，在使用并行传输（多路复用）传递数据时，接收端在接收到响应后，
并不能区分多个响应分别对应的请求，所以无法将多个响应的结果重新进行组装，也就实现不了多路复用。
```

5.HTTP 与 HTTPS 的区别

```text
Http 协议运行在 TCP 之上，明文传输，客户端与服务器端都无法验证对方的身份；Https 是身披 SSL(Secure Socket Layer)外壳的 Http，
    运行于 SSL 上，SSL 运行于 TCP 之上，是添加了加密和认证机制的 HTTP。二者之间存在如下不同：

端口不同：Http 与 Http 使用不同的连接方式，用的端口也不一样，前者是 80，后者是 443；
资源消耗：和 HTTP 通信相比，Https 通信会由于加减密处理消耗更多的 CPU 和内存资源；
开销：Https 通信需要证书，而证书一般需要向认证机构购买；
Https 的加密机制是一种共享密钥加密和公开密钥加密并用的混合加密机制。
```

6.查找域名对应的 IP 地址

```text
浏览器搜索自己的 DNS 缓存（维护一张域名与 IP 地址的对应表）；
搜索操作系统中的 DNS 缓存（维护一张域名与 IP 地址的对应表）；
搜索操作系统的 hosts 文件（ Windows 环境下，维护一张域名与 IP 地址的对应表）；
操作系统将域名发送至 LDNS（本地区域名服务器），LDNS 查询 自己的 DNS 缓存（一般查找成功率在 80% 左右），查找成功则返回结果，失败则发起一个迭代 DNS 解析请求：
LDNS 向 RootName Server （根域名服务器，如 com、net、org 等的解析的顶级域名服务器的地址）发起请求，此处，Root Name Server 返回 com 域的顶级域名服务器的地址；
LDNS 向 com 域的顶级域名服务器发起请求，返回 baidu.com 域名服务器地址；
LDNS 向 baidu.com 域名服务器发起请求，得到 www.baidu.com 的 IP 地址；
LDNS 将得到的 IP 地址返回给操作系统，同时自己也将 IP 地址缓存起来；
操作系统将 IP 地址返回给浏览器，同时自己也将 IP 地址缓存起来；
```

7.127.0.0.1 与 192.168.0.1 有什么区别

```text
首先明确二者没有区别！两个 IP 地址的角度不一样，127.0.0.1 是从 IETF（因特尔工程任务组）规定看，是保留给本机使用的 IP 地址，
所有的计算机默认都是相同的。而 192.168.0.1 其实只是 IETF 在 c 类网址中，专门留出给专用网络用的一个网段中的一个 IP 而已，
该网段包含了 192.168.0.1 到 192.168.255.255 中所有的 IP 地址。
```

8.https 介绍 HTTPS 握手过程

```text
HTTPS要使客户端与服务器端的通信过程得到安全保证，必须使用的对称加密算法，但是协商对称加密算法的过程，需要使用非对称加密算法来保证安全，
然而直接使用非对称加密的过程本身也不安全，会有中间人篡改公钥的可能性，所以客户端与服务器不直接使用公钥，
而是使用数字证书签发机构颁发的证书来保证非对称加密过程本身的安全。这样通过这些机制协商出一个对称加密算法，
就此双方使用该算法进行加密解密。从而解决了客户端与服务器端之间的通信安全问题。
数字签名(证书编号)

1. A => B 请求证书
2. B => A 用私钥加密数字签名 发送证书
3. A => B 用公钥解密数字签名,与证书上的算法算出的结果对比,用公钥加密协商的对称加密的算法
4  B => A 私钥解密 告诉A知道了
5  A <=>B 对称加密

其中 3
    （1）首先浏览器读取证书中的证书所有者、有效期等信息进行校验，校验证书的网站域名是否与证书颁发的域名一致，校验证书是否在有效期内
    （2）浏览器开始查找操作系统中已内置的受信任的证书发布机构CA，与服务器发来的证书中的颁发者CA比对，用于校验证书是否为合法机构颁发
    （3）如果找不到，浏览器就会报错，说明服务器发来的证书是不可信任的。
```

9.单线程

```text
进程：进程（英语：process），是cpu资源分配的最小单位。
线程：线程（英语：thread）是cpu调度的最小单位。
协程：协程（英语：coroutine），又称微线程，是计算机程序的一类组件，推广了协作式多任务的子程序，允许执行被挂起与被恢复。
```

10.浏览器架构

```text
浏览器主进程

    负责浏览器界面显示
    各个页面的管理，创建以及销毁
    将渲染进程的结果绘制到用户界面上
    网络资源管理
        ui线程 浏览器url输入框
        网络线程  协议 DNS等

GPU 进程

    用于 3D 渲染绘制

插件进程

    第三方插件处理，运行在沙箱中

渲染进程

    GUI渲染线程
    JS引擎线程
    事件触发线程
    定时触发器线程
    异步http请求线程
```

10.普通图层以及复合图层
[从浏览器多进程到JS单线程，JS运行机制最全面的一次梳理](https://segmentfault.com/a/1190000012925872)
```text
首先，普通文档流内可以理解为一个复合图层（这里称为默认复合层，里面不管添加多少元素，其实都是在同一个复合图层中）
其次，absolute布局（fixed也一样），虽然可以脱离普通文档流，但它仍然属于默认复合层。
然后，可以通过硬件加速的方式，声明一个新的复合图层，它会单独分配资源
（当然也会脱离普通文档流，这样一来，不管这个复合图层中怎么变化，也不会影响默认复合层里的回流重绘）

可以简单理解下：GPU中，各个复合图层是单独绘制的，所以互不影响，这也是为什么某些场景硬件加速效果一级棒

如何变成复合图层（硬件加速）
    最常用的方式：translate3d、translateZ
    opacity属性/过渡动画（需要动画执行的过程中才会创建合成层，动画没有开始或结束后元素还会回到之前的状态）
    <video><iframe><canvas><webgl>等元素
    其它，譬如以前的flash插件

可以看到，absolute虽然可以脱离普通文档流，但是无法脱离默认复合层。
所以，就算absolute中信息改变时不会改变普通文档流中render树，
但是，浏览器最终绘制时，是整个复合层绘制的，所以absolute中信息的改变，仍然会影响整个复合层的绘制。
（浏览器会重绘它，如果复合层中内容多，absolute带来的绘制信息变化过大，资源消耗是非常严重的）

而硬件加速直接就是在另一个复合层了（另起炉灶），所以它的信息改变不会影响默认复合层
（当然了，内部肯定会影响属于自己的复合层），仅仅是引发最后的合成（输出视图）

复合图层的作用？

一般一个元素开启硬件加速后会变成复合图层，可以独立于普通文档流中，改动后可以避免整个页面重绘，提升性能

但是尽量不要大量使用复合图层，否则由于资源消耗过度，页面反而会变的更卡

硬件加速时请使用index

使用硬件加速时，尽可能的使用index，防止浏览器默认给后续的元素创建复合层渲染

具体的原理时这样的：
**webkit CSS3中，如果这个元素添加了硬件加速，并且index层级比较低，
那么在这个元素的后面其它元素（层级比这个元素高的，或者相同的，并且releative或absolute属性相同的），
会默认变为复合层渲染，如果处理不当会极大的影响性能**

简单点理解，其实可以认为是一个隐式合成的概念：如果a是一个复合图层，而且b在a上面，那么b也会被隐式转为一个复合图层，这点需要特别注意
    
```

11.页面渲染

```text
浏览器在解析html文件时，会”自上而下“加载，并在加载过程中进行解析渲染。在解析过程中，如果遇到请求外部资源时，如图片、外链的CSS、iconfont等，
请求过程是异步的，并不会影响html文档进行加载。 css加载不会阻塞DOM树解析,但会阻塞render树渲染

1.构建Dom树
2.子资源加载 (网络线程 异步加载)
3.JavaScript 阻塞解析 (defer,async 后面是请求完 就开始加载)
    因为 JavaScript 可能改变文档的结构，比如用了 document.write() 之类的函数。这就是为什么 HTML 解析器必须在 JavaScript 执行过后才恢复对 HTML 文档的解析工作。
4.样式的计算
5.布局
6.重排 重绘
```

12.接口如何防刷

```text
接口次数限制
Refer   Header头
UA 验证 User-Agent Header头  浏览器信息
token 时效性
验证码
把某个key加配料，带上时间戳，加密，请求时带上，过期或解密失败则403。

前端如使用axios请求, 是有请求前拦截的, 把最近的一些请求地址加进数组, 请求前拦截器 判断一定时间内这个地址是否再出现 来控制请求频率
按钮Loading
```

14.为什么说 HTTP 是无状态的协议呢？

```text
因为它的每个请求都是完全独立的，每个请求包含了处理这个请求所需的完整的数据，发送请求不涉及到状态变更。

无状态协议的主要缺点在于，单个请求需要的所有信息都必须要包含在请求中一次发送到服务端，这导致单个消息的结构需要比较复杂，必须能够支持大量元数据，
因此HTTP消息的解析要比其他许多协议都要复杂得多。同时，这也导致了相同的数据在多个请求上往往需要反复传输，
例如同一个连接上的每个请求都需要传输Host、Authentication、Cookies、Server等往往是完全重复的元数据，
一定程度上降低了协议的效率。

至于HTTP/2，它应该算是一个有状态的协议了（有握手和GOAWAY消息，有类似于TCP的流控），所以以后说“HTTP是无状态的协议”就不太对了，最好说“HTTP 1.x是无状态的协议”

链接：https://www.zhihu.com/question/23202402/answer/527748675
```

15.幂等
```text
GET请求幂等，POST请求不幂等，幂等指发送 M 和 N 次请求（两者不相同且都大于1），服务器上资源的状态一致。
```
