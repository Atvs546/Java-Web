# 1.jsp技术规范

jsp规范存在于:jsp-api.jar包



来自于javaEE规范的一种
jsp规范制定如何开发jsp文件代替响应对象将处理结果写入到响应体的开发流程
jsp规范制定了Http服务器应该如何调用管理jsp文件



响应对象将处理结果写入到响应体中存在的弊端:
PrintWriter out=reponse.getWriter();
out.print("处理结果");
只能处理数据量较少的处理结果写入到响应体,如果处理结果的数量过多,使用响应对象会增加开发难度要写很多out.print



jsp文件的优势:
JSP文件在互联网通信过程中,是响应对象的替代品
降低将处理结果写入到响应体的开发工作量降低处理结果维护难度
在JSP文件开发时,可以直接将处理结果写入到JSP文件不需要手写out.print命令,在Http服务器调用JSP文件时,根据JSP规范要求自动的将JSP
文件书写的所有内容

# 2.jsp文件展示

每创建一个web网站就会自动创建一个index.jsp文件

<!--JSP文件在执行时,自动将所有内容写入到响应体中,从而节省书写out.print,language="java"表示可以书写java命令-->



```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <table border="2" align="center">
    <tr>
      <td>用户编号</td>
      <td>用户姓名</td>
      <td>用户密码</td>
    </tr>
    <tr>
      <td>01</td>
      <td>刘勇</td>
      <td>227993</td>
    </tr>
  </table>
  </body>
</html>
```

如上jsp文件会自动将表格的信息写入到响应体中,展示在浏览器上

# 3.jsp文件中的java命令规范

```html
<%@ page import="com.entry.user" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<！--在jsp文件中直接书写java命令,不能被jsp文件识别,只会被当做字符串写入到响应体中
    需要将java代码写入到特殊标记中,这个标记称为执行标记<%     %>
-->

<%
   // 只有将java代码写入到这个标记中才会执行这段代码
    //1.声明java变量
    int num1=20;
    int num2=30;


    int num3=num1+num2;//数学运算
    int num4=(num2>=num1?num2:num1);//关系运算
    boolean num5=num2>=200&&num1>=100;//逻辑运算
    //声明控制语句
    if(num2>=num1) {

    }else {

    }

    for(int i=0;1<=10;i++){

    }
%>
<!--在jsp文件,通过输出标记,通知jsp将java变量的值写入到响应体中-->
变量num1的值:<%=num1%></br>
变量num2的值:<%=num2%></br>
<!--执行标记还可以通知jsp将运算结果写入到响应体中-->
num1+num2=<%=num1+num2%>

<1--jsp导入java类,如在其他包里有一个user类,创建对象,并且按alt+enter键导包-->
<%
 user us=new user("liu","123");
%>
学生姓名:<%=us.getUsername()%>

在jsp里把所有的执行标记看成一个整体,所以上一个执行标记声明的变量也可以在别的执行标记中使用
所以可以把int age=15,if(){    }else{  }的结构拆卸成如下,还是一个完整的if语句,这时就可以将对应条件的结果放入结构中
<%
 int age=15;
 %>
<%
 if(age>=18){
  %>
<font style="color: red;>热烈欢迎</font>"
<%
 }else{
%>
<font style="color: red">谢绝入内</font>
<%
 }
%>
```



<!--将类中的属性写入到表格中-->

```html
<%
 user ut1=new user("刘勇","20");
 user ut2=new user("刘丽","21");
 user ut3=new user("刘骏","22");
 List list=new ArrayList();
 list.add(ut1);
 list.add(ut2);
 list.add(ut3);
%>
<table>
 <tr>
  <td>姓名</td>
  <td>年龄</td>
 </tr>
 <%
  for( user ut:list){
 %>
 <tr>
  <td>
  <%=ut.getUsername()%>
  </td>
  <td>
   <%=ut.getUserid()%>
  </td>
 </tr>
 <%
  }
 %>

</table>
```

# 4.jsp文件中的内置对象

```
<age import="java.rmi.ServerException" %>

<%@ page import="java.io.IOException" %>

<%@ page contentType="text/html;charset=UTF-8" language="java" %>

从Servlet将数据共享到jsp文件中的三个作用域对象

/       会话域作用域对象

​        ServletContext application = request.getServletContext();

​        application.setAttribute("sid", 10);

//        全局作用域对象

​        HttpSession session=request.getSession();

​        session.setAttribute("sid",10);

//       请求作用域对象

​        request.setAttribute("sid",10);





<!--JSP文件内置对象:request

​    类型:HttpServletRequest

​    作用:1.在JSP文件运行时读取请求包的信息

​         2.与Servlet在请求转发过程中实现数据共享

 浏览器:http://localhost:8080/myweb/request.jsp?username=alen&password=123

获取这个请求协议包的请求参数-->

<%

 //在JSP文件执行时,借助于内置的request对象读取当前请求包里的请求参数

  String username=request.getParameter("username");

  String password=request.getParameter("password");



%>

来访用户的姓名<%=username%>

来访用户的密码<%=password%>





\--------------------------------------------------------------------------------------------------------------------------------

<!--JSP文件的内置对象session

   类型:Httpsession

   作用:JSP文件在运行时,可以session指向当前用户的私人储物柜,添加共享数据或者读取共享数据

-->

<!--将共享数据添加到用户的私人储物柜-->

<%

 //向tomcat索要当前用户逇私人储物柜

 session.setAttribute("key1",200);



%>





<!--在同一个用户的另一个jsp文件中,取出session中的数据,实现数据的共享-->

<%

 //将存放在session的数据取出

 Integer value= (Integer) session.getAttribute("key1");

%>



\----------------------------------------------------------------------------------------------------------------------------------------------

<!--ServletContext application:全局作用域对象

同一个网站中的Servlet与JSP,都可以通过当前网站中的这个对象实现数据的共享

JSP文件的内置对象:application

-->
```



```java
<%
 application.setAttribute("key2","Helloworld");
%>

在Servlet中拿出jsp文件中放入的数据:
public class deleteservlet extends HttpServlet {
public  void doGet(HttpServletRequest request, HttpServletResponse response)throws ServerException, IOException {
  ServletContext application=request.getServletContext();
 String value= (String) application.getAttribute("key2");
    System.out.println(value);
}
}
```

# 5.servlet和jsp文件的分工

Servlet与JSP文件的分工

Servlet:负责处理业务并得到处理结果-----------厨师

JSP:不负责业务的处理,将Servlet的处理结果写入到响应体中--------传菜员





关于Servlet与JSP之间的调用关系

Servlet在工作完毕后,一般通过请求转发的方式向tomcat申请调用JSP,如下Servlet通过请求转发的方式调用fail.jsp文件

 request.getRequestDispatcher("/fail.jsp").forward(request,response);





\---------------------------------------------------

Servlet和jsp文件实现数据共享(Servlet将处理结果共享给jsp文件)

1.Servlet将处理结果添加到[请求作用域对象]

2.JSP文件在运行时从[请求作用域]得到处理





Servlet中:处理业务并得到处理结果:(查询学员信息)

```java
public class findservlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws SecurityException, IOException {
           Student s1=new Student("1","mike");
	   Student s2=new Student("2","jack");
	   //将这些学生放入到List集合中
	   List list=new ArrayList();
	   list.add(s1);
	   list.add(s2);
	   //将处理结果放入请求作用域对象
	   request.setAttribute("key",list);
	   //通过请求转发的方案向指定的jsp文件申请调用
	     request.getRequestDispatcher("/user.jsp").forward(request,response);

    }
    }
```



在user,jsp文件中

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
//从请求作用域对象中得到Servlet当中添加进去的共享数据
   List stulist= Collections.singletonList(request.getAttribute("key"));
   
%>
<!--将处理结果写入到响应体中-->
<table border="2" align="center">
    <tr>
        <td>
            编号
        </td>
        <td>
            姓名
        </td>
    </tr>
    <tr>
        <%
            for(Student stu:stulist){
        %>
        <td><%=stu.getId()%></td>
        <td><%=stu.getName()%></td>
        <%
            }
        %>
    </tr>
</table>
```

# 6.jsp文件的运行原理

jsp文件的运行原理:

一.Http服务器调用jsp文件的步骤?

1.Http服务器将jsp文件的内容编辑为一个Servlet接口实现类;

2.Http服务器将Servlet接口实现类进行编译为类文件

3.Http服务器负责创建类文件的实例对象

4.Http服务器通过Servlet实例对象调用_jspService方法,将jsp文件内容写入到响应体中



二.Http服务器编辑与编译jsp文件的位置?

在work文件夹下看到了jsp类文件和编译文件