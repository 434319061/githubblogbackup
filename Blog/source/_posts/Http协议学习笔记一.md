---
title: Http协议学习笔记(一)
date: 2020-05-12 21:04:33
tags: Http
---

#### 什么是HTTP？

​    互联网应用最广泛的一种网络协议，所有<u>www文件</u>必须遵守这个标准，设计最初目的是为了提供一种发布和接受<u>HTML</u>页面的方法。

> 超文本传输协议，是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据，互联网上应用最为广泛的一种网络协议,所有的WWW文件都必须遵守这个标准。设计HTTP的初衷是为了提供一种发布和接收HTML页面的方法。

HTTP由两部分组成：**请求**和**响应**。当你在Web浏览器中输入一个URL时，浏览器将根据你的要求创建并发送请求，该请求包含所输入的URL以及一些与浏览器本身相关的信息。当服务器收到这个请求时将返回一个响应，该响应包括与该请求相关的信息以及位于指定URL（如果有的话）的数据。直到浏览器解析该响应并显示出网页（或其他资源）为止。

常见的请求头：

> Accept：浏览器可接受的MIME类型。
> Accept - Charset：浏览器可接受的字符集。
> Accept - Encoding：浏览器能够进行解码的数据编码方式，比如gzip。Servlet能够向支持gzip的浏览器返回经gzip编码的HTML页面。许多情形下这可以减少5到10倍的下载时间。
> Accept - Language：浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到。
> Authorization：授权信息，通常出现在对服务器发送的WWW - Authenticate头的应答中。
> Connection：表示是否需要持久连接。如果Servlet看到这里的值为“Keep - Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接），它就可以利用持久连接的优点，当页面包含多个元素时（例如Applet，图片），显著地减少下载所需要的时间。要实现这一点，Servlet需要在应答中发送一个Content - Length头，最简单的实现方法是：先把内容写入ByteArrayOutputStream，然后在正式写出内容之前计算它的大小。
> Content - Length：表示请求消息正文的长度。
> Cookie：这是最重要的请求头信息之一，参见后面《Cookie处理》一章中的讨论。
> From：请求发送者的email地址，由一些特殊的Web客户程序使用，浏览器不会用到它。
> Host：初始URL中的主机和端口。
> If - Modified - Since：只有当所请求的内容在指定的日期之后又经过修改才返回它，否则返回304“Not Modified”应答。
> Pragma：指定“no - cache”值表示服务器必须返回一个刷新后的文档，即使它是代理服务器而且已经有了页面的本地拷贝。
> Referer：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。
> User - Agent：浏览器类型，如果Servlet返回的内容与浏览器类型有关则该值非常有用。
> UA - Pixels，UA - Color，UA - OS，UA - CPU：由某些版本的IE浏览器所发送的非标准的请求头，表示屏幕大小、颜色深度、操作系统和CPU类型。

#### 什么是HTTPS？

> 《图解HTTP》这本书中曾提过HTTPS是身披SSL外壳的HTTP。HTTPS是一种通过计算机网络进行安全通信的传输协议，经由HTTP进行通信，利用SSL/TLS建立全信道，加密数据包。HTTPS使用的主要目的是提供对网站服务器的身份认证，同时保护交换数据的隐私与完整性。
>
> PS:TLS是传输层加密协议，前身是SSL协议，由网景公司1995年发布，有时候两者不区分。



常见考点：

- **HTTP Method**

  HTTP Method是可以自定义的，且区分大小写。

  - HTTP/1.1协议中共定义了八种方法（有时也叫“动作”）来表明Request-URI指定的资源的不同操作方式：

    **OPTIONS**
    返回服务器针对特定资源所支持的HTTP请求方法。也可以利用向Web服务器发送'*'的请求来测试服务器的功能性。
    **HEAD**
    向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息头中的元信息。
    **GET**
    向特定的资源发出请求。注意：GET方法不应当被用于产生“副作用”的操作中。
    **POST**
    向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
    **PUT**
    向指定资源位置上传其最新内容。
    **DELETE**
    请求服务器删除Request-URI所标识的资源。
    **TRACE**
    回显服务器收到的请求，主要用于测试或诊断。
    **CONNECT**
    HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。 

- CTF

  1.要求从某个**IP或者主机**访问，一般是修改 X-Forwarded-For，X-Forwarded-Host，CLIENT-IP,host的参数的值

  2.要求从**某个国家**访问，一般修改Accept-Language值

  3.要求从**某个页面**访问就修改referer，有关身份认证类的一般再修改cookie的值

- **HTTP 头部解释** 

   

  \1. Accept：告诉WEB服务器自己接受什么介质类型，*/* 表示任何类型，type/* 表示该类型下的所有子类型，type/sub-type。 

  \2. Accept-Charset： 浏览器申明自己接收的字符集 
  Accept-Encoding： 浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法 （gzip，deflate） 
  Accept-Language：：浏览器申明自己接收的语言语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等。 

  \3. Accept-Ranges：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。 

  \4. Age：当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。 

  \5. Authorization：当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，该头部来回应自己的身份验证信息给WEB服务器。 

  \6. Cache-Control：请求：no-cache（不要缓存的实体，要求现在从WEB服务器去取） 
  max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象） 
  max-stale：（可以接受过去的对象，但是过期时间必须小于 
  max-stale 值） 
  min-fresh：（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的 
  缓存对象） 
  响应：public(可以用 Cached 内容回应任何用户) 
  private（只能用缓存内容回应先前请求该内容的那个用户） 
  no-cache（可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端） 
  max-age：（本响应包含的对象的过期时间） 
  ALL: no-store（不允许缓存） 

  \7. Connection：请求：close（告诉WEB服务器或者代理服务器，在完成本次请求的响应 
  后，断开连接，不要等待本次连接的后续请求了）。 
  keepalive（告诉WEB服务器或者代理服务器，在完成本次请求的 
  响应后，保持连接，等待本次连接的后续请求）。 
  响应：close（连接已经关闭）。 
  keepalive（连接保持着，在等待本次连接的后续请求）。 
  Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持 
  连接多长时间（秒）。 
  例如：Keep-Alive：300 

  \8. Content-Encoding：WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。 
  例如：Content-Encoding：gzip 
  Content-Language：WEB 服务器告诉浏览器自己响应的对象的语言。 
  Content-Length： WEB 服务器告诉浏览器自己响应的对象的长度。 
  例如：Content-Length: 26012 
  Content-Range： WEB 服务器表明该响应包含的部分对象为整个对象的哪个部分。 
  例如：Content-Range: bytes 21010-47021/47022 
  Content-Type： WEB 服务器告诉浏览器自己响应的对象的类型。 
  例如：Content-Type：application/xml 

  \9. ETag：就是一个对象（比如URL）的标志值，就一个对象而言，比如一个 html 文件， 
  如果被修改了，其 Etag 也会别修改， 所以，ETag 的作用跟 Last-Modified 的 
  作用差不多，主要供 WEB 服务器 判断一个对象是否改变了。 
  比如前一次请求某个 html 文件时，获得了其 ETag，当这次又请求这个文件时， 
  浏览器就会把先前获得的 ETag 值发送给 WEB 服务器，然后 WEB 服务器 
  会把这个 ETag 跟该文件的当前 ETag 进行对比，然后就知道这个文件 
  有没有改变了。 

  \10. Expired：WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在 
  跟WEB服务器验证了其有效性后，才能用来响应客户请求。 
  是 HTTP/1.0 的头部。 
  例如：Expires：Sat, 23 May 2009 10:02:12 GMT 

  \11. Host：客户端指定自己想访问的WEB服务器的域名/IP 地址和端口号。 
  例如：Host：rss.sina.com.cn 

  \12. If-Match：如果对象的 ETag 没有改变，其实也就意味著对象没有改变，才执行请求的动作。 
  If-None-Match：如果对象的 ETag 改变了，其实也就意味著对象也改变了，才执行请求的动作。 

  \13. If-Modified-Since：如果请求的对象在该头部指定的时间之后修改了，才执行请求 
  的动作（比如返回对象），否则返回代码304，告诉浏览器该对象 
  没有修改。 
  例如：If-Modified-Since：Thu, 10 Apr 2008 09:14:42 GMT 
  If-Unmodified-Since：如果请求的对象在该头部指定的时间之后没修改过，才执行 
  请求的动作（比如返回对象）。

  \14. If-Range：浏览器告诉 WEB 服务器，如果我请求的对象没有改变，就把我缺少的部分 
  给我，如果对象改变了，就把整个对象给我。 浏览器通过发送请求对象的 
  ETag 或者 自己所知道的最后修改时间给 WEB 服务器，让其判断对象是否 
  改变了。 
  总是跟 Range 头部一起使用。 

  \15. Last-Modified：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间， 
  动态页面的最后产生时间等等。 
  例如：Last-Modified：Tue, 06 May 2008 02:42:43 GMT 

  \16. Location：WEB 服务器告诉浏览器，试图访问的对象已经被移到别的位置了， 
  到该头部指定的位置去取。 
  例如：Location： 
  http://i0.sinaimg.cn/dy/deco/2008/0528/sinahome_0803_ws_005_text_0.gif 

  \17. Pramga：主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache。 
  例如：Pragma：no-cache 

  \18. Proxy-Authenticate： 代理服务器响应浏览器，要求其提供代理身份验证信息。 
  Proxy-Authorization：浏览器响应代理服务器的身份验证请求，提供自己的身份信息。 

  \19. Range：浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。 
  例如：Range: bytes=1173546- 

  \20. Referer：浏览器向 WEB 服务器表明自己是从哪个 网页/URL 获得/点击 当前请求中的网址/URL。 
  例如：Referer：http://www.sina.com/ 

  \21. Server: WEB 服务器表明自己是什么软件及版本等信息。 
  例如：Server：Apache/2.0.61 (Unix) 

  \22. User-Agent: 浏览器表明自己的身份（是哪种浏览器）。 
  例如：User-Agent：Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; 
  rv:1.8.1.14) Gecko/20080404 Firefox/2.0.0.14 

  \23. Transfer-Encoding: WEB 服务器表明自己对本响应消息体（不是消息体里面的对象） 
  作了怎样的编码，比如是否分块（chunked）。 
  例如：Transfer-Encoding: chunked 

  \24. Vary: WEB服务器用该头部的内容告诉 Cache 服务器，在什么条件下才能用本响应 
  所返回的对象响应后续的请求。 
  假如源WEB服务器在接到第一个请求消息时，其响应消息的头部为： 
  Content-Encoding: gzip; Vary: Content-Encoding 那么 Cache 服务器会分析后续 
  请求消息的头部，检查其 Accept-Encoding，是否跟先前响应的 Vary 头部值 
  一致，即是否使用相同的内容编码方法，这样就可以防止 Cache 服务器用自己 
  Cache 里面压缩后的实体响应给不具备解压能力的浏览器。 
  例如：Vary：Accept-Encoding

  \25. Via： 列出从客户端到 OCS 或者相反方向的响应经过了哪些代理服务器，他们用 
  什么协议（和版本）发送的请求。 
  当客户端请求到达第一个代理服务器时，该服务器会在自己发出的请求里面 
  添加 Via 头部，并填上自己的相关信息，当下一个代理服务器 收到第一个代理 
  服务器的请求时，会在自己发出的请求里面复制前一个代理服务器的请求的Via 
  头部，并把自己的相关信息加到后面， 以此类推，当 OCS 收到最后一个代理服 
  务器的请求时，检查 Via 头部，就知道该请求所经过的路由。 
  例如：Via：1.0 236-81.D07071953.sina.com.cn:80 (squid/2.6.STABLE13) 

  ==================================== 

  HTTP 请求消息头部实例： 
  Host：rss.sina.com.cn 
  User-Agent：Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.8.1.14) Gecko/20080404 Firefox/2.0.0.14 
  Accept：text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5 
  Accept-Language：zh-cn,zh;q=0.5 
  Accept-Encoding：gzip,deflate 
  Accept-Charset：gb2312,utf-8;q=0.7,*;q=0.7 
  Keep-Alive：300 
  Connection：keep-alive 
  Cookie：userId=C5bYpXrimdmsiQmsBPnE1Vn8ZQmdWSm3WRlEB3vRwTnRtW <-- Cookie 
  If-Modified-Since：Sun, 01 Jun 2008 12:05:30 GMT 
  Cache-Control：max-age=0 
  HTTP 响应消息头部实例： 
  Status：OK - 200 <-- 响应状态码，表示 web 服务器处理的结果。 
  Date：Sun, 01 Jun 2008 12:35:47 GMT 
  Server：Apache/2.0.61 (Unix) 
  Last-Modified：Sun, 01 Jun 2008 12:35:30 GMT 
  Accept-Ranges：bytes 
  Content-Length：18616 
  Cache-Control：max-age=120 
  Expires：Sun, 01 Jun 2008 12:37:47 GMT 
  Content-Type：application/xml 
  Age：2 
  X-Cache：HIT from 236-41.D07071951.sina.com.cn <-- 反向代理服务器使用的 HTTP 头部 
  Via：1.0 236-41.D07071951.sina.com.cn:80 (squid/2.6.STABLE13) 

  Connection：close

  HTTP头部信息简单说明 

  一、HTTP响应码响应码由三位十进制数字组成，它们出现在由HTTP服务器发送的响应的第一行。 
  响应码分五种类型，由它们的第一位数字表示： 
  1xx：信息，请求收到，继续处理 
  2xx：成功，行为被成功地接受、理解和采纳 
  3xx：重定向，为了完成请求，必须进一步执行的动作 
  4xx：客户端错误，请求包含语法错误或者请求无法实现 
  5xx：服务器错误，服务器不能实现一种明显无效的请求 
  下表显示每个响应码及其含义： 
  100 继续101 分组交换协200 OK201 被创建202 被采纳203 非授权信息204 无内容205 重置内容206 部分内容300 多选项301 永久地传送302 找到303 参见其他304 未改动305 使用代理307 暂时重定向400 错误请求401 未授权402 要求付费403 禁止404 未找到405 不允许的方法406 不被采纳407 要求代理授权408 请求超时409 冲突410 过期的411 要求的长度412 前提不成立413 请求实例太大414 请求URI太大415 不支持的媒体类型416 无法满足的请求范围417 失败的预期500 内部服务器错误501 未被使用502 网关错误503 不可用的服务504 网关超时505 HTTP版本未被支持 

  二、HTTP头标头标由主键/值对组成。它们描述客户端或者服务器的属性、被传输的资源以及应该实现连接。 
  四种不同类型的头标： 
  1.通用头标：即可用于请求，也可用于响应，是作为一个整体而不是特定资源与事务相关联。 
  2.请求头标：允许客户端传递关于自身的信息和希望的响应形式。 
  3.响应头标：服务器和于传递自身信息的响应。 
  4.实体头标：定义被传送资源的信息。即可用于请求，也可用于响应。 
  头标格式：<name>:<value><CRLF> 
  下表描述在HTTP/1.1中用到的头标 
  Accept 定义客户端可以处理的媒体类型，按优先级排序；在一个以逗号为分隔的列表中，可以定义多种类型和使用通配符。例如：Accept: image/jpeg,image/png,*/*Accept-Charset 定义客户端可以处理的字符集，按优先级排序；在一个以逗号为分隔的列表中，可以定义多种类型和使用通配符。例如：Accept-Charset: iso-8859-1,*,utf-8 
  Accept-Encoding 定义客户端可以理解的编码机制。例如：Accept-Encoding:gzip,compress 
  Accept-Language 定义客户端乐于接受的自然语言列表。例如：Accept-Language: en,de 
  Accept-Ranges 一个响应头标，它允许服务器指明：将在给定的偏移和长度处，为资源组成部分的接受请求。该头标的值被理解为请求范围的度量单位。例如Accept-Ranges: bytes或Accept-Ranges: none 
  Age 允许服务器规定自服务器生成该响应以来所经过的时间长度，以秒为单位。该头标主要用于缓存响应。例如：Age: 30 
  Allow 一个响应头标，它定义一个由位于请求URI中的次源所支持的HTTP方法列表。例如：Allow: GET,PUT 
  aUTHORIZATION 一个响应头标，用于定义访问一种资源所必需的授权（域和被编码的用户ID与口令）。例如：Authorization: Basic YXV0aG9yOnBoaWw= 
  Cache-Control 一个用于定义缓存指令的通用头标。例如：Cache-Control: max-age=30 
  Connection 一个用于表明是否保存socket连接为开放的通用头标。例如：Connection: close或Connection: keep-alive 
  Content-Base 一种定义基本URI的实体头标，为了在实体范围内解析相对URLs。如果没有定义Content-Base头标解析相对URLs，使用Content- Location URI（存在且绝对）或使用URI请求。例如：Content-Base: 

  Content-Encoding 一种介质类型修饰符，标明一个实体是如何编码的。例如：Content-Encoding: zipContent-Language 用于指定在输入流中数据的自然语言类型。例如：Content-Language: en 
  Content-Length 指定包含于请求或响应中数据的字节长度。例如：Content-Length:382 
  Content-Location 指定包含于请求或响应中的资源定位（URI）。如果是一绝。对URL它也作为被解析实体的相对URL的出发点。例如：Content-Location: http://www.myweb.com/news 
  Content-MD5 实体的一种MD5摘要，用作校验和。发送方和接受方都计算MD5摘要，接受方将其计算的值与此头标中传递的值进行比较。例如：Content-MD5: <base64 of 128 MD5 digest> 
  Content-Range 随部分实体一同发送；标明被插入字节的低位与高位字节偏移，也标明此实体的总长度。例如：Content-Range: 1001-2000/5000 
  Contern-Type 标明发送或者接收的实体的MIME类型。例如：Content-Type: text/html 
  Date 发送HTTP消息的日期。例如：Date: Mon,10PR 18:42:51 GMT 
  ETag 一种实体头标，它向被发送的资源分派一个唯一的标识符。对于可以使用多种URL请求的资源，ETag可以用于确定实际被发送的资源是否为同一资源。例如：ETag: '208f-419e-30f8dc99' 
  Expires 指定实体的有效期。例如：Expires: Mon,05 Dec 2008 12:00:00 GMT 
  Form 一种请求头标，给定控制用户代理的人工用户的电子邮件地址。例如：From: webmaster@myweb.com 
  Host 被请求资源的主机名。对于使用HTTP/1.1的请求而言，此域是强制性的。例如：Host: www.myweb.com 
  If-Modified-Since 如果包含了GET请求，导致该请求条件性地依赖于资源上次修改日期。如果出现了此头标，并且自指定日期以来，此资源已被修改，应该反回一个304响应代码。例如：If-Modified-Since: Mon,10PR 18:42:51 GMT 
  If-Match 如果包含于一个请求，指定一个或者多个实体标记。只发送其ETag与列表中标记区配的资源。例如：If-Match: '208f-419e-308dc99' 
  If-None-Match 如果包含一个请求，指定一个或者多个实体标记。资源的ETag不与列表中的任何一个条件匹配，操作才执行。例如：If-None-Match: '208f-419e-308dc99' 
  If-Range 指定资源的一个实体标记，客户端已经拥有此资源的一个拷贝。必须与Range头标一同使用。如果此实体自上次被客户端检索以来，还不曾修改过，那么服务器只发送指定的范围，否则它将发送整个资源。例如：Range: byte=0-499<CRLF>If-Range:'208f-419e-30f8dc99' 
  If-Unmodified-Since 只有自指定的日期以来，被请求的实体还不曾被修改过，才会返回此实体。例如：If-Unmodified-Since:Mon,10PR 18:42:51 GMT 
  Last-Modified 指定被请求资源上次被修改的日期和时间。例如：Last-Modified: Mon,10PR 18:42:51 GMT 
  Location 对于一个已经移动的资源，用于重定向请求者至另一个位置。与状态编码302（暂时移动）或者301（永久性移动）配合使用。例如：Location: http://www2.myweb.com/index.jsp 
  Max-Forwards 一个用于TRACE方法的请求头标，以指定代理或网关的最大数目，该请求通过网关才得以路由。在通过请求传递之前，代理或网关应该减少此数目。例如：Max-Forwards: 3 
  Pragma 一个通用头标，它发送实现相关的信息。例如：Pragma: no-cache 
  Proxy-Authenticate 类似于WWW-Authenticate，便是有意请求只来自请求链（代理）的下一个服务器的认证。例如：Proxy-Authenticate: Basic realm-admin 
  Proxy-Proxy-Authorization 类似于授权，但并非有意传递任何比在即时服务器链中更进一步的内容。例如：Proxy-Proxy-Authorization: Basic YXV0aG9yOnBoaWw= 
  Public 列表显示服务器所支持的方法集。例如：Public: OPTIONS,MGET,MHEAD,GET,HEAD 
  Range 指定一种度量单位和一个部分被请求资源的偏移范围。例如：Range: bytes=206-5513 
  Refener 一种请求头标域，标明产生请求的初始资源。对于HTML表单，它包含此表单的Web页面的地址。例如：Refener: http://www.myweb.com/news/search.html 
  Retry-After 一种响应头标域，由服务器与状态编码503（无法提供服务）配合发送，以标明再次请求之前应该等待多长时间。此时间即可以是一种日期，也可以是一种秒单位。例如：Retry-After: 18 
  Server 一种标明Web服务器软件及其版本号的头标。例如：Server: Apache/2.0.46(Win32) 
  Transfer-Encoding 一种通用头标，标明对应被接受方反向的消息体实施变换的类型。例如：Transfer-Encoding: chunked 
  Upgrade 允许服务器指定一种新的协议或者新的协议版本，与响应编码101（切换协议）配合使用。例如：Upgrade: HTTP/2.0 
  User-Agent 定义用于产生请求的软件类型（典型的如Web浏览器）。例如：User-Agent: Mozilla/4.0(compatible; MSIE 5.5; Windows NT; DigExt) 
  Vary 一个响应头标，用于表示使用服务器驱动的协商从可用的响应表示中选择响应实体。例如：Vary: *Via 一个包含所有中间主机和协议的通用头标，用于满足请求。例如：Via: 1.0 fred.com, 1.1 wilma.com 
  Warning 用于提供关于响应状态补充信息的响应头标。例如：Warning: 99 www.myweb.com Piano needs tuning 
  www-Authenticate 一个提示用户代理提供用户名和口令的响应头标，与状态编码401（未授权）配合使用。响应一个授权头标。例如：www-Authenticate: Basic realm=zxm.mgmt

  参考链接：https://www.cnblogs.com/jiangxiaobo/p/5499488.html





