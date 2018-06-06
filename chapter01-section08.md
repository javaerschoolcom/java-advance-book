# EL表达式

### 什么是EL表达式

> Expression language的简称，是JSP内置的，可以在JSP页面直接使用

> 目的简化开发，使用更简单

### EL表达式使用

* 语法：${Expression}
* 可以从四大作用域获取值

``` java
   <%
       pageContext.setAttribute("name1","lero");
       pageContext.setAttribute("name2","REQUEST_SCOPE",PageContext.REQUEST_SCOPE);
       pageContext.setAttribute("name3","SESSION_SCOPE",PageContext.SESSION_SCOPE);
       pageContext.setAttribute("name4","APPLICATION_SCOPE",PageContext.APPLICATION_SCOPE);
   %>
   <!-- pageContext.findAttribute("name")-->
   <p>pageContext作用域的值是：${name1}</p>
   <p>REQUEST_SCOPE作用域的值是：${name2}</p>
   <p>SESSION_SCOPE作用域的值是：${name3}</p>
   <p>APPLICATION_SCOPE作用域的值是：${name4}</p>
   <hr/>
```

* 可以取数组，集合，map中取值，可能会配置.或者[]操作符获取值

``` java
<%
     ArrayList list =  new ArrayList();
       list.add("value1");
       list.add("value2");
       list.add("value3");
       pageContext.setAttribute("result",list);
   %>
   <p>list中的第一个元素是：${result[0]}</p>
   <p>list中的第一个元素是：${result[1]}</p>

   <br/>
   <%
      Student  s =  new Student();
       s.setAge(18);
       s.setName("lero");
       pageContext.setAttribute("student",s);
   %>

   <p>student中的name是：${student["name"]}</p>
   <p>student中的age是：${student["age"]}</p>

   <hr/>
   <%
       HashMap map =  new HashMap();
       map.put("name","lero");
       map.put("age","18");
       pageContext.setAttribute("hashmap",map);
   %>

   <p>map中的name是：${hashmap["name"]}</p>
   <p>map中的name是：${hashmap.name}</p>
```

* 可以进行简单运算(算数运算，逻辑运算，比较运算，三目运算，空运算)

``` java
算术型
+、-、*、/、div、%、mod
逻辑型
and、&&、or、||、!、not
关系型
==、eq、!=、ne、lt、gt、<=、le、>=、ge。可以与其他值进行比较，或与布尔型、字符串型、整型或浮点型文字进行比较。
空
empty 空操作符是前缀操作，可用于确定值是否为空。
条件型
A ?B :C。根据 A 赋值的结果来赋值 B 或 C。

```

### EL内置对象

> 可以直接用EL表达式操作的对象

##### pageContext

> 它可以用于访问JSP 隐式对象,然后可以调用对象上的方法

``` java
${pageContext.response} //获取response
${pageContext.request}  //获取request
${pageContext.session}  //获取session
${pageContext.servletContext}//获取servletContext
${pageContext.out.println("hello")} //获取out并调用打印方法
${pageContext.page} //获取page
${pageContext.exception} //获取exception
${pageContext.servletConfig} //获取servletConfig
```

##### param

> 获取请求参数名称对应的参数值

``` java
eg：http://localhost/javaweb/index.jsp?username=lero
${param.name} //lero  相当于 request.getParameter()
```

##### paramValues

> 获取请求参数名称对应的多个值，映射到数组中

``` java
eg：http://localhost/javaweb/index.jsp?fav=man&fave=woman
${paramValues.name[0]} //man
${paramValues.name[1]} //woman
// 相当于 request.getParamterValues(name)
```

##### initParam

> 获取上下文初始化参数名称对应的值

``` xml
eg：xml配置
<context-param>
        <param-name>charset</param-name>
        <param-value>utf-8</param-value>
</context-param>
${initParam.charset}  //utf-8
//相当于 ServletContext.getInitparameter(String name)
```

##### header

> 获取请求头信息

``` java
${header.Host} //localhost
${header["User-Agent"]} //Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36
${header["Cache-Control"]} //max-age=0
//相当于 request.getHeader(name)
```

##### headerValues

> 获取请求头信息映射到数组中

``` java
${headerValues.Connection[0]} //keep-alive
//相当于 request.getHeaderValues(name)
```

##### cookie

> 获取请求中所带的cookie值

``` java
${cookie.JSESSIONID.value} // JSESSIONID对应的值
//如果请求包含多个同名的cookie，则应该使用 ${headerValues.cookie}获取多个
```

### 注意事项

``` java
指令<%@ page isELIgnored="true" %>
//表示是否禁用EL语言,TRUE表示禁止.FALSE表示不禁止.JSP2.0中默认的启用EL语言。使用的时候不需要引入任何jar
```











