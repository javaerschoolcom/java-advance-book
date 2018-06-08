# JSTL

### JSTL是什么

> 是（JSP Standard Tag Language）的检查，作用把JSP中的JAVA代码转换为标签的形式，简化开发

### 如何使用JSTL

1. 必须引进两个jar文件jstl.jar和standard.jar
2. 在要使用的JSP文件中引入<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

###  JSTL分类

* ```jsp
  //JSTL核心，必须掌握
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  ```

* ```jsp
  //格式化数据（文本|数字|时期|时间）
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/fmt" %>
  ```

* ```jsp
  //处理字符串函数
  <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
  ```

* ```jsp
  //操作数据库
  <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
  ```

* ```jsp
  //操作XML
  <%@ taglib prefix="xml" uri="http://java.sun.com/jsp/jstl/xml" %>
  ```

### JSTL核心标签

> 语法：```<c:标签名></c:标签名>```

##### out标签

> 作用输出内容到JSP页面,属性说明：

``` jsp
value:描述的输出到页面的值
default:如果value没有值，会采用默认值
escapeXml:true把内容当成字符串，false解析里面特殊符号
eg:
<c:out value="<h1>lero</h1>" default="xxx" escapeXml="false"></c:out>
```

### set标签

> 作用是给某个作用域设置值，属性说明：

``` jsp
 value:要存储的值
 var:开辟一个内存空间用于表示value存储的值
 scope:值存在何种作用域
 //向作用域存储值
 <c:set value="${param.account}" var="username"></c:set>
 <c:out value="${username}"></c:out>
 //向对象设置值
 <jsp:useBean  id="student" class="fileupload.Student"  ></jsp:useBean>
 <c:set target="${student}" property="name" value="hello" ></c:set>
 <jsp:getProperty name="student" property="name"></jsp:getProperty>
```

##### remove标签

> 删除某个变量,属性说明：

``` jsp
var:要删除的变量名
scope:要删除哪个作用域中的变量
 <c:set value="${param.account}" var="username"></c:set>
 <c:remove var="username"></c:remove>
 <c:out value="${username}" default="默认值"></c:out> //结果：默认值
```

##### if标签

> 作为逻辑分支使用，没有else标签，属性说明：

``` jsp
test:进行表达式逻辑判断，结果为ture,输出标签之间的内容
eg:
http://localhost/javaweb2/test.jsp?account=lero&number=20?status=2
<c:set value="${param.number}" var="number"></c:set>
<c:if test="${number>10}">该数大于10</c:if>

<input type="radio" name="gender" value="1"  <c:if test="${param.status eq 1}">checked="checked"</c:if> >女
<input type="radio" name="gender" value="2"   <c:if test="${param.status eq 2}">checked="checked"</c:if> >男
```

##### choose|when|otherwise标签

> 该三个标签用于复杂的分支结构，必须联合使用相当于if()else if()else()

``` jsp
test:进行表达式逻辑判断，结果为ture,输出标签之间的内容
eg：
http://localhost/javaweb2/test.jsp?account=lero&number=20?status=2
<c:choose>
        <c:when test="${num gt 10}">该数是${num},其大于10</c:when>
        <c:when test="${num gt 5}">该数是${num},其大于5</c:when>
        <c:otherwise>该数是其他数</c:otherwise>
</c:choose>
```

catch标签

> 发生异常处理标签，属性说明：

``` jsp
var:封装发生错误的对象
eg：
<c:catch var="ex">
         <%= 1/0 %>
    </c:catch>
<c:out value="${ex.message}"></c:out>
```

##### forEach

> 循环结构，相当于foreach

``` jsp
var:每次循环的变量
items:要循环的集合|数组|map
begin:指定循环的开始位置，默认的是0
end:指定循环的结束位置，默认为items的长度
varStatus:存储的是每次循环相关的信息
        count:循环的次数
        first:是否是循环中的第一个元素
        last：是否是循环中的最后一个元素
        index:每次循环的索引,默认值等于begin
        begin:循环的开始位置
        end:循环的结束位置
  <%
       ArrayList arrayList = new ArrayList();
         arrayList.add("张三");
         arrayList.add("李四");
         arrayList.add("王五");
      pageContext.setAttribute("list",arrayList);
     %>

 <c:forEach var="item" items="${list}" begin="0" end="${list.size()}" varStatus="status">
        <c:choose>
            <c:when test="${(status.index mod 2)==0}"><h1 style="background-color: darkcyan">${item}<h1></c:when>
            <c:otherwise><h1 style="background-color: darkmagenta">${item}<h1></c:otherwise>
        </c:choose>
            <span>总共：${status.count}</span>
            <span>是否是第一个：${status.first}</span>
            <span>是否是最后一个：${status.last}</span>
            <span>当前索引：${status.index}</span>
            <span>end：${status.end}</span>
            <span>begin：${status.begin}</span>
   </c:forEach>
```

##### forTokens标签

> 和forEach类似，常用于分割字符串，类似于split()方法

```jsp
拥有forEach所有的属性，另外多个一个分割属性：
delims：采用何种方式进行分割

<c:forTokens items="zhangsan|wangwu|lisi" delims="|" var="item" >
             ${item}
</c:forTokens>
```

##### url|redirect|param标签

> 处理url和重定向

``` java
//重定向到index.jsp中会带上参数param1和param2
<c:url value="index.jsp" var="url">
      <c:param name="param1" value="xx"></c:param>
      <c:param name="param2" value="oo"></c:param>
  </c:url>
 <c:redirect url="${url}"></c:redirect>
```

### 格式化标签

##### formatDate标签

> 格式化日期

``` java
属性	描述	是否必要	默认值
value	要显示的日期	是	无
type	DATE, TIME, 或 BOTH	否	date
dateStyle	FULL, LONG, MEDIUM, SHORT, 或 DEFAULT	否	default
timeStyle	FULL, LONG, MEDIUM, SHORT, 或 DEFAULT	否	default
pattern	自定义格式模式	否	无
timeZone	显示日期的时区	否	默认时区
var	存储格式化日期的变量名	否	显示在页面
scope	存储格式化日志变量的范围	否	页面

{type:both}  2011-3-30 10:07:42
{type:date}  2011-3-30
{type:time}  10:07:42
{type:date, dateStyle:default}  2011-3-30
{type:date, dateStyle:short}  11-3-30
{type:date, dateStyle:medium}  2011-3-30
{type:date, dateStyle:long}  2011年3月30日
{type:date, dateStyle:full}  2011年3月30日 星期三
{type:time, timeStyle:default}  10:07:42
{type:time, timeStyle:short}  上午10:07
{type:time, timeStyle:medium}  10:07:42
{type:time, timeStyle:long}  上午10时07分42秒
{type:time, timeStyle:full}  上午10时07分42秒 CST
{type:both, pattern:EEEE, MMMM d, yyyy HH:mm:ss Z}  星期三, 三月 30, 2011 10:07:42 +0800
{type:both, pattern:d MMM yy, h:m:s a zzzz}  30 三月 11, 10:7:42 上午 中国标准时间
{pattern:dd/MM/yyyy HH:mm aa}  30/03/2011 13:12 下午
{pattern:dd/MM/yyyy hh:mm aa}  30/03/2011 01:12 下午

eg：
<fmt:formatDate value="<%= new Date()%>" var="resultDate"  pattern="yyyy-MM-dd" ></fmt:formatDate>
<c:out value="${resultDate}"></c:out>
```

##### formatNumber标签

``` java
属性	描述	是否必要	默认值
value	要显示的数字	是	无
type	NUMBER，CURRENCY，或 PERCENT类型	否	Number
pattern	指定一个自定义的格式化模式用与输出	否	无
currencyCode	货币码（当type="currency"时）	否	取决于默认区域
currencySymbol	货币符号 (当 type="currency"时)	否	取决于默认区域
groupingUsed	是否对数字分组 (TRUE 或 FALSE)	否	true
maxIntegerDigits	整型数最大的位数	否	无
minIntegerDigits	整型数最小的位数	否	无
maxFractionDigits	小数点后最大的位数	否	无
minFractionDigits	小数点后最小的位数	否	无
var	存储格式化数字的变量	否	Print to page
scope	var属性的作用域	否	page

pattern说明：
0	代表一位数字
E	使用指数格式
#	代表一位数字，若没有则显示 0，前导 0 和追尾 0 不显示。
.	小数点
,	数字分组分隔符
;	分隔格式
-	使用默认负数前缀
%	百分数
?	千分数
¤	货币符号，使用实际的货币符号代替
X	指定可以作为前缀或后缀的字符
'	在前缀或后缀中引用特殊字符
 eg:
 <c:set var="num" value="123456.789"></c:set>
 <fmt:formatNumber  value="${num}" pattern="0000.00%" /> //12345678.90%
 <fmt:formatNumber  value="${num}" type="CURRENCY" />    // ￥123,456.79
 <fmt:formatNumber  value="${num}" max	FractionDigits="2" /> //123,456.79
 <fmt:formatNumber  value="${num}" groupingUsed="false" />  //123456.789
```

### 处理字符串函数

``` java

```



### 自定义标签

> 自己开发的标签，能够在JSP中直接使用,也可以提供给别人使用

##### 分析

> 标签本质上上是处理器赋有的功能，也就是说开发标签本质上是开发处理器
>
> 处理器的顶级接口为JspTag，它是一个标志性的接口，核心接口方法定义在子接口SimpleTag中，如下：

``` java
void doTag() throws JspException, IOException; //处理器核心业务逻辑方法

void setParent(JspTag var1); // 把父标签处理器对象传递给进当前标签处理器

JspTag getParent();  // 在当前标签处理器中获取父标签处理器对象

void setJspContext(JspContext var1); //把当前jsp中的pageContext对象传递到处理器中

void setJspBody(JspFragment var1); //把标签之间的内容封装到JspFragment对象中，并传递给处理器
```

##### 执行自定义标签流程

1. 请求JSP页面的时候解析到自定义标签位置,WEB容器调用标签处理器对象的setJspContext方法，把当前JSP中的pageContext对象传递到处理器对象
2. 如果该标签有父标签，则WEB容器会调用setParent方法，将父标签处理器对象传递给这个标签处理器对象
3. 如果标签上有属性，则WEB容器会调用每个属性对应的setter方法，把属性值传递给标签处理器对象，如果标签的属性值是EL表达式，则WEB容器首先计算表达式的值，然后把值传递给标签处理器对象，并且8种基本数据类型会自动转化
4. 如果标签有内容，WEB容器将调用setJspBody方法，把标签内容封装成JspFragment对象，并传递给这个标签处理器，可以实现输出|修改|循环标签内容的目的

##### 开发简介标签步骤：

* 自定义类继承SimpleTagSupport

``` java
public class MyTag extends SimpleTagSupport {
    //eg:标签<lero>xxx</lero>
    @Override
    public void doTag() throws JspException, IOException {
        //获取PageContext对象
        PageContext jspContext = (PageContext) this.getJspContext();
        //获取标签之间的内容，封装到JspFragment对象中
        JspFragment jspBody = this.getJspBody();
        //获取输出到浏览器的流对象
        JspWriter out = jspContext.getOut();
        //标签之间的内容，通过输出流写到浏览器
        jspBody.invoke(out);
    }
}
```

* 自定义xxx.tld文件

``` xml
    <description>welcome use lero tag !</description>
    <display-name>xxx</display-name>
    <tlib-version>1.1</tlib-version>
    <short-name>lero</short-name> <!--默认的标签前缀如<head:lero>中的head-->
    <uri>/xxx/lero</uri> <!--<%@ taglib prefix="lero" uri="/xxx/lero" %>中的uri-->
    <tag> <!--每开发一个标签，则需要在该文件添加一个tag节点-->
        <description>not used</description><!--描述信息-->
        <name>lero</name> <!--标签名称如<head:lero>中的lero-->
        <tag-class>tag.MyTag</tag-class> <!--标签处理器类-->
        <body-content>scriptless</body-content>
        <!--设置标签体类型
		该值有4个：empty  JSP  scriptless tagdepentend
         empty：指定该标签是单标签，没有文本
         JSP： 指定标签中有文本，在实现了Tag接口的标签中允许设置，在实现了SimpleTagSupport					  中不允许设置成JSP,如果设置成JSP就会出现异常
         scriptless：表示该标签是有标签体的，但是标签体中的内容不可以是<%java代码%>
         tagdepentend：标签体里面的内容是给标签处理器类使用的(不常用)
         -->
    </tag>
```

* 使用自定义标签

``` jsp
//在jsp中使用指令引入标签的tld文件
<%@ taglib prefix="head" uri="/xxx/lero" %>
//使用方法如下：
<head:lero>hello world!</head:lero> //结果：hello worlld!
```

##### 开发带属性的标签

* 自定义类继承SimpleTagSupport

``` java
public class MyTag1 extends SimpleTagSupport {
    private boolean test;//变量名test和标签中的属性名一直<head:lero test="true">
    public boolean getTest() {
        return test;
    }
    public void setTest(boolean test) { //该方法把标签中test属性传递到处理器中
        this.test = test;
    }
    @Override
    public void doTag() throws JspException, IOException {
        PageContext jspContext = (PageContext) this.getJspContext();
        JspFragment jspBody = this.getJspBody();
        JspWriter out = jspContext.getOut();
        if(getTest()){
            jspBody.invoke(out);//输出到浏览器
        }
    }
}
```

* 自定义xxx.tld文件

``` xml
    <description>welcome use lero tag !</description>
    <display-name>xxx</display-name>
    <tlib-version>1.1</tlib-version>
    <short-name>lero</short-name>
    <uri>/xxx/lero</uri>
    <tag>
        <description>not used</description>
        <name>lero</name>
        <tag-class>tag.MyTag1</tag-class>
        <body-content>scriptless</body-content>
         <attribute> <!--标签中每有一个属性，则需要在该文件添加一个attribute节点-->
            <description>xxxxxx</description>
            <name>test</name><!--标签属性的名称<head:lero test="true">中的test -->
            <required>true</required><!--指定标签该属性是否为必须设置，true为必须设置 -->
            <rtexprvalue>true</rtexprvalue>
             <!--指定标签该属性对应的值是否支持EL表达式，true为支持 -->
        </attribute>
    </tag>
```

##### 自定义标签控制JSP页面是否继续执行标签后的内容

> 在doTag方法抛出SkipPageException异常即可，JSP收到这个异常，将忽略标签余下jsp页面的执行

``` java
public class MyTag2 extends SimpleTagSupport {
    @Override
    public void doTag() throws JspException, IOException {
        //抛出一个SkipPageException异常就可以控制标签后面的JSP不执行
        throw new SkipPageException();
    }
}
```

``` xml
<description>welcome use lero tag !</description>
<display-name>xxx</display-name>
<tlib-version>1.1</tlib-version>
<short-name>lero</short-name>
<uri>/xxx/lero</uri>
<tag>
      <name>lero</name>
      <tag-class>tag.MyTag2</tag-class>
      <!-- 标签体允许的内容 ，empty表示该标签没有标签体-->
      <body-content>empty</body-content>
</tag>
```

``` jsp
<%@ page language="java" pageEncoding="UTF-8"%>
<%--在jsp页面中导入自定义标签库 --%>
<%--<%@taglib uri="/xxx/lero" prefix="head" %>--%>
<%--在jsp页面中也可以使用这种方式导入标签库，直接把uri设置成标签库的tld文件所在目录 --%>
<%@taglib uri="/WEB-INF/xxx.tld" prefix="head"%>
<!DOCTYPE HTML>
<html>
  <head>
    <title>自定义标签控制JSP页面标签后的内容不执行</title>
  </head>
  <body>
       <h1>lero tag</h1>
       <%--在jsp页面中使用自定义标签 --%>
       <head:lero />
       <!-- 这里的内容位于 <head:lero/>标签后面，因此不会输出到页面上 -->
       <h1>你看不见我</h1>
  </body>
</html>
```

##### 自定义标签修改JSP响应内容

> 把处理器拿到的页面数据，不直接通过jspBody.invoke(out)输出到浏览器，而是偷天换日，把标签中的内容输入到另一个Writer流对象中，修改数据后，从另一个Writer流对象输出到浏览器

``` java
public class MyTag3 extends SimpleTagSupport {
    @Override
    public void doTag() throws JspException, IOException {
        JspFragment jspFragment = this.getJspBody();
        StringWriter sw = new StringWriter();
        //将标签之间的的内容写入到StringWriter流中
        jspFragment.invoke(sw);
        //获取StringWriter流缓冲区的内容
        String content = sw.getBuffer().toString();
        String toHtml ="<p color='red'>"+content+"</p>"
        PageContext pageContext = (PageContext) this.getJspContext();
        //将修改后的content输出到浏览器中
        pageContext.getOut().write(toHtml);
    }
}
```

``` xml
<description>welcome use lero tag !</description>
<display-name>xxx</display-name>
<tlib-version>1.1</tlib-version>
<short-name>lero</short-name>
<uri>/xxx/lero</uri>
<tag>
        <name>lero</name>
        <tag-class>tag.MyTag3</tag-class>
        <body-content>scriptless</body-content>
</tag>
```

``` jsp
<%@ page language="java" pageEncoding="UTF-8"%>
<%--在jsp页面中导入自定义标签库 --%>
<%@taglib uri="/xxx/lero" prefix="head" %>
<!DOCTYPE HTML>
<html>
  <head>
    <title>自定义标签修改JSP响应内容</title>
  </head>
  <body>
       <h1>lero tag</h1>
       <%--在jsp页面中使用自定义标签 --%>
      <head:lero>my color changed</head:lero>
  </body>
</html>
```

