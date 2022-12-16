1.jquery介绍

JQuery是一款跨主流浏览器的JavaScript库封装了JavaScript相关方法的调用,简化了JavaScript对HTML的DOM操作

JQuery用于帮助操作DOM对象的document,JQuery本质是由封装好的多个函数的javascript代码

```javascript
function  fun1(){
        //通过文本的id获取它对应的DOM对象的value值
        var value1=document.getElementById("txt1").value;
        alert(value1)
    }
    function fun2() {
        var  value2=document.getElementById("txt2").value;
        alert(value2);
    }
    如上:获取指定id的dom对象和dom对象的value值,代码很长且重复,不利于开发,可以将获取DOM对象的重复代码封装成一个方法
    ,并且用id做为形参,可以改写为以下的形式


 //自定义方法获取js中的dom对象
    function  getDomobj(domid){
        //获取指定id的dom对象
        var obj=document.getElementById(domid);
        //返回dom对象
        return obj;
    }

    function fun1(){
    //调用getDomobj的方法获取指定id的dom对象
    var domobj1=getDomobj("txt1");
    //输出dom对象的id
    alert(domobj1.value);
    }

    
    function fun2(){
    //调用getDomobj的方法获取指定id的dom对象
    var domobj2=getDomobj("txt2");
    //输出dom对象的id
    alert(domobj2.value);
    }
```

# 2.jquery的理解

jQuery是js库

库:相当于java的工具类,库是用来存放东西的,jQuery是用来存放javascript代码的地方,存放的是用js代码写的函数function

目的:简化对javaScript对于DOM的简化操作



jQuery的优点:

1.写少的代码,做更多的事

2.免费,开源,轻量级的js库,容量很小

3.兼容市面上的主流浏览器

# 3.jquery的初步使用

```javascript
<head>
    <meta charset="UTF-8">
    <title>通过jQuery简化javaScript代码</title>
    <!--通过src指定jQuery文件的位置,使用相对路径,导入js代码库-->
    <script type="text/javascript" src="jquery-3.6.0.js"></script>
    <!--使用jQuery编辑js代码-->
        <script type="text/javascript">
            /*
            1. $(document),$是jQuery中的函数名称,document是函数的参数
            作用是把document对象变成jQuery可以使用的对象
            2.ready是jQuery中的函数,准备的意思,当页面的dom对象加载成功后,会执行ready函数的内容
            ,ready相当于js中的onload事件
            3.function是自定义函数,表示onload后要执行的功能
             */
            $(document).ready(function (){           
                //自定义的功能代码
                alert("你好");
            })
	    //可以简写成(常用)
	    $(function{
            alert("你好");
	    }
	    )

    </script>
</head>
```

# 4.jquery对象和DOM对象

DOM对象:使用javaScript的语法创建的对象,也就是js对象var obj=document.getElementById("txt1"); obj就是一个DOM对象也叫js对象

jquery对象,使用jquery语法对象叫做jquery对象,注意:jquery表示的对象都是数组

例如:var jqueryobj=$("#txt1"),jqueryobj就是使用jquery语法表示的对象,也就是jquery对象,他是一个数组,现在数组就一个值,数组

中存放的是dom对象



dom对象可以和jquery对象相互转换

dom对象转换成jquery,语法:$(dom对象)

jquery对象可以转换成dom对象  从数组中获取第一个对象,第一个对象就是dom对象,使用[0]或者get(0)



为什么要进行domn和jquery的转换,目的是要使用对象的属性,或者方法,是dom对象时,可以调用dom对象的属性和方法,

如果要是用jquery提供的属性和方法,必须时使用jquery对象,将dom对象转换成jquery对象





为了jquery对象和dom对象区别开,jquery对象习惯性以$开头,但这不是必须的

## 1.jquery对象转DOM对象

jquery对象的本质是装有DOM对象的数组,获取jquery对象的数组的第一个就是jquery对象,

```javascript
function  btnclick(){
    //从jQuery对象数组中,第0个对象为DOM对象
    var domobji=$("#txt")[0];
    // 或者可以写成var domobji=$("#txt").get(0);
   alert(domobji.value())
}
```

## 2.DOM对象转jquery对象

```javascript
  function  btnonclick(){
      //获取dom对象
      var domobj=document.getElementById("button1");
      //将dom对象转换成jquery对象
      var jqueryobj=$(domobj);
      alert(jqueryobj.val())
  }
```



jquery中的val()函数相当就是输出value值

# 5.jquery对象中的选择器

选择器:就是一个字符串,用来定位DOM对象的,定位了DOM对象,就可以通过jquery的函数操作dom

常用的选择器有:

1.id选择器.语法:$("#dom对象的id值")          通过dom对象的id定位dom对象,id在当前页面是唯一值

2.class选择器,语法:$(".class的样式名)        class表示css中的样式,通过dom对象的class值获取它对应的dom对象

3.标签选择器,语法:$("标签名称")              标签名称就是标签,使用标签的名称定位到dom对象比如说获取所有div或者text的标签的dom对象,如果有多个这种标签,此时jquery是一个数组

xxxxxxxxxx     public PreparedStatement createStatement(String sql, HttpServletRequest request){        try {            ps=getConn(request).prepareStatement(sql);        } catch (SQLException e) {            e.printStackTrace();        }           return  ps;    }​    public  void close(HttpServletRequest request){            if(ps!=null){                try {                    ps.close();                } catch (SQLException e) {                    e.printStackTrace();                }            }            ServletContext application=request.getServletContext();            //获取全局作用域对象,将conn放入map集合中            Map map= (Map) application.getAttribute("key1");            //当conn不再使用时,将conn归还到存放那20个conn的数据库连接池           map.put(conn,true);    }java

5.组合选择器,语法:$("#id,.class,标签名")     多种选择器可以混合使用,获取所有id,样式,标签名的dom对象

6.表单选择器,语法:$(":text")                 获取表单中所有type=text(单行文本框)的dom对象,根据type的值获取指定的dom对象(表单选择器和有没有form标签无关)

分别通过div的id和css样式获取对应的jquery的对象

```javascript
<script type="text/javascript" src="jquery-3.6.0.js"></script>
<script type="text/javascript">
// 通过id名来获取对应的div的jquery对象
var  jqueryobj1=$("#one");
//通过class的值获取对应的div的jquery对象
var  jqueryobj2=$(".two")
</script>
<div id="one">
  我是id为one的div
</div>
<div class="two">
    我是样式名class为two的div
</div>
```





注意:当jquery对象中有多个DOM对象时,如果要调用每个DOM对象的属性和函数,因为jquery是以数组的形式存储dom对象

所以遍历jquery的dom对象,一次将他们转化成DOM对象,并用一个数组接收,再去调用dom对象的属性和方法,如果是要使用jquery

对象去调用属性和函数,需要将dom对象转换成jquery对象取调用属性和函数

# 6.过滤器过滤DOM对象

jQuery对象中存储的DOM对象顺序与页面标签声明位置关系

```html


<div>1</div>    dom1

<div>2</div>    dom2

<div>3</div>    dom3

<div>4</div>    dom4

$("div")=[dom1,dom2,dom3]
```

过滤器就是过滤条件,对已经定位到数组中DOM对象进行帅选,

,设置过滤条件筛选出自己需要的DOM对象,过滤条件不能独立出现在jquery函数,如果使用只能出现在选择器后方

过滤器有时是一个字符串,用来筛选过滤对象的过滤器不能单独使用,必须和选择器一起使用







*基本过滤器:根据dom对象数组的位置进行筛选

1.选择第一个first,保留数组中第一个DOM对象------------>语法:$("选择器:first")

 例如:$("div:first")保留数组中的第一个DOM对象,也就是dom1

2.选择最后个last,保留数组中最后DOM对象--------------->语法:$("选择器:last")

 例如:$("div:last")保留数组中的最后一个DOM对象,也就是dom4

3.选择数组中指定缩影对象------------>语法:$("选择器:eq(数组索引)")

 例如:$("div:eq(2)")保留数组中第三个DOM对象

4.选择数组中大于指定索引的所有DOM对象-------------->语法:$("选择器:gt(数组索引)")

例如:$("div:gt(1)") 保留dom3和dom4对象

5.选择数组中小于指定索引的所有DOM对象-------------->语法:$("选择器:lt(数组索引)")

例如:$("div:gt(2)") 保留dom1和dom2对象   

# 7.jquery中的on绑定事件

绑定监听器的方法:

方法一:$("#btn").click(function(){

   ...要执行函数的内容

})

方法二:$("#btn").click(add):绑定监听器并执行add函数

方法三:通过jQuery中的on函数绑定监听器

on()绑定事件

on()方法被选元素上添加事件处理程序,该方法给API带来很多的遍历,推荐使用该方法

语法:$(选择器).on("event",function)

event:事件名称一个或者多个事件,多个之间空格分隔,就是js中事件去掉on的部分,例如js中onclick事件,事件名称就是click

function:可选,规定当事件发生时执行函数,也可以是函数名,响应事件执行对应函数名的函数

代码举例:



 

```javascript
<script type="text/javascript" src="jquery-3.6.0.js" ></script>
    <script type="text/javascript">
    
        $(function(){
   
            $("#Ni").on("click",function (){
                alert("你好啊");
            })
        })
    </script>
</head>
<body>
<input type="button" value="surpise1" id="Ni"/>
<input type="button" value="suprise2" id="hi"/>
```

# 8.表单属性过滤器

表单属性过滤器:根据表单中dom对象的状态情况,定位dom对象

启用状态:enabled

不可用状态:disabled

选中状态:checked,例如radio(单选框)和checkbox(复选框)



1.选择可用的文本框:

$(":text:enabled")

2.选择不可用的文本框:

$(":text:disabled")

3.复选框选中的元素:

$(":checkbox:checked")

4.选择指定下拉列表的被选中元素

选择器>option:selected    例如:$("select>option:selected");





文本框的可用与不可用的说明:

 当文本框中的disabled属性为true时,文本框为可用文本框<input type="text" disabled="true"/>默认是可用的文本框

 当文本框的disabled属性为false时,文本框文为不可用文本框<input type="text" disabled="fasle"/>





```javascript
<script type="text/javascript" src="jquery-3.6.0.js"></script>
<script type="text/javascript">
    function  add() {
        $(function () {
            // 获取有用的文本框的jquery对象
            var jqueryobj1=$(":text:enabled");
            //通过遍历将jquery对象转换成dom对象
            for (var i=0;i<jqueryobj.length;i++){
               var domobj=jqueryobj[i];
               alert(domobj.value);
            }
        })
    }
    function  del(){
        $(function (){
            // 获取没用的文本框的jquery对象
            var jqueryobj2=$(":text:disabled");
            //通过遍历将jquery对象转换成dom对象
            for (var i=0;i<jqueryobj2.length;i++){
                var domobj=jqueryobj2[i];
                alert(domobj.value);
            }
        })
    }
    //获取复选框选中的元素
      function  cd(){
        $(function (){
            var jquery3=$(":checkbox:checked");
           for (var i=0;i<jquery3.length;i++){
               alert(jquery3[i].value)
           }
        })
      }
      // 获取下拉列表框选中的元素
     function  ni(){
        $(function (){
            //下拉列表的jquery对象
           var jqueryobj4=$("select>option:selected");
           //因为下拉列表只能选中一个,所以可以直接通过jquery对象调用属性和函数
           alert(jqueryobj4.val());
        })
     }

</script>
</body>
<!--获取有用或没用的文本框-->
<input type="text" id="txt2"value="text1"/><br/>
<input type="text" id="txt1" value="text2"/><br/>
<input type="text" id="txt3" value="text3"/><br/>
<input type="button" value="获取可用的文本框" onclick="add()"/>
<input type="button" value="获取不可用的文本框" onclick="del()"/>
<input type="button" value="获取复选框选中的元素" onclick="cd()"/>
<input type="checkbox" value="下棋"/>
<input type="checkbox" value="游泳"/>
<input type="checkbox" value="爬山"/>
<select id="java">
     <option value="java">java语言</option>
    <option value="go">go语言</option>
    <option value="c">c语言</option>
</select>
<input type="button" value="获取下拉列表的元素" onclick="ni()">
```

# 9.jquery中的函数

1.val:操作数组中DOM对象的value属性

​    $(选择器).val():无参调用形式,读取数组中第一个DOM对象的value属性值

​    $(选择器).val(值):有参形式调用,对数组中所有DOM对象的value属性值进行统一赋值



2.text:操作数组中所有DOM对象的"文字显示内容属性"

​     $(选择器).text():无参调用形式,读取数组中所有DOM对象的文字显示内容,将得到的内容拼接成一个字符串返回

​     $(选择器).text(值):有参形式.对数组中所有DOM对象的文字显示内容进行统一赋值



3.attr:对val和text之外的其他属性进行操作,可以更改任意属性的值,例如通过修改src的值改变图片

​     $(选择器).attr("属性名"):获取DOM数组中的第一个对象的属性值

​     $(选择器).attr("属性名","值"):对数组中所有的DOM对象的属性设为新值



4.remove:将数组中所有DOM对象及其子对象一并删除(下拉列表中select是父标签,option是子标签)

​     $(选择器).remove()



5.empty将数组中所有DOM对象的子对象(只是删除子对象)

​     $(选择器).empty()



6.append:为数组中所有DOM对象添加子对象

​     $(选择器).append("<div>我要添加的子对象<div/>")  将这个div对象,作为子对象添加



7.html:设置或返回被选的元素的内容(innerHTML)    innerHTML:用于将文本和代码输入的方法

​     $(选择器).html():无参数调用方法,获取DOM数组的第一个DOM对象的文本内容,如果文本里有代码,获取的内容的也会包括代码

​     $(选择器).html(值):有参数调用,用设置DOM数组中所有元素的内容



```javascript
<script type="text/javascript" src="jquery-3.6.0.js "></script>
<script type="text/javascript">
    //jquery中val函数的使用
    $(function (){
        $("#ou").click(function (){
            // 获取文本框的jquery对象
            var jqueryobj1=$(":text");
            // 获取文本框的value并输出
            alert(jqueryobj1.val());
            //将文本框的value更改为hello
            jqueryobj1.val("hello");
        })
        //jquery中text函数的使用
        $("#you").click(function (){

            // 通过标签选择器获取所有div的jquery对象
            var jqueryobi2=$("div");
            // 获取所有div中的文本内容,拼接成一条字符串返回
            alert(jqueryobi2.text());
            //设置div的文本内容
            $("div").text("设置的div的text值");
        })
        //jquery中attr函数的使用
        $("#bin").click(function (){
            // 获取指定的jquery对象
            var jqueryobj3=$(":checkbox");
            //获取DOM对象的name属性的值并输出
            alert(jqueryobj3.attr("name"));
            //将获取到的DOM对象中的name属性值设为hello
            jqueryobj3.attr("name","hello");
        })
        //jquery中remov函数的使用
    $("#deleteall").click(function (){
        //获取指定的jquery对象
        var jqueryobj5=$("#hj");
        //remove函数清空父对象和所有的子对象
        jqueryobj5.remove();
    })
        //jquery中empty函数的使用
        $("#deleteone").click(function (){
            //通过id选择器获取它对应的jquery对象
            var jquery6=$("#hj");
            // 清除jquery所有的子对象
            jquery6.empty();
        })

        // jquery中append函数的使用
        $("#di").click(function (){
            //通过选择器获取指定的jquery对象
            var jqueryobj7=$("#hj");
            //给这个jquery添加一个子对象
            jqueryobj7.append("<option>爬山</option>");
        })

        // jquery中html函数的使用
        $("#three").click(function (){
            // 获取jquery对象
            var jqueryobj8=$("#two");
          alert(jqueryobj8.html());
        })
    })
</script>
<body>
<input type="text" value="你好"/>
<input type="button" value="val获取和改变文本框中的value值" id="ou"/>
<div>滚蛋,</div>
<div>请走开</div>
<input type="button" value="获取所有div中的文本内容" id="you"/>
<input type="checkbox" name="你好"/>
<input type="button" value="读取或修改标签中属性的值" id="bin"/>
<!--下拉列表的select是父对象,option是子对象-->
<select id="hj">
    <option>下棋</option>
    <option>游泳</option>
</select>
<input type="button" value="将父对象及子对象一并删除" id="deleteall"/>
<input type="button" value="只删除子对象" id="deleteone"/>
<input type="button" value="将一个标签作为子标签添加到另一个标签中" id="di"/>
<span id="two">你好<p>数据库</p></span>
<input type="button" value="jquery对象数组第一个元素的内容" id="three"/>
```

jquery中的css函数可以通过样式名选择器和id选择器来设置或者更改对应的css样式,实现在javascript脚本块中更改css样式



```javascript
<script type="text/javascript" src="jquery-3.6.0.js"></script>
<script type="text/javascript">
    function  change(){
        var jqueryobj=$(".div");
        jqueryobj.css("background","blue");
    }
</script>
<div class="one">
  你好
</div>
<input type="button" value="改变div的css样式" onclick="change()"/>

//标签选择器表示获取所有标签为div的dom对象
var alldivobj=$("div");
```

# 10.each函数实现遍历

each是对数组,json和dom数组等遍历,数组中的每一个元素都会执行一次自己定义的函数

​     语法1:$.each(要遍历的对象,function(index,element){处理程序}}

​     语法2:jQuery对象.each(function(index,element){处理程序}}

​     index,element都是自定义的形参,名称自定义

​     index:元素在数组中的角标

​     element:数组中的元素 



​     普通数组: var arr={1,2,3}

​     json数组:var json={"name":"lisi","age":20}

​     DOM数组:var obj=$(":text");



```javascript
      <script type="text/javascript"  src="jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            //each函数循环普通数组的操作
            $("#shuzu1").click(function (){
            //创建一个普通的数组
                var arr1=[1,2,3];
                $.each(arr1,function (index,element){
                    alert("数组的角标"+index+"数组成员"+element);
                })
            })
            $("#shuzu2").click(function (){
                // 创建一个json数组对象
                var jsonobj=["name":"刘勇","age":"18"];
                $.each(jsonobj,function (index,element){
                    alert("index是key"+index+"element是value值"+element);
                })
            })
            $("#shuzu3").click(function (){
              // 通过表单选择器获取type=text的jquery对象也就是DOM数组对象
               var jqueryobj8=$(":text") ;
               //遍历DOM数组
                $.each(jqueryobj8,function(index,emelent){
                    alert("index是DOM数组的角标"+index+"emelent是角标对应的DOM对象"+emelent);
                })
            })
        })
    </script>
</head>
<body>
<input type="button" value="each函数循环普通的数组" id="shuzu1"/>
<input type="button" value="json数组的循环遍历" id="shuzu2"/>
<input type="button" value="DOM数组的循环遍历" id="shuzu3"/>
<input type="text" value="你好1"/>
<input type="text" value="你好2"/>
<input type="text" value="你好3"/>
```

# 11.jquery实现局部刷新

没有jquery之前,使用XMLHttpRequest做Ajax,有四个步骤



jquery简化Ajax请求的处理,使用三个函数可以实现ajax请求的处理,是实现局部刷

1.$.ajax():jquery中实现ajax的核心函数



3.$.post():使用post的请求方式方式做ajax请求

​     \>使用post的请求方式发送请求,参数参照$.get()的请求方式

$.post()和$.get()他们在内部都是调用的$.ajax()



一.$.ajax函数的使用:函数的参数表示请求的url,请求的方式,参数值等信息

$.ajax参数是一个json的结构

例如:$.ajax({name1:value1,name2:value2.......})    参数是json的数组,包含请求方式,数据,回调方法

参数的name值以及对应的value值可以有以下:

*name*                        *value*

   async--------------------------------> (boolean类型) true为异步(默认)false为同步

   contentType------------------------>发送到服务器 使用的内容类型如:application/json

   data--------------------------------->规定要发送到服务器的数据,多数是json

   dataType---------------------------->期望从服务器响应的数据类型xml,html,text,json(json对象)

   error--------------------------------->如果请求失败要运行的函数

   success(resp)------------------------>请求成功时运行的函数,resp自定义形参名

   type--------------------------------->规定请求方式get或者post不用区分大小写

   url----------------------------------->规定发送请求的URL



/*
    1.async属性true设置为异步对象,因为默认是true所以可以不用写
    2.contentType属性设置请求的参数的格式json格式,写成application/json
    3.data表示向服务器发送请求的请求参数大都是以json形式的格式,如下
    4.dataType表示期望从服务器中返回的数据格式,可选的有,xml,html,text,json,当我们使用$.ajax()发送请求时,会把dataType的值
     发送给服务器,那我们的servlet能够读取到dataType的值,就知道你的浏览器需要的是json或者xml数据,那么服务器就可以返回你需要的数据格式
    5.error:一个function,表示当请求发生错误时,执行的函数
    6.success:一个function,替代if(readyState==4,status==200)表示数据接收成功,并且访问成功,函数里执行旧数据替换成新数据,局部刷新
    ,自定义一个形参,data表示就是responseText是jquery处理后的数据
    7.url:"bmiajax"表示向服务器发送请求的请求地址
    8.type:请求方式,get或者post,不用区分大小写
     */

```javascript
       var url = "addstudent";
                var data = "name=" + $("#name").val() + "&password=" + $("#password").val() + "&phonenumber=" + $("#phonenumber").val() + "&yanzheng=" + $("#yangzheng").val();
                $.ajax({
                    type: "get",
                    async: false, //同步请求
                    url: url,
                    data: data,
                    dataType:"json",//接收json格式的数据
                    timeout: 1000,
                    success: function (dates) {
                        if(dates=="注册成功"){
                            $("#message").html("<font color='green' size='0.9'>注册成功</font>")
                        }else {
                            $("#message").html("<font color='red' size='0.9'>该账号已被注册过了</font>")
                        }
                        error: function(){
                        
                        }

                    },
                });
```

# 12.级联查询

级联查询:省份下拉列表选中一个省,城市的下拉列表中会有对应的城市,这种有联系的查询称为级联查询

数据库中要有两张表:一张表用来存放省的名称,另一张表用来存放所有的城市

通过省份的id来查询对应的所有城市



创建两个下拉列表,再加载所有的dom对象后$(function(){就通过异步对象,向servlet接口实现类发送请求,查询所有省份的信息,查询到的结果,装入到List集合中,将查询到list集合转换成json格式,设置响应对象的格式,用响应对象的输出流,输出这个json格式的数据,

在$.ajax方法中,将接收到的jso格式的数据,插入到<option>对象中并当做省份的select对象的

的子对象,使用append方法插入到select对象当中(append)



```javascript
$.ajax({
    url:"one",
    type:"get",
    dataType:"json",//指定获取的是json格式的数据
    success:function (data){//处理返回的数据,data是返回的json格式数
        $("#province").empty();
         $.each(data,function (index,element){//循环将查询到的省份的信息作为子对象添加到下拉列表中
           $("#province").append("<option value='"+element.id+"'>"+element.name+"<option/>");
       })
    }
})
```



给选择省份的下拉列表绑定onchange事件,下拉列表改变选中,就触发,获取下拉列表选中的value或者text

通过ajax异步对象,将获取用户选择的省份的value当做请求参数发送给服务端

,,查询根据提供的请求参数,查询对应的所有城市信息,将城市信息放入到list集合,将这个list集合转换成json格式的数据,将获取的json插入到城市的optio中,将option当做子对象插入到,城市的select当中(append)



`  *//下拉列表框,绑定onchange选中事件,当select选中的内容发生变化时,触发事件
*

```javascript
$("#province").change(function (){
    $("#city").empty();
//获取下拉列表选中的值
    var provincevalue=$("#province>option:selected").val();
  $.get({
      url:"twwo",
      data:{pid:provincevalue},
      success:function (data){
          $.each(data,function (index,element){
              $("#city").append("<option value='"+element.id+"'>"+element.city+"<option/>")
          })  ;
      }
  })
})
```