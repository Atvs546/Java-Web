# 1.json介绍

如果输入省份的编号,获取省份的名称,省份的简称

servlet接口实现类调用输出流输出的只能是一行数据,但是这里需要异步刷新两个需要刷新的数据,名称和简称

此时需要将数据库表的字段作为私有属性,创建一个实体类,私有化属性并且写属性的get和set方法,然后使用json实现数据格式的转换





ajax发起请求----servlet(返回一个json格式的字符串{name:"河北",jiancheng:"冀","shenghui":"石家庄"})

json分类:1.json对象,JSONobject,这种对象的格式  名称:值,也可以看做key:value格式

​         2.json数组,JSONArray,基本格式[{name:"河北",jiancheng:"冀","shenghui":"石家庄"}]数组[]中装的是json对象



为什么要使用json:

1.json格式好理解

2.json格式数据在多种语言中并,比较容易处理,使用java,javaScript读写json格式的数据比较容易

3.json格式数据占用的空间下,在网络中传输快

# 2.json常用的工具类

Gson（项目地址：https://github.com/google/gson）。Gson是目前功能最全的Json解析神器，Gson当初是为因应Google公司内部需求而由Google自行研发而来，但自从在2008年五月公开发布第一版后已被许多公司或用户应用。Gson的应用主要为toJson与fromJson两个转换函数，无依赖，不需要例外额外的jar，能够直接跑在JDK上。而在使用这种对象转换之前需先创建好对象的类型以及其成员才能成功的将JSON字符串成功转换成相对应的对象。类里面只要有get和set方法，Gson完全可以将复杂类型的json到bean或bean到json的转换，是JSON解析的神器。



FastJson（项目地址：https://github.com/alibaba/fastjson）。Fastjson是一个Java语言编写的高性能的JSON处理器,由阿里巴巴公司开发。无依赖，不需要例外额外的jar，能够直接跑在JDK上。FastJson在复杂类型的Bean转换Json上会出现一些问题，可能会出现引用的类型，导致Json转换出错，需要制定引用。FastJson采用独创的算法，将parse的速度提升到极致，超过所有json库。



Jackson（项目地址：https://github.com/FasterXML/jackson）相比json-lib框架，Jackson所依赖的jar包较少，简单易用并且性能也要相对高些。而且Jackson社区相对比较活跃，更新速度也比较快。Jackson对于复杂类型的json转换bean会出现问题，一些集合Map，List的转换出现问题。Jackson对于复杂类型的bean转换Json，转换的json格式不是标准的Json格式。



Json-lib（项目地址：http://json-lib.sourceforge.net/index.html）json-lib最开始的也是应用最广泛的json解析工具，json-lib 不好的地方确实是依赖于很多第三方包，包括commons-beanutils.jar，commons-collections-3.2.jar，commons-lang-2.6.jar，commons-logging-1.1.1.jar，ezmorph-1.0.6.jar，对于复杂类型的转换，json-lib对于json转换成bean还有缺陷，比如一个类里面会出现另一个类的list或者map集合，json-lib从json到bean的转换就会出现问题。json-lib在功能和性能上面都不能满足现在互联网化的需求。

# 3.jackjson的使用

jackjson:性能好,规范好

在使用json之前,要将需要局部刷新的对应的数据库表的字段创建一个私有化属性,构造器(空参,和全参),以及这些属性对应的get和set方法

如下:

```java
public class Province {
    private  String id;
    private  String name;
    private  String Jiancheng;
    private  String Shenghui;
    public  Province(){
        
    }
    public  Province(String id,String name,String Jiancheng,String Shenghui){
        this.id=id;
        this.name=name;
        this.Jiancheng=Jiancheng;
        this.Shenghui=name;
    }

    public String getShenghui() {
        return Shenghui;
    }

    public void setShenghui(String shenghui) {
        Shenghui = shenghui;
    }

    public String getJiancheng() {
        return Jiancheng;
    }

    public void setJiancheng(String jiancheng) {
        Jiancheng = jiancheng;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}

再创建一个java类,调用Province类中的set方法给类中的私有化属性赋值,使用jackjson


public class jsontext {
//    需要主函数
    public static void main(String[] args) {
      //使用json之前要创建一个用于存放数据库表的类私有化属性和他们的get和set方法
        //使用jackson把java对象转为json格式的字符串
      Province p=new Province();
      p.setId(1);
      p.setName("河北");
      p.setJiancheng("冀");
      p.setShenghui("石家庄");
      //使用jackson把Province的对象p转换成json
        ObjectMapper om=new ObjectMapper();
        try {
            /*writeValueAsString方法:把参数的java对象转为json格式的字符串,下面的json就是转换的json格式的字符串
            转换出来的json格式的字符串是{"id:1,"name":"河北","jiancheng":"冀","shenghui":"石家庄"}
             */
            String json= om.writeValueAsString(p);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
    }
}
```

## 1.jackjson的使用实例

当输入省份的id局部刷新获取省份的名字,省份的简称这样的多条数据时

创建一个Servlet接口的实现类,获取用户输入的请求参数,也就是用户输入的省份的id,将这个id作为数据库查询的条件

需要创建一个类的作为数据库表中字段的存储,私有化属性和他们对应set和get方法

在执行完sql语句后将查询到的省份的名称和简称,调用类中的set方法将查询到的结果赋值给类中的属性,

在之前创建的Servlet接口的实现类中,将已经存放在类的属性中多条需要更新的数据,转换成json格式的一条字符串,

*特别注意:在接收json后,要设置响应对象的返回给浏览器端的格式是json格式的数据 response.setContentType("application/json;charset=utf-8");

  调用响应对象session的输出流,将json输出,输出的数据会付给ajax,ajax接收的这个数据是转换的json数据,多条数据连接在一起

  的,需要将他们拆解,局部刷新到指定的位置上,

  拆解过程:

  //获取servlet接口实现类输出过来的json

​     var data=xmlHttp.responseText'

  1.把获得的json格式的字符串,转为json对象,json中的key就是json对象的属性名,eval方法执行括号中的代码,将json字符串转为json对象

​     var jsonobj=eval("("+data+")");

​     //将jsonobj对象的属性分别局部刷新到指定位置

​     document.getElementById("proname").value=jsonobj.name;

​     document.getElementById("jiancheng").value=json.jiancheng;

​     document.gteElementById("Shenghui").value=json.shenghui;





```java
    //创建json数据格式,{}表示json格式的数据
    String json="{}";
  //使用jackson把Province的对象p转换成json
        ObjectMapper om=new ObjectMapper();
        try {
            /*writeValueAsString方法:把参数的java对象转为json格式的字符串,下面的json就是转换的json格式的字符串
            转换出来的json格式的字符串是{"id:1,"name":"河北","jiancheng":"冀","shenghui":"石家庄"}
             */
            String json= om.writeValueAsString(p);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
```

# 4.同步和异步的区别

初始化异步对象的open方法 

xmlHttp.open("get",请求地址?请求参数名1=请求参数的值1&请求参数名2=请求参数值2,true)





true表示异步处理请求,使用异步对象发起请求后不用等待数据处理完毕,就可以执行其他的操作

在向服务器发送请求后,不需要等待服务器返回处理好的数据,可以进行执行下面的代码,提高程序运行的效率

一般开发中都是使用异步对象true

//初始化异步对象

xmlHttp.open("get",请求地址?请求参数名1=请求参数的值1&请求参数名2=请求参数值2,true);

//发送请求

xmlHttp.send();

alert("程序继续执行");



异步对象向服务器发送请求后,不需要等待服务其返回处理好的结果,程序往下执行,即使没有服务器也会输出"程序继续执行"

\------------------------------------------------------------------------------------------------------

fase:同步,send发送请求后,异步对象必须处理完成请求,从服务端获取数据后才能执行下面的代码

任意时刻只能执行一个异步请求