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
- [浏览器内核的理解](#浏览器内核的理解)
- [离线储存怎么使用及原理](#离线储存怎么使用及原理)
- [如何实现浏览器内多个标签页之间的通信?](#如何实现浏览器内多个标签页之间的通信?)
- [websocket如何兼容低浏览器](#websocket如何兼容低浏览器)
- [从浏览器地址栏输入url到显示页面的步骤](#从浏览器地址栏输入url到显示页面的步骤)
- [浏览器http缓存](#浏览器http缓存)
- [计算机网络的分层](#计算机网络的分层)
- [http与https](#http与https)
- [http的request和response报文结构](#http的request和response报文结构)
- [请求头contenttype的几种值](#请求头contenttype的几种值)
- [http状态码](#http状态码)
- [cdn原理及更新机制](#cdn原理及更新机制)
- [如何进行网站性能优化](#如何进行网站性能优化)
- [web安全](#web安全)
- [解决跨域问题该如何处理cookie](#解决跨域问题该如何处理cookie)
- [cors请求的类别](#cors请求的类别)
- [面向对象的三大特性](#面向对象的三大特性)
- [常见的算法](#常见的算法)

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
>   - 服务端解密信息:服务端用私钥解密后，得到了客户端传过来的随机值，然后把内容通过该值进行对称加密
>   - 传输加密后的信息:服务端用随机值加密后的信息，可以在客户端被还原。
>   - 客户端解密信息:客户端用之前生成的随机值解密服务段传过来的信息。


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

## 请求头contenttype的几种值
- application/x-www-form-urlencoded：
    - 浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据。
    - 提交的数据放于Form Data，提交的数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL 转码，以tomcat作为服务器的后端可通过request.getParameter(name)获取参数
- multipart/form-data：
    - 上传文件时必须设置contentType为这个值
    - Content-Type 里指明了数据是以 mutipart/form-data 来编码，本次请求的 boundary 是什么内容。
    - 提交的数据放于RequestPayload，消息主体里按照字段个数又分为多个结构类似的部分，每部分都是以 –boundary 开始，紧接着内容描述信息，然后是回车，最后是字段具体内容（文本或二进制）。如果传输的是文件，还要包含文件名和文件类型信息。消息主体最后以 –boundary– 标示结束。
- application/json：
    - 消息主体是序列化后的 JSON 字符串,可用于传输复杂结构的数据
    - 提交的数据放于RequestPayload
- text/plain：
    - 原生ajax的默认值，提交的数据放于RequestPayload
## http状态码
> 1**(信息类)：表示接收到请求并且继续处理<br>
> - 100——客户必须继续发出请求
> - 101——客户要求服务器根据请求转换HTTP协议版本

> 2**(响应成功)：表示动作被成功接收、理解和接受
> -	200——表明该请求被成功地完成，所请求的资源发送回客户端
> -	201——提示知道新文件的URL
> -	202——接受和处理、但处理未完成
> -	203——返回信息不确定或不完整
> -	204——请求收到，但返回信息为空
> -	205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
> -	206——服务器已经完成了部分用户的GET请求

> 3**(重定向类)：为了完成指定的动作，必须接受进一步处理
> -	300——请求的资源可在多处得到
> -	301——本网页被永久性转移到另一个URL
> -	302——请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
> -	303——建议客户访问其他URL或访问方式
> -	304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用
> -	305——请求的资源必须从服务器指定的地址得到
> -	306——前一版本HTTP中使用的代码，现行版本中不再使用
> -	307——申明请求的资源临时性删除

> 4**(客户端错误类)：请求包含错误语法或不能正确执行
> -	400——客户端请求有语法错误，不能被服务器所理解
> -	401——请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
> 	HTTP 401.1 - 未授权：登录失败<br>
> -	　　HTTP 401.2 - 未授权：服务器配置问题导致登录失败
> -	　　HTTP 401.3 - ACL 禁止访问资源
> -	　　HTTP 401.4 - 未授权：授权被筛选器拒绝
> -	    HTTP 401.5 - 未授权：ISAPI 或 CGI 授权失败
> -	402——保留有效ChargeTo头响应
> -	403——禁止访问，服务器收到请求，但是拒绝提供服务
> 	HTTP 403.1 禁止访问：禁止可执行访问<br>
> -	　　HTTP 403.2 - 禁止访问：禁止读访问
> -	　　HTTP 403.3 - 禁止访问：禁止写访问
> -	　　HTTP 403.4 - 禁止访问：要求 SSL
> -	　　HTTP 403.5 - 禁止访问：要求 SSL 128
> -	　　HTTP 403.6 - 禁止访问：IP 地址被拒绝
> -	　　HTTP 403.7 - 禁止访问：要求客户证书
> -	　　HTTP 403.8 - 禁止访问：禁止站点访问
> -	　　HTTP 403.9 - 禁止访问：连接的用户过多
> -	　　HTTP 403.10 - 禁止访问：配置无效
> -	　　HTTP 403.11 - 禁止访问：密码更改
> -	　　HTTP 403.12 - 禁止访问：映射器拒绝访问
> -	　　HTTP 403.13 - 禁止访问：客户证书已被吊销
> -	　　HTTP 403.15 - 禁止访问：客户访问许可过多
> -	　　HTTP 403.16 - 禁止访问：客户证书不可信或者无效
> -	    HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
> -	404——一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg：输入了错误的URL
> -	405——用户在Request-Line字段定义的方法不允许
> -	406——根据用户发送的Accept拖，请求资源不可访问
> -	407——类似401，用户必须首先在代理服务器上得到授权
> -	408——客户端没有在用户指定的饿时间内完成请求
> -	409——对当前资源状态，请求不能完成
> -	410——服务器上不再有此资源且无进一步的参考地址
> -	411——服务器拒绝用户定义的Content-Length属性请求
> -	412——一个或多个请求头字段在当前请求中错误
> -	413——请求的资源大于服务器允许的大小
> -	414——请求的资源URL长于服务器允许的长度
> -	415——请求资源不支持请求项目格式
> -	416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
> -	417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。

> 5**(服务端错误类)：服务器不能正确执行一个正确的请求
> -	HTTP 500 - 服务器遇到错误，无法完成请求
> -	　　HTTP 500.100 - 内部服务器错误 - ASP 错误
> -	　　HTTP 500-11 服务器关闭
> -	　　HTTP 500-12 应用程序重新启动
> -	　　HTTP 500-13 - 服务器太忙
> -	　　HTTP 500-14 - 应用程序无效
> -	　　HTTP 500-15 - 不允许请求 global.asa
> -	　　Error 501 - 未实现
> -  HTTP 502 - 网关错误
> -  HTTP 503：由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常

## cdn原理及更新机制
> CDN有个源站的概念，源站就是提供内容的站点(网站的真实服务器), 从源站取内容的过程叫做回源。<br>
 <img src="{{ site.baseurl }}/img/CDN.png" alt="cdn"/>
> 用户在首次访问 https://assets-cdn.github.com/pinned-octocat.svg , 假设不委托local DNS服务器递归查询，会经历以下几个过程
> 1. 浏览器检查本地有没有这个东东的有效缓存，有则使用缓存，没有有效缓存则进行对assets-cdn.github.com的DNS查询，获得一个 CNAME记录, github.map.fastly.net,值得注意的是，多个加速域名可以解析到同一个CNAME，CDN回源和缓存的时候考虑到了hostname
> 2. 进行对github.map.fastly.net的DNS查询，获得一个A/AAAA记录，给出地址103.245.222.133（视网站不同返回的不一样，可以有多个）, 这一步对CDN来说时十分重要的，它给出了离用户最近的边缘节点；
> 3. 浏览器选一个返回的地址，然后进行真正的http请求，开始向103.245.222.133握手，握手完了把http请求头也发给了该边缘服务器;
> 4. 边缘服务器检查自己的cache里面有没有https://assets-cdn.github.com/pinned-octocat.svg这个资源，有则返回给用户，如果没有，向CDN中心服务器发起请求;
> 5. CDN中心服务器检查自己的cache里面有没有这个资源，有则返回给边缘服务器，没有则回源;
> 6. 中心服务器发现客户配置了github.map.fastly.net的回源地址(这个只有cdn会知道，假设是xxx.xxx.xxx.xxx)，就把http请求发到源站地址上，源站返回后返回给请求方;

> CDN加速的原理很大部分是跟DNS挂钩在一起的，CDN供应商几乎一定需要一个智能DNS服务器。CDN可以拿到所有的明文数据，所以对数据安全性、保密性要求比较高的企业会选择自建CDN或者设置NS记录，指向自建的智能DNS服务器。

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



## 解决跨域问题该如何处理cookie
- nigix转发：通过配置proxy_cookie_*的相关配置，使转发时带上cookie(ip等信息同样可以带上)
- CORS跨域：可通过设置Access-Control-Allow-Credentials:true来使请求带上cookie（客户端xhr.withCredentials也要设为true）
- 主域名相同：设置cookie的domain为主域名(如qq.com)

## cors请求的类别
- 简单请求-满足以下两个条件：
    1. 请求方法是HEAD、GET、POST中的一种
    2. HTTP的头信息不超出以下几种字段：
        - Accept
        - Accept-Language
        - Content-Language
        - Last-Event-ID
        - Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
- 对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。
- 非简单请求：
- 非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight），请求类型为options,预检"请求可通过accept-control-max-age控制缓存时间

## 面向对象的三大特性
>  - 封装：封装是指将客观事物抽象成类，每个类对自身的数据和方法实行保护。类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。
>  - 继承：继承是一个联结类的层次模型，并且允许和鼓励类的重用，它提供了一种明确表述共性的方法。对象的一个新类可以从现有的类中派生，这个过程叫做类继承。
>  - 多态：多态是指允许不同类的对象对同一消息作出响应。多态包括参数化多态和包含多态。多态语言具有灵活、抽象、行为共享、代码共享等优势，很好地解决了应用程序函数的同名问题。
>       - 参数化多态（编译多态）：函数名要相同，参数的个数或者参数的类型不同
>       - 包含多态（运行多态）：子类对父类的方法进行了重写

## 常见的算法
- 冒泡算法：逐个的比较
    - E A D B H   => A E D B H => A D E B H => A D B E H => A B D E H

            function bubleSort(arr){
                if({}.toString.call(arr) != "[object Array]"){
                    console.log("参数不是数组类型");
                    return [];
                }
                let len = arr.length;
                for(let outer = len;outer >= 2;outer--){
                    for(let inner = 0;inner < outer-1;inner++){
                        if(arr[inner] > arr[inner+1]){
                            let temp = arr[inner];
                            arr[inner] = arr[inner+1];
                            arr[inner+1] = temp;
                            //[arr[inner],arr[inner+1]] = [arr[inner+1],arr[inner]];
                        }
                    }
                }
                return arr;
            }

    - 平均时间复杂度为O(n^2),最坏情况为O(n^2),空间复杂度为O(1)

- 选择排序：从数组的开头开始，将第一个元素和其他元素作比较，检查完所有的元素后，最小的放在第一个位置，接下来再开始从第二个元素开始，重复以上一直到最后。
    - E A D B H  => A E D B H => A D E B H -> A B E D H => A B D E H

            function selecSort(arr){
                if({}.toString.call(arr) != "[object Array]"){
                    console.log("参数不是数组类型");
                    return [];
                }
                let len = arr.length;
                for(let i = 0;i < len-1;i++){
                    for(let j = i;j < len;j++){
                        if(arr[i] > arr[j]){
                            [arr[i],arr[j]] = [arr[j],arr[i]];
                        }
                    }
                }
            }

    - 平均时间复杂度为O(n^2),最坏为O(n^2),空间复杂度为O(1)

- 插入排序：将待排序的第一个记录作为一个有序段，从第二个开始，到最后一个，依次和前面的有序段进行比较，确定插入位置
    - E A D B H => A E D B H => A D E B H => A B D E H 

            function insertSort(arr){
                if({}.toString.call(arr) != "[object Array]"){
                    console.log("参数不是数组类型");
                    return [];
                }
                let len = arr.length;
                for(let i = 1;i < len;i++){
                    for(let j = i;j > 0;j--){
                        if(arr[j] < arr[j-1]){
                            [arr[j],arr[j-1]] = [arr[j-1],arr[j]];
                        }else{
                            break;
                        }
                    }
                }
                return arr;
            }

    - 平均时间复杂度O(n^2),最坏为O(n^2),空间复杂度为O(1)
    - 插入排序在前面为有序的情况下的时间复杂度为O(n)

- 快速排序：通过递归的方式将数据依次分解为包含较小元素和较大元素的不同子序列。该算法不断重复这个步骤直至所有数据都是有序的。
    - 1.选择一个基准元素，将列表分割成两个子序列；2.对列表重新排序，将所有小于基准值的元素放在基准值前面，所有大于基准值的元素放在基准值的后面；3.分别对较小元素的子序列和较大元素的子序列重复步骤1和2
    - 6, 3, 45,2 , 55, 23, 5, 4 => 3,2,5,4,6,45,55,23 => 2,3,5,4,6,23,45,55 => 2,3,4,5,6,23,45,55

            function quickSort(arr){
                if(arr.length <= 1){
                    return arr;
                }
                let left = [];
                let right = [];
                let current = arr.splice(0,1);
                for(let i = 0; i < arr.length;i++){
                    if(arr[i] < current){
                        left.push(arr[i])
                    }else{
                        right.push(arr[i])
                    }
                }
                return quickSort(left).concat(current,quickSort(right));
            }

    - 平均时间复杂度O(nlogn),最坏O(n^2),空间复杂度O(logn)

    
---



