# 1.servlet接口基础

Servlet:根据客户端提交的请求,调用服务端相关的java代码,完成对请求的处理与运算

Servlet接口位于tomcat-->lib-->Servlet.api中

创建Servlet接口的实现类并不是直接实现Servlet接口,而是继承Servlet接口的实现类HttpServlet,这是因为,我们所需要的Servlet接口的

实现类只用到两个方法,如果直接实现Servlet要将Servlet接口当中的所有方法进行重写,加大开发的难度,通过继承接口实现类的方法间接实现

Servlet接口,可以简化



```java
public class enterservlet  extends HttpServlet  {
//采用get的请求方式
    public  void  doGet(HttpServletRequest request, HttpServletResponse response) throws ServerException, IOException {
  
  }
  //采用post的请求方式
    public  void  doPost(HttpServletRequest request, HttpServletResponse response) throws ServerException, IOException {
}
```





Servlet的生命周期:

初始化阶段:调用init()方法

响应客户请求阶段:调用service()方法

终止阶段:调用destroy()方法







创建完Servlet接口的实现类之后要在web.xml进行配置

```xml
 <servlet>
        <servlet-name>four</servlet-name>
        <servlet-class>com.controller.enterservlet</servlet-class>    <!--Servlet接口实现类的文件路径-->
    </servlet>
    <servlet-mapping>
        <servlet-name>four</servlet-name>
        <url-pattern>four</url-pattern>      <!--给servlet接口实现类起一个别名-->
    </servlet-mapping>
```

# 2.HttpServletRequest接口和请求对象

HttpServletRequest对象代表客户端的请求,

//方法中两个形参分别是请求对象和响应对象通过请求对象可以获取客户端的所有请求信息

```java
public  void  doGet(HttpServletRequest request, HttpServletResponse response) throws ServerException, IOException {

request.setCharacterEncoding("utf-8")用来确保发往服务器的参数的编码格式,如果没有设置按照服务器端默认的“iso-8859-1”来进行编码



request.getParameter("id") 表单是按照name=value的方式进行提交,获取请求参数名id的value



//获取客户机信息:

getRequestURL() 返回客户端发出请求时的完整URL。

getRequestURI() 返回请求行中的参数部分。

getQueryString () 方法返回请求行中的参数部分（参数名+值）

getRemoteHost() 返回发出请求的客户机的完整主机名。

getRemoteAddr() 返回发出请求的客户机的IP地址。

getPathInfo() 返回请求URL中的额外路径信息。额外路径信息是请求URL中的位于Servlet的路径之后和查询参数之前的内容，它以"/"开头。

getRemotePort() 返回客户机所使用的网络端口号。

getLocalAddr() 返回WEB服务器的IP地址。

getLocalName() 返回WEB服务器的主机名。





//获取客户机请求头

getHeader(string name) 以 String 的形式返回指定请求头的值。如果该请求不包含指定名称的头，则此方法返回 null。

如果有多个具有相同名称的头，则此方法返回请求中的第一个头。头名称是不区分大小写的。可以将此方法与任何请求头一起使用



getHeaders(String name) 以 String 对象的 Enumeration 的形式返回指定请求头的所有值

getHeaderNames() 返回此请求包含的所有头名称的枚举。如果该请求没有头，则此方法返回一个空枚举



//获取客户机请求参数

getParameter(String name) 根据name获取请求参数(常用)

getParameterValues(String name) 根据name获取请求参数列表(常用)

getParameterMap() 返回的是一个Map类型的值，该返回值记录着前端（如jsp页面）所提交请求中的请求参数和请求参数值的映射关系。

(编写框架时常用)





//请求转发

request.getRequestDispatcher("/index.html").forward(request.response);





setAttribute(String name,Object o)方法，

将数据作为request对象的一个属性存放到request对象中,如创建数据库连接时,要提前创建20个Connection对象conn,要将conn装入到Map

集合中,但是因为创建的Map集合的对象是局部变量,不能在方法外打开,所以需要将Map集合放入到request对象中

request.setAttribute("key1",map);







getAttribute(String name)方法，获取request对象的name属性的属性值，例如：数据库连接池将存方法request对象中的map集合取出来

Map map=(Map)request.getAttribute("key1");





getAttributeNames方法

获取request对象的所有属性名，返回的是一个，例如：Enumeration

attrNames = request.getAttributeNames()；

  }
```

# 3.HttpServletResponse和响应对象

```java
HttpResponse response
response是响应对象
//设置向浏览器返回的文件类型与编码格式
response.setContentType("text/html;charset=utf-8");

//输出流向浏览器的用户反馈信息
 PrintWriter out ;
 out = response.getWriter();
 out.print("登录失败");


 //重定向到指定的url
 response.sendRedirect("/myweb/系统.html");

```

# 4.欢迎资源文件

欢迎资源文件:

前提:用户可以记住网站名,但是不会记住网站资源文件名

默认欢迎资源文件:用户向http服务器发送的针对于某个网站的默认请求时,此时由http服务器自动的从当前网站返回资源文件

正常的请求:http://localhost:8080/myweb/index.html

默认请求:http://localhost:8080/myweb/      当输入这个网站时.服务器返回一个资源文件,这个资源文件就是默认资源文件

关于tomcat对于默认欢迎资源文件的定位规则

1.规则位置:Tomcat安装位置/conf/web.xml文件

```xml
<welcome-file>index.html</welcom-file>
<welcome-file>index.htm</welcom-file>
<welcome-file>index.jsp</welcom-file>
```

当网站中存在index.html文件,这个文件就是默认资源文件,如果没有这个文件往下寻找

index.htm,如果没有这个资源文件,再往下寻找,index.jsp如果还是不存在这个资源文件,tomcat服务器会将404响应码放入到响应头中

设置当前网站的默认欢迎文件

规则位置:网站/web/WEB-INF/web.xml

规则相关的命令:<welcome-file>myself.html</welcom-file>

​    设置好后,不在遵循网站的默认资源文件,而是自己所设置的资源文件

​    默认资源文件既可以是静态资源文件也可以是动态资源文件,但是动态资源文件开头去除一条斜杠

​    <welcome-file>user/add</welcom-file>

​    user/add是servlet接口实现类重命名的文件

# 5.http状态码

Http状态码:

  介绍码:是一个由三位数字组成的一个符号

  由Http服务器在推送响应包之前,根据本次请求处理,将http的状态码写入到响应包的状态行上

  作用:http服务器针对本次请求,返回了对应的资源文件,用通过http状态码通知浏览器应该如何处理这个结果

  如果http服务器针对本次请求,无法返回对应的资源文件,通过http状态码,向浏览器解释不能提供服务的原因

 分类:

 1.组成100-599之间分为5个大类

##  1XX:1开头的状态码

 最有特征100;通知浏览器本次返回的资源文件并不是一个独立的资源文件,需要浏览器在接收到响应包后,继续向http服务器所依赖的其他资源文件

##    2XX:2开头的状态码

   最有特征200:通知浏览器本次返回的资源文件,是一个完整独立资源文件,浏览器在接收到之后不需要索要其他关联文件

##    3XX:3开头的状态码

   最有特征302,通知浏览器本次返回的不是一个资源文件内容而是一个资源文件的地址,需要浏览器根据这个地址自动发起请求来索要

   这个资源文件,如:respose.sendRedirect("资源文件的地址")写入到响应投location中,=

   tomcat将302状态码写入到状态行中,在浏览器接收到响应包之后,应为302状态码,浏览器不会读取响应体内容,自动根据响应头中的location当中的地址

   发起二次请求,

##    4XX开头的状态码:

   404:通知浏览器,由于在服务端没有定位到被访问的资源文件,因此无法提供帮助

   405:通知浏览器,在服务端已经定位到被访问的资源文件(servlet),但是这个servlet对于浏览器所采用的的请求方式不能处理

   通过地址栏访问网站是get请求方式,如果servlet接口的实现类采用的是post请求方式,则会出现405的状态码

   5XX开头的状态码:服务器已经定位到了被访问的资源文件,这个servlet可以接收浏览器采用的请求方式,但在servlet在处理请求期间,由于java异常处理导致处理失败

   如:int num=(int)map.get("key");会报空指针异常因为null不能赋值给int,应该写成Integer nunm=(Integer)map.get("key");

# 6.重定向和请求转发

多个servlet之间的调用规则:

前提条件:某些来自于浏览器发送请求,往往需要服务端中的多个servlet来协同处理,但是浏览器一次只能访问一个servlet,

导致用户需要手动通过浏览器发起多次请求,才能

得到服务,增加用户得到服务的难度,导致用户放弃访问

提高用户使用感受规则:无论本次请求设计多少个servlet,用户只需要手动的通知浏览器发起一次请求即可

调用规则的使用方案:1.重定向response.sendRedirect()     2.请求转发解决方案

假设需要访问oneservlet和twoservlet

重定向解决方案:用户手动通知浏览器访问opneservlet,oneservlet工作完毕后,

将twoservlet的地址写入到响应头的location中(respose.sendRedirect)

交给浏览器:跳转到twoservlet的地址,重定向的是通过地址栏发送请求的所以重定向的解决方案一定是get的请求方式

重定向的缺点:浪费时间(需要在浏览器和服务器之间多次往返,增加用户等待时间

请求转发解决方案:oneservlet工作完成后,通过请求对象代替浏览器向http服务器申请调用twoservlet来完成剩余任务

实现命令://1.通过当前请求对象生成资源文件生情报告对象

getRequestDispatcher report=request.getRequestDispatcher("/当前网站需要跳转的资源文件名");//一定要以斜线"/"开头如/two

​        //将报告对象发送给tomcat

​        report.forward(当前的请求对象,当前的响应对象)

优点:无论本次请求涉及多少servlet用户只需向浏览器发起一次请求,节省服务端与浏览器端之间往返次数,增加处理服务速度

相关的特征:只向浏览器发送一次请求

​          请求地址:只能向服务器申请调用当前网站下的资源文件的地址

​          请求方式随用户的请求方式而定

​	  所以请求转发:request.getRequestDispatcher("/当前网站需要跳转的资源文件名").forword(当前请求对象,当前响应对象)



# 7.Servlet接口之间的数据共享



## 1.PageContext

范围最小的一个域，可以获取其他八大内置对象，可以认为是一个入口对象，能够获取其他所有域中的数据。能跳转到其他资源，其身上提供forward和sendRedirect方法，简化了转发和重定向的操作，代表页面上下文，该对象主要用于访问jsp之间的共享数据，当对jsp的请求时开始，当响应结束时销毁

```jsp
<%

    pageContext.Method();

%>

pageContext对象的主要用法如下。

1）getOut()

该方法返回一个JspWriter类的实例对象，也就是JSP内置对象——out对象，可以往客户端写入输入流。它的使用方法如下：

<%

       JspWriter myout=pageContext.getOut();

       myout.println(“myout write: hello world”+”<br>”);

       out.println(“out write: hello world”);

%>
```



## 2.HttpServletRequest

1.介绍:

​    1.如果在同一个网站,连个servlet之间是通过请求转发的方式进行调用彼此之间共享同一个请求协议包,而一个请求协议包只对应一个请求对象,因此servlet之间共享一个

​    请求对象,此时可以利用这个请求对象在连个servlet之间实现数据的共享,两个请求方式必须是一致的



​    在请求对象实现servlet之间的数据共享功能的时候,开发人员将请求对象称为请求作用域对象

​    命令的实现:

​    oneservlet通过请求转发申请调用twoServlet时,需要给twoservlet提供共享数据

​    oneservlet:

  

```java
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.将数据添加到其你去作用域对象中attribute属性,可以是任意类型的数据
    request.setAttribute("key1",数据);
    2.向tomcat调用twoServlet
request.getRequestDispatcher("/two").forward(request.response);
    }

    twoservlet:
     public void doGet(HttpServletRequest request,HttpServletResponse response){
     //从当前请求对象中得到onesservlet写入到共享数据中
     Object 数据=request.getAttribute("key1");
     }
```

## 3.HttpSession

方式三:HttpSession接口
介绍:HttpSession接口来自于servlet规范下一个接口,存在于Tomcat的servlet-api中,其实现类由Http服务器提供的tomcat提供实现类存在于
如果两个servlet来自于同一个网站,并且为同一个浏览器/用户提供服务期,此时借助于HttpSession对象进行数据共享
开发人员习惯于HttpSession接口修饰对象称为会话作用域对象
HttpSession session=request.getSession();//确定用户是合法的用户,只能在用户登录验证的servlet中使用
HttpSession session=request.getSession(false);//不确定用户是否为合法的用户,这个是常用的
2.HttpSession与cook的区别:



HttpSession是一个接口,Cookie是一个类
存储位置不同,Cookie存放在客户端计算机(浏览器内存中/硬盘中)HttpSession存放在服务端计算机内存中
数据类型不同:Cookie对象存储的共享的数据类型只能是String类型,HttpSession对象可以存储任意类型的共享数据Object
数据数量:个Cookie对象只能存储一个共享数据类型
HttpSession使用map集合存储共享数据,所以可以存储任意数据量的共享数据
参照物不同Cookie相当于在服务端会员卡,HttpSession相当于客户再服务端的私人保险柜
实现命令:同一个网站下oneServlet将数据传递twoServlet
oneServlet{

```java
public void doGet(HttpServletRequest request,HttpServletResponse response){
//1.嗲用请求对象想Tomcat索要当前用户在服务的私人储物柜
HttpSession  session= request.getSession();
//2.将数据添加到私人储物柜中
Session.setAttribute("key1",共享数据);
}
```

}
浏览器访问/myweb网站twoServlet
TwoServlet{

```java
public void doGet(HttpServletRequest request,HttpServletResponse response){
//1.通过请求对象索要当前用户在服务端的这个私人储物柜
//2.从会话作用域对象得到oneServlet提供的共享数据,
Object 共享数据=session.getAttribute("key1");
}
```

}
HttpSession的应用场景:网上购物,将oneservlet网站的商品添加到购物车,然后将购物车推送到twoservlet进行结算,
在界面的添加到购物车,是一个超链接,超链接对应的Servlet将商品添加到HttpSession的私人储物柜,在同一个页面有一个查看购物车的超链接
对应的servlet是接收这个私人储物柜,在这个servlet接收完私人储物柜后,在twoservlet中打开将私人储物柜里添加的商品通过out.print方法展示给客户端,实现查看购物车的功能
商品   价格   购物车
牙膏   12   加入购物车
查看购物车
加入购物车的超链接(要有提交的商品名称)    <a href="/myweb/onservlet?goodname="牙膏">加入购物车
查看购物车的超链接:<a href="/myweb/twoservlet>查看购物车
[//1.调用请求对象](http://1.xn--cetv29aivgltyxby6a),获取请求参数,得到用户选择商品名
String goodsname=request.getParameter("goodname");
[//2.调用请求对象](http://2.xn--cetv29aivgltyxby6a),向tomcat搜药当前用户在服务端的私人储物柜

```java
HttpSession session=request.getSession();
oneservlet的编写:将数据放入私人存储柜
Integer goodsNum=(Integer)session.getAttribute(goodsName);
//如果储物柜里没有商品就添加,如果储物柜里有商品数目加1
if(goodsNum==null){
session.setAttribute(goodsName,1);  //将一件商品添加到储物柜
}else{
session.setAttribute(goodsName,goodsNumber+1);
}
```



twoservlet调用请求对象.向Tomcat索要当前用户在服务端私人储物柜

```java
HttpSession session=request.getSession();
//将session中所有的key读取出来,存放在一个枚举对象中,这个储物柜里存放的key是Names,所以是session.getAttributeNames()
Enumeration goodsNames=session.getAttributeNames();
//迭代器遍历储物柜里的所有key
while(goodsName.hasMoreElement()){
String goodsName=(String)goodsNames.nextElement();
int goodsNUm=(int) session.getAttribute(goodsName);  //是从key中拿值,所以不存在空指针异常,可以用int
System.out.println("商品名称"+goodsName+"商品数量"+goodsNum);
}
```

用户如何访问的是自己的储物柜,也就是Http服务器如何将用户与HttpSession关联起来
Http服务器创建一个HttpSession对象的时候,自动为这个HttpSession对象生成一个唯一的编号 ,tomcat将编号保存到COOkie中,推送到浏览器的缓存中,
此时Cookie的键值对是JESSIONId=110620,将保存标号的Cookie返回到响应协议包中,用户daunt获取到Cookie里的标号,当用户第二次访问时,Tomcat根据请求头JSESSONIS确认
用户是否有HttpSession以及哪一个HttpSession是当前用户
getSession()与getSession(false)两个方法的区别
相同:都是向tomcat索要私人储物柜,
gteSession方法
如果当前用户已经拥有了我们的私人储物柜,要求tomcat将私人储物柜进行返回,如果当前用户在服务端尚未拥有自己的私人储物柜,要求tomcat为当前用户创建一个全新的私人储物柜
getSession(false)方法
如果当前用户已经拥有了我们的私人储物柜,要求tomcat将私人储物柜进行返回,如果当前用户在服务端尚未拥有自己的私人储物柜,此时tomcat将返回一个null,不为它创建死人储物柜
当用户是通过登录验证的方式进入服务器时,采用getSession方法,为用户提供最好的方法,当用户是不合法的方式进入时,采用getSession的方法,不为用户创建私人储物柜
HttpSession销毁时机:
1.用户与HttpSession关联时使用的Cookie只能存放在浏览器缓存中
2.在浏览器关闭时,意味着用户与他的HttpSession关系被切断+
3.由于tomcat无法监测浏览器何时关闭,因此在浏览器关闭时,并不会导致Tomcat将浏览器关联的HttpSession进行销毁
4.为了解决这个问题,Tomcat为每一个HttpSession对象设置一个空闲时间,这个空闲时间默认30分钟,如果当前HttpSession对象的空闲时间达到30分钟
此时tomcat认为用户已经放弃了自己的HttpSession,此时Tomcat就会销毁掉这个HttpSession
30分钟内HttpSession将占用内存,为了防止内存在不使用的情况下白白占用,需要手动设置缩短空闲时间
7.HttpSession空闲时间的手动设置,将空闲时间改为5分钟
在当前网站/web/WEB-INF/web.xml

## 4.application

绍:来自于Servlet规范中一个接口,在Tomcat中存在servlet-api.jar,在tomcat中负责提供这个接口的实现类

 如果连个Servlet来自于同一个网站,彼此之间通过网站的 ServletContext实例对象实现数据共享

  ServletContext对象被称为全局作用域对象,

 工作原理:每一个网站都存在一个全局作用域对象servletContext对象,servletContext对象相当于一个Map集合以key-value的形式存储,onservlet发送的共享的数据是key

 twoservlet可以从全局作用域获取value,  实现onservlet和twoservlet之间的数据共享

  全局作用域对象的生命周期:在http服务器在启动的过程中,自动为当前的网站在内存中创建一个全局作用域对象

​                       在http服务器运行期间时,一个网站只有一个全局作用域对象

​                       在http服务器运行期间,全局作用域对象一直处于存活状态

​                       在http服务器准备关闭时,负责将当前网站中全局作用域对象进行销毁处理,全局作用域对象贯穿网站的整个运行期间

  相关的命令:同一个网站中oneservlet将数据共享给twoservlet:

​     在oneservlet当中:

​           

```java
 //1.通过请求对象向Tomcat索要当前网站中的全局作用域对象
           ServletContext application= request.getSercletContxet();
           //2.将数据添加到全局作用域对象中(当作共享数据),setAttribute就是Map类型的属性
           application.setAttribute(" key1",共享数据);
     在twoservlet当中:
            //1.当过请求对象想tomcat索要当前网站中的全局作用域对象
            ServletContext application=request.getServletContext();
            //2.从全局变量作用域对象得到指定关键字对应的数据(根据类型需要进行类型强转)如获得Integer类型
         Integer obj= (Integer)application.getAttribute("key1");
```

## 5.cookie

Coookie本质是服务器中的Map集合



方式二:Cookie类:

介绍:

来自于servlet规范中的工具类,存在于tomcat提供的servlet-api.jar包中

两个servlet来自于同一个网站,并且为同一个浏览器/用户提供服务器,此时借助于Cookie对象进行数据共享

Cookie存放当前用户的私人数据,在共享数据过程中提高服务质量

在现实生活场景中,Cookie相当于用户在服务端得到的会员卡

oneservlet在为客户服务过程中,onervlet生成一个cookie存储与当前用户相关的数据,oneservlet工作完毕后,将cookie写入到响应头中,交还给当前浏览器

浏览器接收到响应包之后将cookie存储在浏览器的缓存,一段时间之后,用户通过同一个浏览器再次向myweb网站,发送请求申请twoservlet时

浏览器,需要无条件将web网站之前已送过来的cookie无条件写入到请求头中发送过去,此时twoservlet在运行时,就可以通过读取请求头中cookie信息

得到oneservlet提供的共享数据

并且携带cook访问twoservlet,twoservlet

3.具体的实现命令

 同一个网站的oneservlet与twoservlet借助于cookie来实现数据的共享方案,

 在oneservlet中:

 //1.创建一个cookie对象,保存共享数据(当前用户的数据)如订餐会员卡开卡,Cookie存放会员卡的姓名和预存金额,需要两个Cookie对象,用来分别存放姓名和预存金额

 Cookie card=new Cookie();//cookie相当于一个map,一个cookie只能存放一个键值对,里面的key-value都是String且key不能是中文

 //2.将cookie写入到响应头,将给浏览器

 response.addCookie(card);

 在twoservlet里

 //1.调用请求对象从请求头中得到浏览器返回的Cookie

 cookie cookieArray[]=request.getCookies()

//2.循环遍历数组,得到每一个cookie的key与value

```java
for(ccokie card:cookieArray){
string key=card.getName();//    读取key
string value=card.getvalue();//读取value(取值)
}
```

Cookie应用实例:订餐会员卡

index.html:新用户申请会员卡通道 用户名,和预存金额界面`

给服务端端发送请求,onservlet开卡1.调用请求对象获取请求头的参数信息 2.创建两个Cookie对象,一个存放用户名称,一个存放预存金额,

3.将两个Cookie对象写入到响应头中4.将点餐页面写入到响应体中,响应包既有点餐页面也有存储的Cookie,保存的Cooie放入到请求协议包中再推送给twoservlet

twoservlet获取请求头部,得到用户点餐数据类型,2.获取请求头中的Cookie,得到用户会员卡,根据事务类型进行清理,根据Cookie交还给用户,并将清理,并将消费记录写入到响应体中

通知tomcat将点餐页面内容写入到响应体交给浏览器,用请求转发的方式request.getRequestDispatchPather("订餐界面.html").forword(request,response);

twoservlet读取请求头中的Cookie信息

cookieArray=request.getCookies("key",value);

//加强for循环获取cookie里存放的共享数据

```java
for(Cookie card:cookieArray){
String key=card.getName();
String value=card.getvalue();
if("username".equals(key)){
userName=value;
}else if("money".equals(key)){
money=Integer.valueof(value);//字符串转int型
}
}
}
```

当给twoservlet的cookie数据发生变化时,如会员卡的预存金额刷卡购物之后发生变化Cookie newCard=new Cookie("money",money-price+"");

第一个参数是cookie的key,第二个参数是对应的value值 ,cookie的value是字符串,而进行运算是int型所以要将运算后的结果进行字符串拼接,使之重新成为字符串

将修改后的cookie返还给用户,

response.addCookie(newCard);

cookie的生命周期

在默认的请款改下cooie情况下,cookie对象存放在浏览器的缓存中,浏览器关闭则cookie关闭

在手动的设置情况下,可以要求浏览器将接收的cookie存放在客户端计算机的硬盘上,同时需要指定cookie在硬盘上的存活时间,自指定时间的范围里,关闭浏览器,关闭客户端计算机,关闭服务器

都不会导致Cookie被销毁在存活时间到达时,cookie自动从硬盘上被删除

手动设置将接收到的cookie对象放在客户端计算机的硬盘上:

cookie.setMaxAge(60);                 设置cookie在硬盘上存放的时间是60秒