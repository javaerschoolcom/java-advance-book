# Tomcat服务器

### 下载安装访问

*  [下载地址](http://tomcat.apache.org/)
*  安装tomcat前必须安装JDK，需要配置tomcat环境变量

#####  tomcat环境变量配置

``` java
CATALINA_HOME=D:\apache-tomcat-7.0.29
path=%CATALINA_HOME%\bin;
```

#####tomcat常用命令

``` jav a
启动：startup
关闭: shutdowm
```

##### tomcat访问

``` java
http://localhost:8080/
```

### tomcat目录结构

``` java
bin:tomcat一些执行的命令
conf:tomcat服务器配置信息
lib:tomcat库文件
logs:tomcat运行日志信息
temp：临时文件
webapps:项目发布区域
work:工作信息
```

### 如何创建通过web服务器访问的项目

> 标准的项目结构如下

``` java
项目名称
    ---html
    ---css
    ---img
    ---js
       ----WEB-INF// （单词不能该，大小写不能该）该路径下面的文件不能通过浏览器直接访问
    		 ----classes// (照写) 放的是编译后的java文件
    		 ----lib //存放第三方开发的jar文件
    		 ----web.xml //配置web项目文件
    		 ----其他文件
```

### 如何发布项目到服务器

> 常用的有三种发布的方式：比如创建了项目myServer

###### 第一种

> 直接把项目扔到webapps下面去，一定要重启服务器

* 访问方式：http://localhost:8080/myServer/资源地址
* 特点：修改文件必须重启&&项目放在了服务器中

###### 第二种

> 修改tomcat的conf目录下的server.xml文件如下：

``` xml
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log." suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
        <Context path="/lero"  docBase="D:\myServer"  />
</Host>
```

* 访问方式：http://localhost:8080/lero/资源地址
* 修改过后还是要重启&&项目和服务进行了分离

第三种

> 在tomcat>conf>Catalina>localhost新增一个demo.xml文件如下：

``` xml
<Context docBase="D:\myServer"  />
```

* 访问方式：http://localhost:8080/demo/资源地址
* 如果demo.xml重新命名了，不需要重启服务器就能直接访问

### 常见服务器错误

> 请求一个资源，服务器会返回一个对应的请求结果状态的码值

| 错误码  | 解释                                                    |
| ------- | ------------------------------------------------------- |
| 100-199 | 一个请求发送后，需要后续的请求才能完成一个完整请求      |
| 200-299 | 请求成功的一个返回码                                    |
| 300-399 | 请求细分,需要进一步处理，比如请求转发 302 304 307       |
| 400-499 | 客户端出错  404（请求的路径有错误->单词错误，路径写错） |
| 500-599 | 服务器出错了 500                                        |

> 常见错误代码

``` java
一：1xx （临时响应）表示临时响应并需要请求者继续执行操作的状态代码。
100 （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。
101 （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。
二：2xx （成功）表示成功处理了请求的状态代码。
200 （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
201 （已创建） 请求成功并且服务器创建了新的资源。
202 （已接受） 服务器已接受请求，但尚未处理。
203 （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。
204 （无内容） 服务器成功处理了请求，但没有返回任何内容。
205 （重置内容） 服务器成功处理了请求，但没有返回任何内容。
206 （部分内容） 服务器成功处理了部分 GET 请求。
三：3xx （重定向） 表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。
300 （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (useragent)选择一项操作，或提供操作列表供请求者选择。
301 （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或HEAD请求的响应）时，会自动将请求者转到新位置。
302 （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
303 （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
304 （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
305 （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
307 （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
四：4xx （请求错误） 这些状态代码表示请求可能出错，妨碍了服务器的处理。
400 （错误请求） 服务器不理解请求的语法。
401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
403 （禁止） 服务器拒绝请求。404 （未找到） 服务器找不到请求的网页。
404  Not Found  无法找到指定位置的资源。这也是一个常用的应答
405 （方法禁用） 禁用请求中指定的方法。
406 （不接受） 无法使用请求的内容特性响应请求的网页。
407 （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。408 （请求超时）服务器等候请求时发生超时。
409 （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。
410 （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。
411 （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。
412 （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。
413 （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。
414 （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。
415 （不支持的媒体类型） 请求的格式不受请求页面的支持。
416 （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。
417 （未满足期望值） 服务器未满足”期望”请求标头字段的要求。
五：5xx （服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。
500 （服务器内部错误） 服务器遇到错误，无法完成请求。
501 （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
502 （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
503 （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
504 （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。
505 （HTTP 版本不受支持）服务器不支持请求中所用的 HTTP 协议版本。

```



### 如何修改端口

> 在tomcat的server.xml做如下修改

``` java
 <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

### 如何访问项目的时候不加项目名

> 在tomcat的server.xml中做如下修改

``` java
<!--Context中的path为空自符串 -->
<Context path=""  docBase="D:\myServer"  />

```

### 如何设置项目发布的域名

1. 需要增加一个主机配置

   ``` java
    <Host name="www.lero.com"  appBase="D:\temp"
               unpackWARs="true" autoDeploy="true">
        <Context path=""  docBase="D:\temp\myServer"  />
   </Host>
   ```

2. 需要在本机文件(C:\Windows\System32\drivers\etc)下的hosts文件中添加如下内容

 ``` java
127.0.0.1   www.lero.com
 ```

3. 在浏览器中访问该项目

``` java
http://www.lero.com/
```

### 如何打包项目

> 开发后的项目部署上线，需要对项目进行打包，打包的目的是为了构建成标准的web项目，而且会编译java源文件

* 在CMD中使用如下命令

``` java
//假设需要打包的项目所在路径D：myServer
//切换到目标路径 d:mySerer，然后输入如下打包命令
//说明：myServer.war 打包后的文件
//myServer 源文件
jar -cvf  myServer.war  myServer
```

* 对打包后的项目进行部署

### **浏览器与服务器交互过程**

``` java
1）浏览器输入网站：http://www.demo.com/javaWebDemo/1.jsp
2）查询主机名www.demo.com对应的ip（1是找本地window下的hosts文件配置，2是找远程DNS服务器匹配ip）
3）根据查询到的ip连接web服务器
4）向服务器发送http请求
5）服务器从请求中解析出客户机访问的主机名
6）服务器从请求中解析出客户机访问的web应用/或者web应用中要访问的web资源
7）以流的响应给客户端浏览器中
```

### Tomcat体系结构

``` java
<?xml version='1.0' encoding='utf-8'?>
<Server port="8005" shutdown="SHUTDOWN">
  <Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS"
               keystoreFile="conf/.keystore" keystorePass="123456"/>
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <Engine name="Catalina" defaultHost="localhost">

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log." suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
      <Host name="www.gacl.cn" appBase="F:\JavaWebApps">
        <Context path="" docBase="F:\JavaWebApps\JavaWebDemo1"/>
      </Host>

    </Engine>
  </Service>
</Server>
```

> 分析

``` java
    Tomcat服务器的启动是基于一个server.xml文件的，Tomcat启动的时候首先会启动一个Server，Server里面就会启动Service，Service里面就会启动多个"Connector(连接器)"，每一个连接器都在等待客户机的连接，当有用户使用浏览器去访问服务器上面的web资源时，首先是连接到Connector(连接器)，Connector(连接器)是不处理用户的请求的，而是将用户的请求交给一个Engine(引擎)去处理，Engine(引擎)接收到请求后就会解析用户想要访问的Host，然后将请求交给相应的Host，Host收到请求后就会解析出用户想要访问这个Host下面的哪一个Web应用,一个web应用对应一个Context。
```













