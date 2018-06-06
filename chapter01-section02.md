# HTTP协议

### 定义

> HTTP是hypertext transfer protocol（超文本传输协议）的简写，它是一个应用层协议，用于定义WEB浏览器与WEB服务器之间通信的过程，下层使用的TCP/IP协议。

### 特点

* 支持浏览器/服务器模式
* 简单快速：客户端向服务端请求服务时，只需要传递请求方法和路径。请求方法常有get，post.
* 灵活，HTTP允许传输任意类型的数据对象，正在传输的类型由Content-Type指定。
* 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
* 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

### 版本

* HTTP/1.0  一次请求就是一个连接，该连接中只能处理一次请求，请求完毕就断开连接
* HTTP/1.1 请求后建立连接，该连接中可以处理很多次请求与响应，也就是该连接是一个长连接

### HTTP请求

> 用户通过浏览器输入一个URL，然后回车发送，就是一次HTTP请求

* 完整的请求比如包含：请求行，请求报头，请求正文

* 请求行

  > 请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本

  ``` java
  GET /form.html HTTP/1.1 (CRLF) //CRLF表示回车和换行（除了作为结尾的CRLF外，不允许出现单独的CR或LF字符）。
  ```

* 请求报头

  > 请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息

  ``` java
  | 请求报名            | 含义                                                         |
  | ------------------- | ------------------------------------------------------------ |
  | Accept              | 指定客户端接受哪些类型的信息 eg：Accept：image/gif           |
  | Accept-Charset      | 用于指定客户端接受的字符集 eg：Accept-Charset:iso-8859-1,gb2312 |
  | Accept-Encoding     | 类似Accept,它是用于指定可接受的内容编码                      |
  | Accept-Language     | 类似于Accept，但是它是用于指定一种自然语言                   |
  | Host                | 指定被请求资源的Internet主机和端口号                         |
  | User-Agent          | 允许客户端将它的操作系统、浏览器和其它属性告诉服务器         |
  | Referer             | 这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中 |
  | Cookie              | 这个头信息把之前发送到浏览器的 cookies 返回到服务器          |
  | Content-Length      | 这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位） |
  | Connection          | 这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 Keep-Alive 意味着使用了持续连接 |
  | If-Modified-Since   | 这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 Not Modified 头信息 |
  | If-Unmodified-Since | 这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功 |
  | Authorization       | 这个头信息用于客户端在访问受密码保护的网页时识别自己的身份   |

  ```


  > 案例

  ``` java
  GET /form.html HTTP/1.1 (CRLF)
  Accept:image/gif,image/x-xbitmap,image/jpeg,application/x-shockwave-flash,application/vnd.ms-excel,application/vnd.ms-powerpoint,application/msword,*/* (CRLF)
  Accept-Language:zh-cn (CRLF)
  Accept-Encoding:gzip,deflate (CRLF)
  If-Modified-Since:Wed,05 Jan 2007 11:21:25 GMT (CRLF)
  If-None-Match:W/"80b1a4c018f3c41:8317" (CRLF)
  User-Agent:Mozilla/4.0(compatible;MSIE6.0;Windows NT 5.0) (CRLF)
  Host:www.guet.edu.cn (CRLF)
  Connection:Keep-Alive (CRLF)
  (CRLF)
  ```

  ​

### HTTP响应

> 客户端发送请求后，服务器对请求接收和处理后，最终通过浏览器进行回复

* 完成的响应包含：状态行，响应报头，响应正文

* 状态行

* 响应报头

  ``` java
  | 响应报头            | 含义                                                         |
  | ------------------- | ------------------------------------------------------------ |
  | Allow               | 这个头信息指定服务器支持的请求方法（GET、POST 等）           |
  | Cache-Control       | 这个头信息指定响应文档在何种情况下可以安全地缓存。可能的值有：public、private 或 no-cache 等。Public 意味着文档是可缓存，Private 意味着文档是单个用户私用文档，且只能存储在私有（非共享）缓存中，no-cache 意味着文档不应被缓存 |
  | Connection          | 这个头信息指示浏览器是否使用持久 HTTP 连接。值 close 指示浏览器不使用持久 HTTP 连接，值 keep-alive 意味着使用持久连接 |
  | Content-Disposition | 这个头信息可以让您请求浏览器要求用户以给定名称的文件把响应保存到磁盘 |
  | Content-Encoding    | 在传输过程中，这个头信息指定页面的编码方式                   |
  | Content-Language    | 这个头信息表示文档编写所使用的语言。例如，en、en-us、ru 等   |
  | Content-Length      | 这个头信息指示响应中的字节数。只有当浏览器使用持久（keep-alive）HTTP 连接时才需要这些信息 |
  | Content-Type        | 这个头信息提供了响应文档的 MIME（Multipurpose Internet Mail Extension）类型 |
  | Expires             | 这个头信息指定内容过期的时间，在这之后内容不再被缓存，-1为控制浏览器不缓存 |
  | Last-Modified       | 这个头信息指示文档的最后修改时间。然后，客户端可以缓存文件，并在以后的请求中通过 If-Modified-Since 请求头信息提供一个日期 |
  | Location            | 这个头信息应被包含在所有的带有状态码的响应中。在 300s 内，这会通知浏览器文档的地址。浏览器会自动重新连接到这个位置，并获取新的文档 |
  | Refresh             | 这个头信息指定浏览器应该如何尽快请求更新的页面。您可以指定页面刷新的秒数 |
  | Retry-After         | 这个头信息可以与 503（Service Unavailable 服务不可用）响应配合使用，这会告诉客户端多久就可以重复它的请求 |
  | Set-Cookie          | 这个头信息指定一个与页面关联的 cookie                        |
  | Server              | 服务器通过这个头，告诉浏览器服务器的型号                     |
  | Pragma              | no-cache                                                     |
  ```

  ### HTTP工作原理

  ###### 1. 客户端连接到Web服务器

  一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如http://www.lero.com

  ###### 2.发送HTTP请求

  通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成。

  ###### 3.服务器接受请求并返回HTTP响应

  Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。

  ###### 4.释放连接TCP连接

  若connection 模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;

  ###### 5、客户端浏览器解析HTML内容

  客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

  例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

  1、浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;

  2、解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立TCP连接;

  3、浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 [TCP 三次握手](http://www.jianshu.com/p/ef892323e68f)的第三个报文的数据发送给服务器;

  4、服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;

  5、释放 TCP连接;

  6、浏览器将该 html 文本并显示内容; 　　

  ### HTTP,UDP,TCP简介

  ##### HTTP

  ​	http协议是超文本传输协议(HyperText Transfer Protocol)的简称,是网络上最为广泛的一种网络协议。所有的WWW文件都遵循这种标准。

  ​	1960年美国人Ted Nelson构思了一种通过计算机处理文本信息的方法，并称之为超文本（hypertext）,这成为了HTTP超文本传输协议标准架构的发展根基。Ted Nelson组织协调万维网协会（World Wide WebConsortium）和互联网工程工作小组（Internet Engineering Task Force ）共同合作研究，最终发布了一系列的RFC，其中著名的RFC 2616定义了HTTP1.1。其中RFC(Request For Comments)-意即“请求评议”，包含了关于Internet的几乎所有重要的文字资料。

  #####  UDP

  ​	 UDP 是User Datagram Protocol的简称，中文名是用户数据报协议。和TCP同属于一层。用于处理数据包，是一种无连接的协议。主要作用是将网络数据流量压缩成数据包的形式。一个典型的数据包就是一个二进制数据的传输单位。每一个数据包的前8个字节用来包含报头信息，剩余字节则用来包含具体的传输数据。

  * 优点：它不属于连接型协议，因而具有资源消耗小，处理速度快的优点，所以通常音频、视频和普通数据

  * 缺点：没有可靠性保证、顺序保证和流量控制字段等，可靠性较差。

    ### TCP

     	TCP（Transmission Control Protocol 传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议。

  * 优点：传送数据会有三次握手建立连接，数据传输时有确认，重传，拥塞控制机制，在数据传输完成后，会断开连接节约系统资源。

  * 缺点：慢，效率低，占用资源多，容易被攻击。

  ​

  ### URI,URl,URN联系与区别

  >  URI: Uniform Resource Identifier (URI) 是一个紧凑的字符串用来标示抽象或物理资源

  > URL:Uniform Resource Locator (URL) 是URI的子集, 除了确定一个资源,还提供一种定位该资源的主要访问机制(如其网络“位置”)

  ​    基本URL包含:**模式（或称协议）+服务器名称（或IP地址）+路径和文件名**

  1. eg:http://www.baidu.com/index.jsp

     其中模式/协议（scheme）又有如下：

     http——超文本传输协议资源

     https——用安全套接字层传送的超文本传输协议

     ftp——文件传输协议

     mailto——电子邮件地址

     ldap——轻型目录访问协议搜索

     file——当地电脑或网上分享的文件

     news——Usenet新闻组

     gopher——Gopher协议

     telnet——Telnet协议

  2. 服务器的名称或IP地址:它也可以包含接触服务器必须的用户名称和密码,后面有时还跟一个冒号和一个端口号

  3. 路径是等级结构的定义，一般来说不同部分之间以斜线（/）分隔，有时候还会带参数

  >URN:Uniform Resource Name(URN)是URI的子集，统一资源名称,唯一标识一个实体的标识符，但是不给出实体的位置

  ​     URI可以分为URL,URN或同时具备locators 和names特性的一个东西。URN作用就好像一个人的名字，URL就像一个人的地址。换句话说：URN确定了东西的身份，URL提供了找到它的方式。

  #####  结论

  * URL是URI的一种
  * 让URI成为URI的是它拥有“访问机制”，“网络位置”
  * URN是唯一标识的一部分，就是一个特殊的名字
  * URI包含URL和URN

  下面就来看看来自权威的RFC案列：

  ``` java
  ftp://ftp.is.co.za/rfc/rfc1808.txt(also a URL because of the protocol)

  http://www.ietf.org/rfc/rfc2396.txt(also a URL because of the protocol)

  ldap://[2001:db8::7]/c=GB?objectClass?one(also a URL because of the  protocol)

  mailto:John.Doe@example.com(also a URL because of the protocol)

  news:comp.infosystems.www.servers.unix(also a URL because of the protocol)

  tel:+1-816-555-1212

  telnet://192.0.2.16:80/(also a URL because of the protocol)

  urn:oasis:names:specification:docbook:dtd:xml:4.1.2

  ```

  ​

  ​


