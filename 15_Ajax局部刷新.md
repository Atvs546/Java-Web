# 1.全局刷新和局部刷新

 全局刷新:整个浏览器被新的数据覆盖,在网络中传输大量的数据,浏览器需要加载渲染页面

 局部刷新:在浏览器的内部,发起请求获取数据,改变页面中的部分内容而其余的页面无需加载和渲染,网络中数据传输量少,给用户的体验更好



 Ajax的使用是用来做局部刷新的

# 2.异步对象

全局刷新:是通过表单提交给新的Servlet的实现类,在这个Servlet接口的实现类中,请求转发到到一个新的jsp文件,来完成新页面的加载

局部刷新:不仅是有一个表单,还有一个异步对象,这个一异步对象是Ajax中的一个核心对象,用这个异步对象来发送请求,来获取数据

可以创建多个异步对象,每一个异步对象都可以发送请求来获取数据 



异步对象向servlet的接口的实现类发送请求获取数据

异步对象:XMLHttpRequest

异步对象的功能:

1.在不重新加载页面的情况下更新网页

2.在页面已加载后向服务器发送请求数据



所有的浏览器都内建了异步对象,只需一段简单的javaScript就可以创建异步对象,异步对象存在于浏览器的内存中

var xmlhttp=new XMLHttpRequest();

# 3.Ajax的概念

ajax是做一种局部刷新的新方法,不是一种语言

ajax包含的技术主要有javaScript,dom,css,xml等等,核心是javascript和xml



javaScript:创建异步对象,发送请求,更新页面的dom对象,ajax的请求需要服务端的数据

xml:网络中的传输数据的格式

<数据1>宝马1<数据1>

<数据2>宝马2<数据2>

<数据3>宝马3<数据3>

# 4.异步对象的使用步骤

异步对象的步骤:

```javascript
//1.创建异步对象并绑定事件
var xmlHttp=new XMLHttpRequest();
xmlHttp.onreadystatechange=function(){
//3.判断异步对象接收处理数据完成且网络请求是成功的
if(xmlHttp.readyState==4&&state==200){
//4.获取服务端返回的数据并且进行局部更新如下将标签为name的value更新为data的value,达到局部刷新的目的
var data=xmlHttp.responseText;
document.getElementById("name").value=data;
}
}
//5.初始化异步对象,设置发送请求的请求方式,请求地址,true代表的是异步对象,默认是true
xmlHttp.open("get","loginServlet?name=zhangsan&password="123",true);
//6.向服务器发送请求
xmlHttp.send();
       }
```

# 5.全局刷新计算BMI

全局属性:将请求提交给Servlet接口的实现类或者JSP文件,界面的数据全部改变,全局刷新借助表单实现刷新,整个浏览器界面都发生变化

在jsp文件中,编写提交给servlet接口的实现类的表单fj就是servlet接口实现类的别名

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>全局刷新</title>
</head>
<body>
<p>
  全局刷新计算BMI
</p>
<form action="/myweb/fj"  method="get">
    姓名: <input type="text" name="name"><br/>
    体重(kg):<input type="text" name="weight"><br/>
    身高(m):<input type="text" name="tall"><br/>
    <input type="submit" value="提交"><br/>
</form>
</body>
</html>
```

\------------------------------------------------------------

servlet接口实现类接收用户输入的请求参数计算用户的BMI值

```java
public class pricate extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        //获取请求参数
        String name = request.getParameter("name");
        float weight = Float.parseFloat(request.getParameter("weight"));
        float tall = Float.parseFloat(request.getParameter("tall"));
        //计算BMI值,BMI=身高除以体重的平方
        float BMI = tall % (weight * weight);
        //通过BMI的数值判断你的身体情况`
       String messaage="";
       if(BMI<=18.5){
           messaage="你比较瘦";
       }else if(BMI>=18.5&&BMI<=23.9) {
           messaage="你好,你的身体很健康";
       }else  if(BMI>24&&BMI<=27){
           messaage="你的BMI指数较高,身体比较胖";
       }else {
           messaage="你的身体非常胖,请注意身体健康";
       }
      String  msg="你好"+name+"先生/女士,你的BMI的值是:"+BMI+","+messaage;
        //将查询的结果和用户的姓名写入到请求作用域对象中
        request.setAttribute("key1",name);
        request.setAttribute("key2",msg);
       //请求转发到指定的jsp文件中
        request.getRequestDispatcher("show.jsp");   
    }
}
```

-

将计算号的BMI的值反馈给用户写入到jsp文件中

```javascript
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>用户的BMI展示</title>
</head>

<body>
<!--这里用的是简化版的EL表达式-->
用户:${key1}
你的BMI值是${key2}
</body>
</html>
```

# 6.使用响应对象的输出流

 //调用响应对象response的输出流PrintWriter,输出结果

```javascript
 response.setContentType("text/html;charset=utf-8");
        //获取响应流的输出对象,
        PrintWriter pw=response.getWriter();
        pw.print(name+"先生/女士你好你的BMI是:"+BMI+messaage);
        //清空缓存
        pw.flush();
```

这种输出方式也是全局刷新

# 7.Ajax局部刷新实现计算BMI

大体步骤: 一设计用户输入的界面不需要form表单和submit提交按钮只需要button按钮上绑定单击事件按钮调用执行局部刷新的方法

​          二.在该方法中,创建异步对象,异步对象绑定onreadystatechange事件

​          三.获取用户输入的value值,将他们拼接成name1=value1&name2=value2的形式,也就是请求参数的形式

​	  四.调用异步对象的open对象设置请求方式,请求协议包(请求协议包=访问的地址(servlet接口实现类在webinfo中的别名)+请求参数(

​	     之前拼接成的name1=value1&name2=value2的形式的字符串),向指定的Servlet接口实现类发送请求,

​	      调用异步对象的send方法代替浏览器向服务器发送请求

​	  五.创建Servlet接口的实现类,获取请求参数,进行想要的数据处理,设置响应对象response的的格式和编码字符集,调用response的

​	     输出流输出处理结果

​	  六.通过异步对象的属性readyState和Staus判断是否接收成功和网络是否访问成功,然后用 var data=xmlHttp.responseText获取

​	      Servlet中输出流的输出结果

​	  七.将获取的新数据data写入到id名为mydata的div界面中,达到局部更新界面的目的,innerText表示以纯文本的形式添加到div界面

​	     中,inner表示以java代码和文本的形式添加到div界面中

​              document.getElementById("mydata").innerText=data;





特别注意:异步对象调用open方法代替浏览器向服务器发送请求时,xmlHttp.open(get,"onservlet?name1=value1&name2=value2",true)

get代表发送的请求方式是get

请求地址和请求参数是:onservlet?name1=value1&name2=value2     其中name1=value1&name2=value2 是由用户提交的请求参数名和请求参数值拼接而成的

true代表的是异步对象,false代表的同步对象,这里如果不填默认是true,默认的异步对象





一.新建一个jsp文件,使用XMLHttpRequest异步对象

使用异步对象的四个步骤

1.创建异步对象

2.绑定事件

3.初始化请求

4.向Servlet发送请求

```javascript
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Ajax局部刷新</title>
    <!--使用脚本并设置格式是文本格式和javaScript的格式-->
    <script type="text/javascript">
    //使用内存中的异步对象,代替浏览器发起请求,异步对象使js创建和管理
    function  doAjax() {
        //1.创建异步对象(异步对象发起请求,接收数据,可以有多个异步对象)
        var xmlHttp = new XMLHttpRequest();
        //2.绑定事件
        xmlHttp.onreadystatechange = function () {
            //readyState==4从servlet接口实现类中拿到了数据,status==200代表请求发起成功
           if(xmlHttp.readyState==4&&xmlHttp.status==200){
           //获取局部刷新更新的数据,
               var data=xmlHttp.responseText;
           //将获取的新数据data写入到id名为mydata的div界面中,达到局部更新界面的目的
               document.getElementById("mydata").innerText=data;
           }
        }
        //获取dom对象的value值
        var name=document.getElementById("name").value;
        var weight=document.getElementById("w").value;
        var tall=document.getElementById("h").value;
        //将value拼接成get请求方式的请求形式name=value
        //n=name&w=weight&h=tall-
        var param="n="+name+"&w="+weight+"&h="+tall

        /*3.初始化请求数据:get的请求方式,bmiAjax是访问地址,param就是访问的请求参数,所以 "bmiAjax?"+param就是访问的
        整个地址栏的内容,true代表的是使用异步的方式             
         */
        xmlHttp.open("get", "bmiAjax?"+param, true);
        //4.发起请求
        xmlHttp.send();
    }
    </script>
</head>
<body>
<p>Ajax局部刷新计算BMI</p>
<div>
    <!--局部刷新不需要提交,所以局部刷新既不需要form表单也不需要提交按钮submit但是实现功能的按钮button上要绑定按钮单击事件,实现局部刷新-->
    姓名:<input type="text" id="n" /><br/>
    体重(kg)<input type="text" id="w"/><br/>
    身高(m)<input type="text" id="h"/><br/>
    <input type="button" value="计算BMI" onclick="doAjax()">
</div>
<!--将局部刷新的更新的数据加载到此div中-->
<div id="mydata">等待加载数据.....</div>
</body>
</html>
```



二.创建服务器的Servlet,接收并处理数据,把数据输出给异步对象

使用HttpServletResponse输出数据

```java
public class pricate extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        //接收从异步对象发送过来的访问的请求参数
        String name=request.getParameter("n");
         float weight= Float.parseFloat(request.getParameter("w"));
         float tall= Float.parseFloat(request.getParameter("h"));
        float BMI = tall % (weight * weight);
        //通过BMI的数值判断你的身体情况`
        String messaage="";
        if(BMI<=18.5){
            messaage="你比较瘦";
        }else if(BMI>=18.5&&BMI<=23.9) {
            messaage="你好,你的身体很健康";
        }else  if(BMI>24&&BMI<=27){
            messaage="你的BMI指数较高,身体比较胖";
        }else {
            messaage="你的身体非常胖,请注意身体健康";
        }
        String  msg="你好"+name+"先生/女士,你的BMI的值是:"+BMI+","+messaage;
         //设置响应对象输出的格式
        response.setContentType("text/html;charset=utf-8");
       //调用响应对象的输出流
        PrintWriter pr=response.getWriter();
        pr.print(messaage);
        //清空缓存
        pr.flush();
    }
}
```

# 8.Ajax通过数据库完成局部刷新

需求:根据省份id查询省份的名称

是要从数据库中获取省份id对应的省份名称

1.创建一个存储省份id和省份名称的数据库,并设值一个主键和自增的字段

2.设计界面

3.脚本块中获取用户输入的value并采用异步对象代替浏览器向服务器发送请求

4.servlet接口实现类中执行数据库查询操作,查询用户输入的省份id对应的省份名称,调用响应对象的输出流,输出

5.判断异步对象是否接收成功,和访问成功,获取servlet接口实现类输出的数据,调用document将这个数据用innerText方法写入到指定的div界面

中,达到局部更新的作用







在jsp文件中:

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>查询省份ID对应的省份名称</title>
  </head>
  <script type="text/javascript">
     function  doajax(){
       //创建异步对象
      var xmlHttp=new XMLHttpRequest();
      //异步对象绑定事件
      xmlHttp.onreadystatechange=function (){
        //判断servlet是否接收成功并且响应对象是否访问服务器成功
        if(xmlHttp.readyState==4,xmlHttp.status==200){
          //从Servlet接口类中获取输出的新数据
        var data=xmlHttp.responseText;
        //局部刷新到指定的div界面中
        document.getElementById("shuaxing").innerText=data;
        }
      }
      //获取用户输入的value值
       var Id=document.getElementById("shengid").value;
      //拼接成地址栏的请求参数的形式
       var para="shengid="+Id;
       //设值异步对象发起访问的请求方式,地址栏的格式=请求地址?+请求参数,异步
       xmlHttp.open(get,"doselect?"+para,true);
       //向服务器中指定的Servlet接口的实现类发送请求
       xmlHttp.send();
    }
  </script>
  <body>
    请输入省份的ID<input type="text" id="shengid"/>
  <input type="button" value="确定" onclick="doajax()">
  <div id="shuaxing">
    等待数据加载......
  </div>
  </body>
</html>
```



在异步对象访问的servlet接口的实现类中

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        String id = request.getParameter("shengid");
     //配置文件连接数据库
     ResourceBundle bundle=ResourceBundle.getBundle("JDBC");
     String driver=bundle.getString("driver");
     String url=bundle.getString("url");
     String user=bundle.getString("user");
     String password=bundle.getString("password");
     Connection conn = null;
     PreparedStatement ps=null ;
     ResultSet rs=null;
     try {
         //连接数据库
         conn= DriverManager.getConnection(url, user, password);
         //3、获取预编译的数据库操作对象,?是占位符
         String sql="select * from province where provice=? ";
         ps=conn.prepareStatement(sql);
         //给占位符？传值
         ps.setString(1,id);
         rs=ps.executeQuery();
         while (rs.next()){
             //接收查询到的省份的名称
            String  provicename=rs.getString("provicename");
            //设置response的格式和编码字符集
             response.setContentType("text/html;utf-8");
             //调用response的输出流
             PrintWriter pr=response.getWriter();
             //将查询到的省份名称通过输出流输出
             pr.print(provicename);
             //清空缓存
             pr.flush();
         }
     
     } catch (SQLException e) {
         e.printStackTrace();
     } finally {
         //6、释放资源
         if (ps != null) {
             try {
                 ps.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
         if (conn != null) {
             try {
                 conn.close();
             } catch (SQLException e) {
                 e.printStackTrace();
             }
         }
     }
               }
```