# Servlet技术

### 什么是Servlet

> Servlet是Sun公司开发一种javaweb开发的技术标准

### 第一个Servlet开发

1.  创建一个javaweb项目
2.  自定义类实现Servlet接口
3.  注册Servlet
4. 启动服务器访问该Servlet

### Servlet执行流程

1. init()方法只会执行一次，而且是在访问该Servlet的时候才会创建Servelt实例
2. service()方法每次请求都会被调用
3. destroy()方法是服务器关闭的时候调用

### Servlet映射配置

* *.do
* /xx/*   优先级最高
* /*        优先级高于*.do
* /          匹配优先级最低

### 创建Servelt方式

* 自定义类实现Servlet接口
* 继承HttpServlet

### ServletConfig

> Servlet的配置信息封装成该对象

##### 常用方法

```java
 String  servletConfig.getInitParameter("charEncoding")//获取web.xml中servlet对应的配置的value

 Enumeration<String>   servletConfig.getInitParameterNames() //获取web.xml中servlet中所有的key

 String servletConfig.getServletName()//获取servlet对应的名称

 ServletContext servletConfig.getServletContext();//获取当前的web上下文

```

### ServletContext

> 当前运行的web上下文环境

##### 常用方法

``` java
 String  servletContext.getInitParameter("charEncoding")//获取web.xml中全局配置的value

 Enumeration<String>   servletContext.getInitParameterNames() //获取web.xml中servlet中所有的key

 //以下四组方法常用于Servlet之间通信，传递数据
 servletContext.setAttribute("name","lero");
 servletContext.getAttribute("name");
 servletContext.removeAttribute("name");
 servletContext.setAttribute("name","xxx");
 servletContext.getAttributeNames();

 servletContext.getClassLoader(); //获取类的装载字节码
 servletContext.getContextPath(); //javaweb  获取项目的根路径
 servletContext.getRealPath("/"); //获取项目的绝对路径
 servletContext.getRequestDispatcher();//请求转发 302
 servletContext.getResourceAsStream(); //获取输入流
```

### HttpServletRequst

> 每一次请求都是一个新的HttpServletRequst对象

##### 常用方法

### HttpServletResponse

> 每一次请求都是一个新的HttpServletResponse对象

##### 常用方法





