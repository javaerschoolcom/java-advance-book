# JSP技术

### 什么是JSP

> JSP是（java service page)的简称，是java核心技术中的一种

> JSP=脚本+HTML+CSS+JS组成的页面

### JSP的特点

* 和ASP比较，JSP可以运行在非Window的系统上
* 和HTML比较，它可以展示动态的内容
* 和JavaScript比较，它可以操作数据库，可以和服务器对接
* 和Servlet比较，简化了视图的表现方式

### JSP原理

* JSP本质上就是Servlet，可以从以下地址找到：

  > C:\Users\Administrator\.IntelliJIdea15\system\tomcat\Tomcat_7_0_29_javaweb2\work\Catalina\localhost\javaweb1\org\apache\jsp

* JSP首次会编译成class文件

### JSP组成

##### 模板元素HTML

##### 注释

> 语法：<%-- 注释 --%>

#####  元素

* 脚本元素

  > 声明语法：<%!  [代码块|定义变量|定义方法    %>  在_jspService方法之外的声明方式，相当于类的成员

  > 表达式语法：<%=  表达式  %>  注意：表达式后面不要加分号

  > 普通语法: <%  JAVA代码  %> 在该脚本中定义的变量存在于_jspService方法只中，属于临时变量

* 指令元素

  > 语法 ：<%@ 指令名   属性1=值1 属性2=值2.....  %>

  > page指令：用于描述页面的信息，不会在视觉上表现出来，常用属性如下：

  | 属性名       | 描述                      | 案例                                   |
  | ------------ | ------------------------- | -------------------------------------- |
  | language     | 页面使用的语言            | language="java"                        |
  | contentType  | 页面呈现的方式            | contentType="text/html;charset-utf-8"  |
  | import       | 导入java包                | import="java.util.Random,java.io.File" |
  | errorPage    | 指定错误处理页面          | errorPage="error.jsp"                  |
  | isELIgnored  | 该页面是否忽略EL表达式    | isELIgnored=“false"                    |
  | isThreadSafe | 指定该类是否线程安全      | isThreadSafe=”false“                   |
  | pageEncoding | 对响应的内容进行编码      | pageEncoding="utf-8"                   |
  | session      | 指定该页面能否使用session | session="true"                         |
  | buffer       | 指定页面输出缓冲区大小    | buffer="8kb"                           |

  > include指令：静态包含另外一份文件，合并到当前文件中

  ``` java
  注意事项：
  1）include指令会在编译前合并其他包含进来的文件，一次性编译
  2）在包含中文件和被包含文件可以进行变量共享，尽量不要定义重复变量
  ```

  > taglib指令：需要使用第三方提供的内置函数JSTL这样的标签库，需要引入第三方库

* 动作元素

  > 目的是尽可能降低在jsp中直接写大量的java代码，把常用的方法转化为标签的方式，从而简化开发

  > 语法：``` <jsp:actionElement attr1=value1></jsp:actionElement>```

  > 常用的动作元素如下：

  ```  jsp
  //动态包含一个网页404.jsp，编译后合并
  <jsp:include page="404.jsp">
      <jsp:param name="p1" value="value1"></jsp:param>
      <jsp:param name="p2" value="value2"></jsp:param>
  </jsp:include>
  //请求转发到404.jsp
  <jsp:forward page="404.jsp">
  	<jsp:param name="p1" value="value1"></jsp:param>
      <jsp:param name="p2" value="value2"></jsp:param>
  </jsp:forward>
  /*创建一个com.Student.java类的实例
  * id:实例对象的引用
  * class:类所在的全路径名
  * scope:类
  */
  <jsp:useBean id="student" class="com.Student"></jsp:useBean>
  /*给实例设置指定成员变量的值
  * name:实例的引用
  * property:实例对应类中对应的成员变量名
  * value:设置的值可以为[字符串|脚本表达式|el表达式]
  * param:url传递的参数的key
  */
  <jsp:setProperty name="student" property="name" value="lero"></jsp:setProperty>
  /*获取实例指定的成员变量的值
  * name:实例的引用
  * porperty:实例对应类中对应的成员变量名
  */
  <jsp:getProperty name="student" property="name"></jsp:getProperty>
  ```

### JSP内置对象

> 可以不用通过new创建，便能直接使用的对象,总共九大内置对象

| 对象名      | 作用                                                 |
| ----------- | ---------------------------------------------------- |
| pageContext | 可以关联出其他的内置对象                             |
| page        | 相当于JAVA中的this                                   |
| out         | 相当于response.getWriter();打印流对象                |
| exception   | 可以做异常相关的操作，注意的该对象只能在错误页面使用 |
| config      | 相当于ServletConfig                                  |
| request     | 相当于HttpServletRequset                             |
| response    | 相当于HttpServletResponse                            |
| application | 相当于ServletContext                                 |
| session     | 相当于HttpSession                                    |

##### pageContext

> 持有其他8大对象的引用

> 常用方法如下：

| 方法                                         | 说明                                                        | 案例                                                         |
| -------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| pageContext.setAttribute(String,Object);     | 在当前页面存储                                              | pageContext.setAttribute("name","lero");                     |
| pageContext.setAttribute(String,Object,int); | 存入数据到指定作用域                                        | pageContext.setAttribute("name","xxx",PageContext.REQUEST_SCOPE); |
| pageContext.getAttribute(String);            | 获取当前页面存储值                                          | pageContext.setAttribute("name");                            |
| pageContext.getAttribute(String,int);        | 获取指定作用域存储值                                        | pageContext.getAttribute("name",PageContext.REQUEST_SCOPE);  |
| pageContext.findAttribute(String)            | 依次从pageContext,request,session,application找，找到就停止 | pageContext.findAttribute("name")                            |
| pageContext.forward(String);                 | 请求转发，参数不丢失                                        | pageContext.forward("/404.jsp");                             |
| pageContext.include("/404.jsp");             | 动态包含，分别编译再合并                                    | pageContext.include("/404.jsp");                             |

##### page

> 相当于JAVA中的this,在jsp中不是很常用

##### out

>输出内容到浏览器

##### exception

> 只能在错误页面使用，其他页面使用无效

##### config

> 就是ServletConfig,获取Servlet初始化参数

##### request

> 就是HttpServletRequst

##### response

> 就是HttpResponse

##### application

> 就是ServletContext

##### session

> 就是HttpSession

####  作用域

> pageContext,request,session,application四个对象中都能存储数据，作用域依次从小到大