# Cookie&Session

### 会话

> 指的是从打开一个浏览器，访问一个网站，并且随意访问该站点的任意资源，直到关闭浏览器的整个过程称为一次会话

### 会话需求

> 在会话中需要操作一些数据，而Http协议有个特点：无状态性，会话可能需要让服务器识别你的身份

* 记住我的功能
* 上次登录时间
* 访问历史记录

### Servlet中会话解决方案

* Cookie技术

  >1.当客户端第一次请问服务端的时候，服务端生成一个cookie并且响应给客户端
  >
  >2.客户端第二次或者第N次访问同一个服务端的时候，客户端带上服务端第一次发送到cookie
  >
  >3.Cookie一个浏览器最多可以存300个，每一个站点至多存20个，每一个cookie最大4KB
  >
  >4.Cookie默认的有效期是一次会话，若需要延长Cookie存储时间，设置setMaxAge后，关闭浏览器会写入硬盘中 ​


### Cookie常用方法

``` java
Cookie(String name, String value) //创建一个Cookie,cookie的值大小4KB
cookie.setMaxAge(int expiry)  //设置cookie有效期
getName() //获取cookie中存储的名字
getValue() //获取cookie存储的值
setPath(String uri) //在访问uri指定路径的时候，会带上该路径上设置过的cookie  eg:localhost:8080/javaweb/x/y
request.getCookies() //获取客户端请求头中的cookie，
response.addCookie(cookie); //把cookie写入响应头
```



### Session

> 保持会话的技术

> 应用场景： 登录后，刚网站后续访问都能保持会话

### Session和Cookie区别

1. cookie是存在客户端的，session是存在服务端的
2. cookie只能存字符串，session可以存Object
3. 不重要的数据采用cookie,sesssion相对安全，存储比较重要的数据
4. 提高服务器性能，尽量使用cookie
5. 同一个浏览器只有一个session
6. cookie生命周期可能在浏览器关闭后还存在，session只存在于浏览器开启之内

### Session常用方法

``` java
HttpSession session=requset.getSession() //获取session，第一次没有session会创建，如果有则获取
session.isNew() //判断session是否是新创建的，true是|false否
session.setAttribute(String key,Object obj) //添加数据到session
session.removeAttribute(String key) //删除指定的属性
session.getAttributeNames() //获取session中所有的key
session.invalidate() //让session失效
session.setMaxInactiveInterval(10) //设置session最大有效时间
```

### Cookie被禁用解决办法

>  URL重写解决Cookie被禁用

``` java
//常用于form或者a
String url = resp.encodeURL("/javaweb/persion"); //http://localhost/javaweb/persion;jsessionid=C0884F55B63238A856A4C277AE6CF11D

//常用于重定向 resp.sendRedirect()
String url = resp.encodeRedirectURL("/javaweb/persion")

```

### Session处理表单重复提交

* 网络延迟可能会造成表单重复提交
* 提交了表单，按了返回按钮后，又重新提交表单
* 提交了表单，按了刷新按钮

##### 解决办法：采用令牌机制

> 原理：生成一个令牌， 往session中存一份并且把该令牌往表单中存一份，然后提交表单，把session中的令牌和表单中的令牌匹配，匹配成功，则该次是第一次提交，否则不处理后续表单提交的数据，操作完毕清除session中令牌





