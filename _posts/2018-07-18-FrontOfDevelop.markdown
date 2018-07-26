---
layout:     post
title:      "前端综合"
subtitle:   "面试题归纳"
date:       2018-07-18
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---

## 前端综合

> 主要是前端的综合题目

## 目录
1. [浏览器内核的理解](#浏览器内核的理解)
2. [离线储存怎么使用及原理](#离线储存怎么使用及原理)
3. [如何实现浏览器内多个标签页之间的通信?](#如何实现浏览器内多个标签页之间的通信?)
4. [websocket如何兼容低浏览器](#websocket如何兼容低浏览器)
5. [从浏览器地址栏输入url到显示页面的步骤](#从浏览器地址栏输入url到显示页面的步骤)
6. [浏览器http缓存](#浏览器http缓存)
7. [计算机网络的分层](#计算机网络的分层)
8. [http与https](#http与https)
9. [http的request和response报文结构](#http的request和response报文结构)
10. [如何进行网站性能优化](#如何进行网站性能优化)
11. [web安全](#web安全)
12. [从打开app到刷新出内容整个过程中都发生了什么](#从打开app到刷新出内容整个过程中都发生了什么)
13. [面向对象的三大特性](#面向对象的三大特性)
14. [设计模式](#设计模式)

## 浏览器内核的理解
>  内核一般指的是浏览器渲染进程，包含GUI渲染线程，js引擎线程，事件触发线程，定时触发器线程，异步http请求线程。GUI渲染线程负责渲染浏览器界面，解析HTML，CSS，构建DOM树和RenderObject树，布局和绘制等；JS引擎线程负责解析Javascript脚本，运行代码。归属于浏览器而不是JS引擎，用来控制事件循环；定时触发器线程来计时并触发定时；在XMLHttpRequest在连接后是通过浏览器新开一个线程请求，就是异步http请求线程

>  常见内核：
>  1. Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML] 
>  2. Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等 
>  3. Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;] 
>  4. Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）] 

>  相关文章：[浏览器内核的解析和对比](http://www.cnblogs.com/fullhouse/archive/2011/12/19/2293455.html) 

## 离线储存怎么使用及原理 
>  如何使用
>  1. 页面头部像下面一样加入一个manifest的属性;
>  2. 在cache.manifest文件的编写离线存储的资源
>     CACHE MANIFEST
        #v0.11
        CACHE:
        js/app.js
        css/style.css
        NETWORK:
        resourse/logo.png
        FALLBACK:
        / /offline.html
>  3. 在离线状态时，操作window.applicationCache进行需求实现。 

>  原理：HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

## 如何实现浏览器内多个标签页之间的通信?
>  1. WebSocket、SharedWorker；<br/>
>  注：Chrome浏览器为SharedWorker单独创建一个进程来运行JavaScript程序，在浏览器中每个相同的JavaScript只存在一个SharedWorker进程，不管它被创建多少次。
>  2. 调用localstorge、cookies等本地存储方式;localstorge另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件(storge事件)，我们通过监听事件，控制它的值来进行页面信息通信；<br/>
>  注意： Safari 在无痕模式下设置localstorge值时会抛出 QuotaExceededError 的异常


## websocket如何兼容低浏览器
>  1. Adobe Flash Socket
>  2. 基于http-stream通信(ActiveX HTMLFile (IE)、基于 multipart 编码发送 XHR及iframe),原理是让客户端在一次请求中保持和服务端连接不断开，然后服务端源源不断传送数据给客户端，就好比数据流一样，并不是一次性将数据全部发给客户端。
>     1. 基于 multipart 编码发送 XHR:构造一个XHR对象，通过监听它的onreadystatechange事件，当它的readyState为3的时候，获取它的responseText然后进行处理，readyState为3表示数据传送中，整个通信过程还没有结束，所以它还在不断获取服务端发送过来的数据，直到readyState为4的时候才表示数据发送完毕，一次通信过程结束。（低版本IE不支持readyState为3时获取其responseText属性）
>     2. 基于iframe的数据流:  在浏览器中动态载入一个iframe,让它的src属性指向请求的服务器的URL，实际上就是向服务器发送了一个http请求，然后在浏览器端创建一个处理数据的函数，在服务端通过iframe与浏览器的长连接定时输出数据给客户端，但是这个返回的数据并不是一般的数据，而是一个类似于<script type=\"text/javascript\">parent.process('"+randomNum.toString()+"')</script>脚本执行的方式，浏览器接收到这个数据就会将它解析成js代码并找到页面上指定的函数去执行，实际上是服务端间接使用自己的数据间接调用了客户端的代码，达到实时更新客户端的目的。（浏览器一直处于加载状态）
>     3. ActiveX HTMLFile (IE): 在IE中，动态生成一个htmlfile对象，这个对象ActiveX形式的com组件，它实际上就是一个在内存中实现的HTML文档，通过将生成的iframe添加到这个内存中的HTMLfile中，并利用iframe的数据流通信方式达到上面的效果。
>  3. 基于长轮询的 XHR

## 从浏览器地址栏输入url到显示页面的步骤
>  1. 浏览器输入url，浏览器主进程接管，开一个下载线程，然后进行 http请求（略去DNS查询，IP寻址等等操作），然后等待响应，获取内容，随后将内容通过RendererHost接口转交给Renderer进程,浏览器渲染流程开始
>     1. 查看缓存，如果请求资源在缓存中并且有效，跳转到转码步骤
>         - 如果资源未缓存，发起新请求
>         - 如果已缓存，检验是否足够新鲜，足够新鲜直接提供给客户端，否则与服务器进行验证。
>         - 检验有效通常有两个HTTP头进行控制Expires和Cache-Control：
>             - HTTP1.0提供Expires，值为一个绝对时间表示缓存新鲜日期
>             - HTTP1.1增加了Cache-Control: max-age=,值为以秒为单位的最大新鲜时间
>     2. 解析URL获取协议，主机，端口，path
>     3. 组装一个http请求报文（应用层）
>     4. 分割一个http请求报文为多个数据包（传输层）
>     5. 获取目标IP地址和mac 地址（网络层）
>         1. 浏览器缓存
>         2. 本机缓存
>         3. hosts文件
>         4. 路由器缓存
>         5. ISP DNS缓存
>         6. DNS递归查询（可能存在负载均衡导致每次IP不一样）
>         7. mac 地址根据IP地址通过ARP协议查出
>     6. 打开一个socket与目标IP地址，端口建立TCP链接，三次握手如下
>           1. 客户端发送一个TCP的SYN=1，Seq=X的包到服务器端口
>           2. 服务端发回一个SYN=1，ACK=X+1,Seq=Y的响应包
>           3. 客户端发送一个ACK=Y+1,Seq=Z的包
>     7. TCP链接建立后发送HTTP请求
>     8. 服务器接受请求并解析，将请求转发到服务程序
>     9. 服务器检查HTTP请求头是否包含缓存验证信息如果验证缓存新鲜，返回304等对应状态码
>     10. 处理程序读取完整请求并准备HTTP响应，可能需要查询数据库等操作
>     11. 服务器将响应报文通过TCP连接发送回浏览器
>     12. 浏览器接收HTTP响应，然后根据情况选择关闭TCP连接或者保留重用，关闭TCP连接的四次握手如下：
>           1. 主动方发送Fin=1， Ack=Z， Seq= X报文
>           2. 被动方发送ACK=X+1， Seq=Z报文
>           3. 被动方发送Fin=1， ACK=X， Seq=Y报文
>           4. 主动方发送ACK=Y， Seq=X报文
>     13. 浏览器检查响应状态吗：是否为1XX，3XX， 4XX， 5XX，这些情况处理与2XX不同
>     14. 判断资源是否缓存；对响应进行解码
>  2. GUI渲染线程解析html建立dom树;解析css构建render树（将CSS代码解析成树形的数据结构，然后结合DOM合并成render树）;布局render树（Layout/reflow），负责各元素尺寸、位置的计算;绘制render树（paint），绘制页面像素信息;浏览器会将各层的信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上。
>    - 注：构建DOM过程遇到外部资源会交给下载线程，不会阻塞GUI渲染的DOM构建,但js下载完会立即通过js引擎线程解析，js引擎与GUI线程互斥，因此会阻塞GUI渲染
>  3. js解析：
>    - 浏览器创建Document对象并解析HTML，将解析到的元素和文本节点添加到文档中，此时document.readystate为loading
>    - 当GUI渲染线程遇到设置了async属性的script时，开始下载脚本并继续解析文档。脚本会在它下载完成后尽快执行，但是解析器不会停下来等它下载。异步脚本禁止使用document.write()，它们可以访问自己script和之前的文档元素
>    - 当文档完成解析，document.readState变成interactive
>    - 所有defer脚本会按照在文档出现的顺序执行，延迟脚本能访问完整文档树，禁止使用document.write()
>    - 浏览器在Document对象上触发DOMContentLoaded事件
>    - 此时文档完全解析完成，浏览器可能还在等待如图片等内容加载，等这些内容完成载入并且所有异步脚本完成载入和执行，document.readState变为complete,window触发load事件


## 浏览器http缓存
> 浏览器HTTP缓存可以分为强缓存和协商缓存。强缓存命中的话不会发请求到服务器（比如chrome中的200 from memory cache），协商缓存一定会发请求到服务器，通过资源的请求首部字段验证资源是否命中协商缓存，如果协商缓存命中，服务器会将这个请求返回，但是不会返回这个资源的实体，而是通知客户端可以从缓存中加载这个资源（304 not modified）。<br/>
>浏览器HTTP缓存由HTTP报文的首部字段决定；强缓存包含cache-control,expires,协商缓存包含Last-Modified/If-Modified-Since和ETag/If-None-Match.<br/>
> - cache-control:Cache-Control是一个通用首部字段,值包括max-age,no-cache等；max-age（单位为s）设置缓存的存在时间，相对于发送请求的时间。只有响应报文首部设置Cache-Control为非0的max-age或者设置了大于请求日期的Expires（下文会讲）才有可能命中强缓存。当满足这个条件，同时响应报文首部中Cache-Control不存在no-cache、no-store且请求报文首部不存在Pragma字段，才会真正命中强缓存。
> - expire：Expires是一个响应首部字段，它指定了一个日期/时间，在这个时间/日期之前，HTTP缓存被认为是有效的。如果同时设置了Cache-Control响应首部字段的max-age，则Expires会被忽略。
> - Last-Modified/If-Modified-Since：If-Modified-Since是一个请求首部字段，并且只能用在GET或者HEAD请求中。Last-Modified是一个响应首部字段，包含服务器认定的资源作出修改的日期及时间。如果Last-Modified的时间早于或等于If-Modified-Since则会返回一个不带主体的304响应。
> - ETag/If-None-Match ：ETag是一个响应首部字段，根据实体内容生成的一段hash字符串，标识资源的状态，由服务端产生。If-None-Match是一个条件式的请求首部。如果请求资源时在请求首部加上这个字段，值为之前服务器端返回的资源上的ETag，当服务器上有资源的ETag属性值与之相同，会返回不带实体的304响应。ETag优先级比Last-Modified高


## 计算机网络的分层
> OSI七层协议：应用层，表示层，会话层，传输层，网络层，数据链路层，物理层
 <img src="{{ site.baseurl }}/img/osi.png" alt="osi"/>
> TCP/IP协议：应用层，传输层，网络层，链路层（另一种分法：将链路层分为数据链路层和物理层）<br/>
>   - 应用层：提供各种应用应用服务，http协议，DNS协议，ftp协议都在这一层
>   - 传输层：规定网络传输时的数据格式，数据包是网络传输时的最小单位,包含TCP协议，UDP协议
>   - 网络层：规定了数据传输的线路，包含IP协议
>   - 链路层：包含网络传输的硬件部分
>   - 下面以http为例：<br/>
>       1.发送方的客户端在应用层（http）发出某个web页面的请求  
>       2.为了传输方便，传输层将从应用层收到的http报文分割，打上标记发给网络层  
>       3.网络通过IP地址，找到mac地址，发给链路层 
>       4.链路层进行发送，服务端的链路层接受到数据后再一层层往上发，直到服务端的应用层 

## http与https
>   - HTTP协议传输的数据都是未加密的，也就是明文的,使用HTTP协议传输隐私信息非常不安全，为了保证这些隐私数据能加密传输,设计了SSL（Secure Sockets Layer）协议用于对HTTP协议传输的数据进行加密，从而就诞生了HTTPS。主要区别如下：
>   1. https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用
>   2. http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
>   3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
>   4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
>   - 采用HTTPS协议的服务器必须要有一套数字证书，可以自己制作，也可以向组织申请，区别就是自己颁发的证书需要客户端验证通过，才可以继续访问，而使用受信任的公司申请的证书则不会弹出提示页面(startssl就是个不错的选择，有1年的免费服务)。这套证书其实就是一对公钥和私钥,公钥用来加密数据，私钥用来解密存于服务端
>   - 传送证书：其实就是公钥，只是包含了很多信息，如证书的颁发机构，过期时间等等
>   - 客户端解析证书：有客户端的TLS来完成的，首先会验证公钥是否有效，比如颁发机构，过期时间等等，如果发现异常，则会弹出一个警告框，提示证书存在问题，如果证书没有问题，那么就生成一个随机值，然后用证书对该随机值进行加密
>   - 传送加密信息: 传送的是用证书加密后的随机值，目的就是让服务端得到这个随机值，以后客户端和服务端的通信就可以通过这个随机值来进行加密解密了
>   - 服务端解密信息:服务端用私钥解密后，得到了客户端传过来的随机值(私钥)，然后把内容通过该值进行对称加密
>   - 传输加密后的信息:服务端用私钥加密后的信息，可以在客户端被还原。
>   - 客户端解密信息:客户端用之前生成的私钥解密服务段传过来的信息。


## http的request和response报文结构
> - request结构
> 1. 首行是Request-Line包括：请求方法，请求URI，协议版本，CRLF
> 2. 首行之后是若干行请求头，包括general-header，request-header或者entity-header，每个一行以CRLF结束
> 3. 请求头和消息实体之间有一个CRLF分隔
> 4. 根据实际请求需要可能包含一个消息实体 
> - response结构
> 1. 首行是状态行包括：HTTP版本，状态码，状态描述，后面跟一个CRLF
> 2. 首行之后是若干行响应头，包括：通用头部，响应头部，实体头部
> 3. 响应头部和响应实体之间用一个CRLF空行分隔
> 4. 最后是一个可能的消息实体

## 如何进行网站性能优化
> - content方面:
>     1. 减少HTTP请求：合并文件、CSS精灵、inline Image
>     2. 减少DNS查询：DNS查询完成之前浏览器不能从这个主机下载任何任何文件。方法：DNS缓存、将资源分布到恰当数量的主机名，平衡并行下载和DNS查询
>     3. 避免重定向：多余的中间访问
>     4. 使Ajax可缓存
>     5. 非必须组件延迟加载,未来所需组件预加载
>     6. 减少DOM元素数量,减少iframe数量
>     7. 将资源放到不同的域下：浏览器同时从一个域下载资源的数目有限，增加域可以提高并行下载量
>     8. 避免空src的img标签
> - Server方面:
>     1. 使用CDN
>     2. 添加Expires或者Cache-Control响应头
>     3. 对组件使用Gzip压缩
>     4. 配置ETag
>     5. Flush Buffer Early
>     6. Ajax使用GET进行请求
>     7. 减小cookie大小,引入资源的域名不要包含cookie
> - css方面:
>     1. 将样式表放到页面顶部
>     2. 不使用CSS表达式
>     3. 使用不使用@import
>     4. 不使用IE的Filter
> - Javascript方面:
>     1. 将脚本放到页面底部
>     2. 将javascript和css从外部引入
>     3. 压缩javascript和css
>     4. 删除不需要的脚本
>     5. 减少DOM访问
>     6. 合理设计事件监听器
> - 图片方面:
>     1. 优化图片：根据实际颜色需要选择色深、压缩
>     2. 优化css精灵
>     3. 不要在HTML中拉伸图片
>     4. 保证favicon.ico小并且可缓存


## web安全
> 关于XSS与CSRF
> - xss(跨站脚本攻击)：当浏览器渲染HTML文档的过程中出现不被预期的脚本指令并执行时，xss就会发生。xss分为存储型、DOM 型，反射型。
>   1. 存储型：将xss攻击代码提交到服务端存储，如评论功能，用户查询
>   2. DOM型：只发生在客户端的攻击，利用DOM元素的插入等操作执行XSS代码
>   3. 反射型：将用户输入存在xss攻击的代码发送到服务端，服务端未存储直接返回，浏览器渲染解析并执行xss代码<br/>
> 防御：
>   1. 将标签闭合符合过滤转义
>   2. 利用xss过滤库设置白名单，校验不安全变量，如script，这种一般用于富文本
>   3. 将URL内容进行编码，大部分浏览器已有
> - CSRF(跨站请求伪造)：在用户不知情的情况下做一些不合法的操作
> - 场景：目标网站A有某项功能，在恶意站点放一个CSRF页面，利用img或iframe等请求目标网站A的功能，若此时用户在浏览器已登录A站，而A站的该功能存在csrf漏洞，未做请求来源判断等，则会造成用户不想看到的结果，攻击成功。
>   1. HTML CSRF攻击：基于html元素，link,img,iframe等
>   2. JSON HiJacking攻击：利用eval解析JSON执行代码
>   1. Flash CSRF攻击：通过ActionScript脚本进行攻击



## 从打开app到刷新出内容整个过程中都发生了什么



## 面向对象的三大特性
>  - 封装：封装是指将客观事物抽象成类，每个类对自身的数据和方法实行保护。类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。
>  - 继承：继承是一个联结类的层次模型，并且允许和鼓励类的重用，它提供了一种明确表述共性的方法。对象的一个新类可以从现有的类中派生，这个过程叫做类继承。
>  - 多态：多态是指允许不同类的对象对同一消息作出响应。多态包括参数化多态和包含多态。多态语言具有灵活、抽象、行为共享、代码共享等优势，很好地解决了应用程序函数的同名问题。
>       - 参数化多态（编译多态）：函数名要相同，参数的个数或者参数的类型不同
>       - 包含多态（运行多态）：子类对父类的方法进行了重写

---



