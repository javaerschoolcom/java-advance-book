# 过滤器

### 过滤器概述

* 过滤器是Servlet中的标准，它又不是标准的Servlet
* 不能够直接处理请求或者响应，但是可以对请求预处理或者对响应后处理或者直接阻塞请求

### 过滤器特点

* 发起一个请求，过滤器捕捉请求，对请求预处理，对响应预处理，再分发到某个具体的Servlet
* Servlet执行完毕后，会再次进入该过滤器，对响应进行后处理
* 如果不满足过滤条件，可以强制在过滤器中断请求，就不会再去某个具体的Servlet

### 过滤器的运用

* 处理乱码情况
* 过滤敏感关键字
* 实现权限控制
* 实现动态页面静态化

### 过滤器的开发

* 实现Filter接口，就会实现init(),doFilter(),destory()
* 在doFilter()写核心业务逻辑，如果需要放行，必须调用filterChain.doFilter(req,resp);
* 在web.xml做过滤器注册

### 过滤器的生命周期

##### 加载和实例化

* Web容器启动时，即会根据 web.xml中声明的 filter顺序依次实例化这些filter,并且是单例的

##### 初始化

* Web容器调用过滤器的 init(FilterConfig)方法来初始化过滤器。利用 方法参数FilterConfig对象可以得到ServletContext对象，以及在 web.xml 中配置的过滤器的初始化参数
* ps:实例化和初始化的操作只会在容器启动时执行，而且只会执行一次

##### 核心方法dofilter

* 过滤器中的doFilter方法类似于Servlet接口的 service方法。当客户端请求目标资源的时候，容器会筛选出符合filter-mapping中的 url-pattern的 filter，并按照声明 filter-mapping的顺序依次调用这些filter的doFilter方法在这个链式调用过程中可以调用 chain.doFilter(ServletRequest, ServletResponse)将请求传给下一个过滤器(或目标资源)，也可以直接向客户端返回响应信息，或者利用 RequestDispatcher的forward和 include方法，以及 HttpServletResponse的 sendRedirect方法将请求转向到其它资源。
* ps:每一次请求时都只会调用方法doFilter()进行处理

##### 销毁

* Web容器停止服务器时调用过滤器的destroy方法，销毁实例，可以在该方法里释放过滤器使用的资源

### 过滤器链

* 一个请求需要多个过滤器处理，可以让请求依次穿过一系列过滤，这个一系列过滤就叫做过滤器链
* 过滤链的执行顺序，就是在web.xml中的注册顺序
* 一个请求被过滤器链处理的时候，上一个过滤器执行完毕后，会继续把请求依次传递给下一个过滤器

### 过滤器配置

##### filter节点配置

* filter:用于注册filter，filter节点在web.xml可以有多个
* filter-name：指定filter的名字(可随便命名)
* filter-class：指定filter类的全路径名，不要后缀.java
* init-param:用于配置filter的初始化参数，在filter节点中可以有多个，每一个节点中包含param-name和param-value。并且可以在filer的init方法中获取到。

##### filter-mapping节点配置

* filter-mapping:用于指定filter的匹配命中规则，在web.xml可以有多个
* filter-mapping节点中的filer-name：指定使用注册的filter
* url-pattern:指定filter如何通过请求URL命中规则

``` java
/*      命中所有的URL eg:localhost/lero/testFilter.do
/lero/* 命中部分的URL eg:localhost/lero/testFilter.do|localhost/lero/demo.action|
*.jsp   命中后缀是.jsp的URL eg:localhost/login.jsp|localhost/lero/index.jsp
*.html  命中后缀是.html的URL eg:localhost/login.html|localhost/lero/index.html
```

* filter-mapping节点中的servlet-name:用于指定过滤器对某一个或者多个servlet命中
* filter-mapping节点中的dispatcher:用于指定过滤器更详细的命中规则，可以在一个filter-mapping中添加多个dispatcher：该节点有如下常用值:

``` java
* 直接请求(REQUEST)：通过浏览器访问的请求，包括重定向
* 请求转发(FORWARD)：req.getRequestDispatcher("/index.jsp").forward(req,resp)
* 请求包含(INCLUDE)：req.getRequestDispatcer("/index.jsp").include(req,resp)
* 请求出错(ERROR)：web.xml中有<error-page>的配置项
```

### 过滤器高级

* Servlet API 中提供了一个request对象的装饰设计模式的默认实现类HttpServletRequestWrapper，HttpServletRequestWrapper 类实现了request 接口中的所有方法，可以自定义一个类继承自该类，实现对request的增强

> 处理get请求乱码

``` java
public class MyRequestWaper extends HttpServletRequestWrapper {
    private  HttpServletRequest request ;
    public MyRequestWaper(HttpServletRequest request) {
        super(request);
        this.request=request;
    }
    @Override
    public String getParameter(String name) {
        String value = request.getParameter(name);
        System.out.println(value);
        if(value==null){
            return null;
        }
        if(request.getMethod().equalsIgnoreCase("get")){
            try {
                return  new String(value.getBytes("ISO8859-1"), "utf-8");
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        }else{
            return  value;
        }
        return  null;
    }
}
```

> 过滤器

``` java
public class MyFilter implements Filter {
    private FilterConfig config;
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.config=filterConfig;
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest  request =(HttpServletRequest)servletRequest;
        HttpServletResponse response =(HttpServletResponse)servletResponse;
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        //装饰模式，更换标准的request，偷天换日
        MyRequestWaper requestWaper = new MyRequestWaper(request);
        filterChain.doFilter(requestWaper,response);
    @Override
    public void destroy() {
        config=null;
        System.out.println("destroy");

    }
}
```

* Servlet API 中提供了一个response对象的装饰设计模式的默认实现类HttpServletResponseWrapper，HttpServletResponseWrapper 类实现了response接口中的所有方法，可以自定义一个类继承自该类，实现对response的增强

> 动态网页静态化

``` java
public class MyResponseWaper extends HttpServletResponseWrapper {
    PrintWriter writer;
    public MyResponseWaper(HttpServletResponse response,File file) {
        super(response);
        try {
            writer = new PrintWriter(file);
        }catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
    @Override
    public PrintWriter getWriter() throws IOException {
       //默认是实现类是写到浏览器？写文件夹中
        return this.writer;
    }
}
```

> 过滤器

``` java
public class MyFilter implements Filter {
    private FilterConfig config;
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.config=filterConfig;
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest  request =(HttpServletRequest)servletRequest;
        HttpServletResponse response =(HttpServletResponse)servletResponse;
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");
        //动态网页静态化，首页|新闻|文章
        MyRequestWaper requestWaper = new MyRequestWaper(request);
        String id = requestWaper.getParameter("id"); //1.html|2.html
        String basePath = config.getServletContext().getRealPath("/static");                             //d:xx/static/1.html
        File file = new File(basePath);
        if(!file.exists()&&!file.isDirectory()){
            file.mkdirs();
        }
        String  fileName =id+".html";
        File file1 = new File(file, fileName);

        if(!file1.exists()){
            file1.createNewFile();
        }else{
         response.sendRedirect(config.getServletContext().getContextPath()+"/static/"+fileName);
            return;
        }
        MyResponseWaper myResponseWaper = new MyResponseWaper(response, file1);
        filterChain.doFilter(requestWaper,myResponseWaper);
  myResponseWaper.sendRedirect(config.getServletContext().getContextPath()+"/static/"+fileName);
    }

    @Override
    public void destroy() {
        config=null;
        System.out.println("destroy");
    }
}

```

