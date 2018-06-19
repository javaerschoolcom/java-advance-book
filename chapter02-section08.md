# 监听器

### 监听器概述

>  在JAVAWEB中，指的是Servlet中的一个技术标准接口，主要作用监听某一个对象上的事件，若发生指的事件，监听器则做出对应的响应和处理

### 事件驱动模型三要素

* 事件源：事件发生的地点
* 事件对象：事件源发生事件的类型，是事件源和事件监听器桥梁
* 事件监听器：事件捕捉和处理

### 监听器原理案例

> 设计一个监听器用于监听水烧开这一动作的执行

> Water类（事件源）

``` java
public class Water {
    private  WaterListener   listenter;
    //被监听的方法：水烧开了
    public  void  hot(){  
        if(listenter!=null){
            listenter.watchWaterHot(new Event(this));
        }
    }
    //注册监听器
    public void  registerListener(WaterListener   listenter){
        this.listenter =listenter;
    }
}
```

> Event类(事件对象)

``` java
public class Event {
    public  Event(Water  source){
        this.source = source;
    }
    //事件源
    private  Water  source;
    public void setSource(Water source) {
        this.source = source;
    }
    public Water getSource() {
        return source;
    }
}
```

> WaterListener接口（事件监听器）

``` java
public interface WaterListener {
    //监听水烧开
    void  watchWaterHot(Event e);
}
```

> Test测试类

``` java
public class Test {
    public static void main(String[] args) {
        //模拟事件源(水)
        Water water = new Water();
        //给事件源注册监听器：监听事件源(水)的hot()方法
        water.registerListener(new WaterListener() {
            @Override
            public void watchWaterHot(Event e) {
                //可以通过参数e:获取到监听对象Water source = e.getSource();
                System.out.println("监听到了水烧开了！");
            }
        });
        //水烧开
        water.hot();
    }
}
```

### Servelt中内置监听器

> 概念：JavaWeb中的监听器是Servlet规范中定义的一种特殊类，它用于监听web应用程序中ServletContext, HttpSession和 ServletRequest等域对象的创建与销毁事件，以及监听这些域对象中的属性发生修改的事件

> 在Servlet规范中定义了多种类型的监听器，它们用于监听的事件源分别为ServletContext，HttpSession和ServletRequest这三个域对象

##### 监听器分类

* 监听域对象自身的创建和销毁的事件监听器
* 监听域对象中的属性的增加和删除的事件监听器
* 监听绑定到HttpSession域中的某个对象的状态的事件监听器

##### 监听ServletContext域对象的创建和销毁

>  ServletContextListener接口用于监听ServletContext对象的创建和销毁事件,实现了ServletContextListener接口的类都可以对ServletContext对象的创建和销毁进行监听

* 当ServletContext对象被创建时，激发contextInitialized (ServletContextEvent e)方法
* 当ServletContext对象被销毁时，激发contextDestroyed(ServletContextEvent e)方法
* 激发contextInitialized方法时机：服务器启动针对每一个Web应用创建ServletContext
* 激发contextDestroyed方法时机：服务器关闭前先关闭代表每一个web应用的ServletContext

##### 案例

> 开发ServletContextListener监听器

``` java
public class LeroApplicationListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent e) {
        /**
         * 应用场景 1）读取公共的配置信息  2)统计在线人数  3）启动定时任务
         * 可以从参数 e 获取到 ServletContext对象
         */
        Properties  p =  new Properties();
        InputStream in = LeroApplicationListener.class.getClassLoader().getResourceAsStream("init.properties");
        try {
            p.load(in);
        } catch (IOException e1) {
            e1.printStackTrace();
        }
        String username = p.getProperty("author");
        System.out.println("监听到了ServletContext 初始化 读取了配置文件init.properties中的作者是"+username);
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        /**
         * 应用场景 1）写出文件到硬盘  2)释放资源
         * 可以从参数 e 获取到 ServletContext对象
         */
        System.out.println("监听到了ServletContext 销毁 ");
    }
}
/**
*配置文件放在src下面，文件为init.properties，内容如下所示：
*author=lero
*/
```

> 在web.xml进行注册

``` xml
  <listener>
        <listener-class>listener.LeroApplicationListener</listener-class>
  </listener>
```

> 执行结果

``` text
服务器启动输出"监听到了ServletContext 初始化读取了配置文件init.properties中的作者是lero"
服务器关闭输出"监听到了ServletContext 销毁"
```

##### 监听HttpSession域对象的创建和销毁

> HttpSessionListener 接口用于监听HttpSession对象的创建和销毁

* 创建一个Session时，激发sessionCreated(HttpSessionEvent e)方法
* 销毁一个Session时，激发sessionDestroyed(HttpSessionEvent e)方法

##### 案例

```  java
public class LeroSessionListener implements HttpSessionListener{
    @Override
    public void sessionCreated(HttpSessionEvent e){
        /**
         * 应用场景 1）统计在线人数  2）记录访问日志
         * 可以从参数 e 获取到 httpSession对象
         */
        System.out.println("监听到了session为:"+e.getSession().getId()+"的用户访问本站");
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent e) {
        /**
         * 1）用户退出的时候触发 2）session过期的时候触发
         */
        System.out.println("监听到了session为:"+e.getSession().getId()+"的用户退出本站");
    }
}

```

##### 监听ServletRequest域对象的创建和销毁

> ServletRequestListener接口用于监听ServletRequest 对象的创建和销毁

* request对象被创建时，监听器的requestInitialized(ServletRequestEvent e)方法将会被调用

* request对象被销毁时，监听器的requestDestroyed(ServletRequestEvent e)方法将会被调用

* ServletRequest域对象创建和销毁时机：

  创建：用户每一次访问都会创建request对象

  销毁：当前访问结束，request对象就会销毁

##### 案例

``` java
public class LeroRequestListener implements ServletRequestListener {
    @Override
    public void requestDestroyed(ServletRequestEvent e) {
        System.out.println("监听到了request对象的销毁");
    }

    @Override
    public void requestInitialized(ServletRequestEvent e) {
        /**
         * 应用场景 1）读取请求参数  2)记录访问历史
         * 可以从参数 e 获取到 ServletRequest对象
         */
        System.out.println("监听到了request对象的创建");
        HttpServletRequest   request = (HttpServletRequest)e.getServletRequest();
        String requestURI = request.getRequestURI();
        request.getParameter("username");
        System.out.println(requestURI);
    }
}
```

### 监听器注册两种方法

* 使用注解@WebListener注册监听器(直接在监听器类上使用)适用场景：自己编写的监听器
* 在web.xml注册监听器,适用场景：别人提供的监听器,不能直接添加注解

