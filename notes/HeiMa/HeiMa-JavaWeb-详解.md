[TOC]



# HTTP

> HTTP(HyperText Transfer Protocol)，超文本传输协议，该协议规定了浏览器和服务器之间数据传输的规则。HTTP分为请求和响应两部分。

## 特点

1、Http协议基于TCP协议，面向连接，较为安全。
2、基于请求-响应模型，一次请求对应一次响应。
3、HTTP协议是无状态的协议: 对于事物处理没有记忆能力，每次请求-响应都是独立的。
4、HTTP协议的缺点是请求之间不能共享数据，优点是速度快。但是在Java中使用会话技术(Cookie、Session)来解决请求之间不能共享数据的问题。

## 请求数据格式

> HTTP中的请求部分。

请求格式举例:
                GET /HTTP/1.1
                
                Host: www.baidu.com
                User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
                Accept-Encoding: gzip, deflate, br
                Accept-Language: zh-CN,zh;q=0.9
                Cache-Control: max-age=0
                Connection: keep-alive
                ...
                
                POST /HTTP/1.1
                
                Host: www.baidu.com
                User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
                Accept-Encoding: gzip, deflate, br
                Accept-Language: zh-CN,zh;q=0.9
                Cache-Control: max-age=0
                Connection: keep-alive
                
                username=zs&password=123

HTTP请求数据分为三部分:

1、请求行: 请求数据的第一行。其中GET表示请求方式，/表示请求资源路径，HTTP/1.1表示请求协议版本。

2、请求头: 请求数据第二行开始。格式为key:value形式。
	常见的HTTP请求头有:
	2.1、Host: 表示请求的主机名
	2.2、User-Agent: 浏览器版本，例如Chrome浏览器的标识Chrome/79。
	2.3、Accept: 表示浏览器能接受的资源类型，如text/*, image/*或者*/*表示所有类型资源。
	2.4、Accept-Language: 表示浏览器偏好的语言，服务器可以据此返回不同语言的网页。
	2.5、Accept-Encoding: 表示浏览器可以支持的压缩类型，如gzip, deflate等。
	
3、请求体: POST请求的最后一部分，用来存放请求参数。
	扩展: GET和POST请求的区别:
	3.1、GET请求请求参数在请求行中，直接加在资源路径后，且无请求体。POST请求请求参数在请求体中。
	3.2、GET请求对请求参数大小有限制，而POST没有。

## 响应数据格式

> HTTP中的响应部分。

响应格式举例:
                HTTP/1.1 200 OK
                Server: Tengine
                Content-type: text/html
                Transfer-Encoding: chunked...

​                <html>
​                ...
​                </html>

**HTTP响应数据分为三部分**:

1、响应行: 响应数据的第一行。其中HTTP/1.1表示协议版本，200表示响应状态码，OK表示状态码描述。
	状态码扩展:
		

| 状态码分类 | 说明                                                         |
| :--------: | ------------------------------------------------------------ |
|    1xx     | 响应中–临时状态码，表示请求已经接收，告诉客户端应该继续请求或者如果它已经完成，则可以忽略它。 |
|    2xx     | 成功–表示请求已经被成功接收，处理已经完成。                  |
|    3xx     | 重定向–重定向到其他地方: 它让客户端再发起一个请求以完成整个处理。 |
|    4xx     | 客户端错误–处理发送错误，责任在客户端，如: 客户端请求一个不存在的资源、客户端未被授权、禁止访问等。 |
|    5xx     | 服务器端错误–处理发生错误，责任再服务端，如: 服务端抛出异常、路由出错、HTTP版本不支持等。 |

2、响应头: 第二行开始，格式为key:value。

​		常见的HTTP响应头有：

​			1、Content-Type：表示该响应内容的类型，如text/html，image/jpeg。

​			2、Content-Length：表示该响应内容的长度（字节数）。

​			3、Content-Encoding：表示该响应压缩算法，如gzip。

​			4、Cache-Control：知识客户端应如何缓存，例如max-age=300表示最多可以缓存300秒。

3、响应体: 最后一部分。存放响应数据。



**常见的响应状态码**：

| 状态码 |            英文描述             |                             解释                             |
| :----: | :-----------------------------: | :----------------------------------------------------------: |
|  200   |               OK                |                  客户端请求成功，处理成功。                  |
|  302   |              Found              | 表示请求的资源已经移动到由Location响应头所指定的URL，浏览器会自动重新访问到这个页面。 |
|  304   |          Not Modified           | 告诉客户端，请求的资源于上次请求并未更改，可以直接用本地缓存，即隐式重定向获取。 |
|  400   |           Bad Request           |           客户端请求出现语法错误，服务器无法理解。           |
|  403   |            Forbidden            |   服务器收到请求，但是拒绝服务。如：没有权限访问相关资源。   |
|  404   |            Not Found            |  请求资源不存在，一般是URL输入有误，或者网站资源已被删除。   |
|  428   |      Precodition Required       | 服务器要求请求需要带有特定的请求头，即告诉客户端如果想要访问某资源，必须携带特定的请求头。 |
|  429   |        Too Many Required        | 请求过多。可以限制客户端请求某个资源的频率，可以配合Retry-After(多尝试后可以请求)响应头使用。 |
|  431   | Request Header Fields Too Large | 请求头过大。服务器不愿意处理请求，可以再适当减少请求头域的大小后重新提交。 |
|  405   |       Method Not Allowed        |           请求方式有误。如应用GET的请求用了POST。            |
|  500   |      Internal Server Error      | 服务器出现不可预期的错误。服务器出现异常，可以查看日志纠错。 |
|  503   |       Service Unavailable       |  服务器尚未做好准备好处理请求。即服务器刚启动未初始化完成。  |
|  511   | Network Authentication Required |         客户端需要进行身份验证才能获得网络访问权限。         |

# Tomcat

> 开源的Web服务器软件之一。对HTTP协议相关操作进行了封装，让程序员无需直接面对HTTP协议进行开发，而是更专注于业务，这也让Web的开发更加便捷了。其工作原理为将我们自己写的代码、资源等部署到可访问的路径中，供浏览器进行访问。所以Tomcat只是一个软件，用来部署我们的Web项目供浏览器访问。

## 简介

Tomcat是轻量级的Web服务器，即Tomcat只支持Servlet/JSP的少量JavaEE规范（JavaEE规范是指Java企业级开发的技术规范，包含13项技术规范：JBDC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JavaMail、JAF。这些技术规范中有些已经过时，如EJB，已经被Spring替代）。

因此Tomcat也被称为Web容器、Servlet容器，Servlet需要依赖于Tomcat才能运行。

## 结构介绍

Tomcat目录介绍：

1、bin目录

> 放了一些可执行文件，一类是以bat结尾的Windows批处理文件，双击即可运行。另一类是以.sh结尾的Linux下运行文件。

2、conf目录

> 放了一些Tomcat的配置文件。

3、lib目录

> 放了一些Tomcat运行时需要使用到的jar包。其实Tomcat也是使用Java语言编写。

4、logs目录

> 放了一些日志文件，即Tomcat启动运行时产生的一些日志信息。

5、temp目录

> 放了一些临时文件，即Tomcat运行时产生的一些临时文件。

6、webapps目录

> 放的是部署到Tomcat的Web项目，里面的每一个文件都是一个Web项目。

7、work目录

> 放的是Tomcat在运行时产生了临时数据。

## 基本使用

### 下载安装

官网下载。Tomcat是绿色版，直接解压即可。

### 卸载

直接删除目录即可。

### 启动

双击bin/startup.bat启动。

### 控制台中文乱码问题

修改conf/logging.properties下的java.util.logging.ConsoleHandler.encoding = GBK即可。

### 关闭

共有三种关闭方式

1. 直接关掉运行窗口，强制关闭（不推荐，可能有数据仍未保存）
2. bin\shutdown.bat：正常关闭
3. Ctrl + c：正常关闭

### 修改启动端口号

在conf/server.xml下将<Connector port="8080”…>修改为指定端口即可。

### 常见问题

1、端口号被占用：关闭占用端口程序即可。

2、窗口一闪而过：检查JAVA_HOME环境变量是否配置成功。

### 部署

需要部署的项目放到webapps目录下即可，可以是文件夹或者其它，但文件夹传输可能效率低，所以Java一般打包成war包放到该目录下，Tomcat会自动对war包解压缩。

### IDEA集成

如果每次运行项目都要手动放到Tomcat的webapps目录下，太过麻烦。因此可以直接在IDAE中集成Tomcat，简化操作。共有两种集成方式：

1、通过IDEA配置集成（推荐）。

![image-20220729105548834](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220729105548834.png)

2、通过maven插件集成（版本只能使用到7）。

# Servlet

> Servlet是Java提供的一门动态Web资源开发技术。同样也是JavaEE规范之一，Servlet本质上就是一个接口，我们需要定义Servlet类实现Servlet接口，由Tomcat服务器来创建并调用我们的Servlet来执行相应方法。

## Servlet新版快速入门

1、创建出web项目，导入Servlet依赖坐标：

```xml
<dependency>
	<groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope> //让依赖在编译环境和测试环境生效，在运行环境无效，原因是Tomcat自带了该依赖，再引入会导致依赖冲突。
</dependency>
```

2、定义一个类，实现Servlet接口，并重写接口中所有方法，共有五个方法，如init、service、destroy等。

```java
public class ServletDemo1 implements Servlet{
    @Override
    public void service(){}
}
```

3、使用注解配置该Servlet的访问路径（通过xml也可以）

```java
@WebServlet("/demo1")
public class ServletDemo1 implements Servlet{
    @Override
    public void service(){}
}
```

4、访问该路径。首先启动tomcat（默认已集成），在浏览器中输入URL，访问该Servlet。

```
http://localhost:8080/web-demo/demo1
```

## Servlet执行流程

1、当收到第一次Servlet请求时执行init()方法，该方法会将Servlet加载进内存，该方法只会执行一次。

2、Tomcat服务器将接收到的http请求封装成一个Request对象，作为service()方法的参数传入。每次访问Servlet，service()方法就会被执行一次。

3、返回的结果封装在response对象中，Tomcat服务器会将response中的信息拆解出来，形成http响应格式，然后将这个结果返回给浏览器。

4、浏览器得到结果后会以自己能识别的格式进行解析。



## Servlet生命周期

> 即Servlet对象从被创建到销毁的整个过程

Servlet运行在Servlet容器中，其生命周期也是由容器管理，共分为四个阶段：

1、加载和实例化。默认情况下，只有当Servlet第一次被访问时，容器才会创建这个Servlet的对象。当然，可以通过@WebServlet(loadOnStartup = 1)对初始化进行修改，loadOnStartup的参数允许赋两种值：1、负整数，即第一次被访问时才创建对象，这是默认情况。2、0或正整数，服务器启动时就创建Servlet对象，整数越小，优先级越高。

2、初始化。在Servlet实例化之后，容器将调用Servlet的init()方法初始化这个对象，在Servlet对象实现init()方法时可以完成一些如加载配置文件、创建连接等初始化工作，该方法只调用一次。

3、请求处理。每次请求Servlet时，Servlet容器都会调用该Servlet的service()方法对请求进行处理。

4、服务停止。当容器服务正常关闭（非服务中断）或需要释放内存时，容器就会调用Servlet实例的destroy()方法。在该方法中可以对资源进行释放操作。在destroy()方法调用后，容器会释放这个Servlet实例，然后实例将被Java的垃圾收集器回收。

## Servlet方法介绍

> Servlet接口中定义了五个抽象方法。

1、初始化方法init()

2、提供服务方法service()

3、销毁方法destroy()

4、获取ServletConfig对象方法 ServletConfig getServletConfig()

5、获取Servlet信息方法 String getServletInfo()



## Servlet体系结构

> Servlet的顶层接口就是Servlet，其下常用的实现类有HttpServlet，这个类是对HTTP协议封装的Servlet实现类，也是我们最常用的Servlet实现类。

HttpServlet类的使用（继承HttpServlet类，重写doGet()方法和doPost()方法）：

```java
@WebServlet("demo")
class ServletDemo extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求处理
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //POST请求处理
    }
}
```

使用HttpServlet而不是Servlet实现类的原因是：

1. HTTP请求分为POST和GET，如果使用Servlet实现类，需要在service()方法中先将请求转换为HttpServletRequest，然后拿出请求方法，判断是GET还是POST，然后根据请求方式从请求行或者请求体中获取参数。单个Servlet没有问题，若有多个Servlet请求，则代码相当冗余。而HttpServlet实现了Servlet类，将这套代码进行了封装，对外提供doGet()和doPost()简化操作。



# 会话

> 用户打开浏览器，与Web服务器建立起连接，会话建立。一方断开连接，会话结束。在一次会话中，可以包含多次请求和响应。

HTTP协议是无状态的，每次浏览器向服务器发送请求时，都会被视为新的请求，有时候我们的多个请求具有连续性（如下单请求，付款请求），需要上下文的信息，而服务器并不知道，因此需要会话跟踪技术来实现会话内的数据共享。

会话跟踪的实现方式有：

1、客户端会话跟踪技术：Cookie。

2、服务端会话跟踪技术：Session。

## Cookie

> 客户端会话技术，将KV结构数据保存到客户端浏览器，浏览器每次访问该服务器时，请求中会都携带该来自该服务器的所有Cookie数据。

服务端Cookie基本使用：

发送Cookie（浏览器会自动保存起来）：

1、创建Cookie对象，设置数据：

```java
Cookie cookie = new Cookie("key", "value");
```

2、发送Cookie到客户端：使用response对象

```java
response.addCookie(cookie);
```

获取Cookie：

1、获取客户端携带的所有Cookie（使用request对象）

```java
Cookie[] cookies = request.getCookies();
```

2、遍历数组，获取每一个Cookie对象

```
for(Cookie cookie : cookies){
	//获取cookie的key
	cookie.getName();
	//获取cookie的value
	cookie.getValue();
}
```

### 原理

Cookie的实现是基于HTTP协议的，Tomcat执行请求方法时，根据响应结果内容在响应头中添加：set-cookie:username=zs。浏览器收到响应头解析并执行，将cookie根据服务端网址存储在内存中。

浏览器需要向服务器发送请求时，先找到来自该服务器的所有存储在浏览器中的Cookie，并放在请求头中发送给服务端。

### 细节

#### Cookie的存活时间

1、默认情况下，Cookie存储在浏览器的内存中，当浏览器关闭时，内存就会释放，Cookie也会被释放。

2、Cookie可以设置存活时间，即通过cookie.setMaxAge(int seconds)可以设置Cookie的存活时间。

​	参数：

​		1、整数：将Cookie写入浏览器所在的电脑硬盘，进行持久化存储，到时间自动删除。

​		2、负数：默认值。Cookie存储在当前浏览器内存中，当浏览器关闭时，Cookie被销毁。

​		3、零：删除对应Cookie。

#### Cookie存储中文

Cookie默认不支持存储中文，如果需要存储中文，需要转码存储。如通过URL编码进行转码：

```java
URLEncoder.encode("张三", "UTF-8");
```

获取时再通过解码即可：

```java
URLEncoder.decode(value, "UTF-8");
```

## Session

> 服务端会话跟踪技术：将数据保存到服务端。JavaEE提供了HttpSession接口，来实现一次会话的多次请求间数据共享功能。

Session使用：

1、获取Session对象

```java
HttpSession session = request.getSession();
```

2、Session对象功能

```java
void setAttribute(String name, Object o): 存储数据到session域中
Object getAttribute(String name): 根据key, 获取值
void removeAttribute(String name): 根据key, 删除该键值对
```



### 原理

Session是基于Cookie实现的，在一次会话期间服务端使用的Session都是同一个。

首先浏览器向Tomcat服务器发送请求时，服务器会检测请求头中是否存在名为：“JSESSIONID” 的cookie，如果存在，则可以直接获取JSESSIONID中的信息进行操作，若不存在，则会自动为该请求创建出一个JSESSIONID的cookie并存储在浏览器。

### 细节

#### Session钝化、活化

使用Session可能存在一个问题，Session存储的对象数据是在服务端，如果服务端被关闭，那么Session域的数据是否会全部丢失？这个问题可以被Session的钝化、活化机制解决。

**钝化**：在服务器**正常关闭**的情况下，Tomcat会自动地将Session中的数据写入硬盘中的文件，即Tomcat下的work目录。

**活化**：再次启动服务器时，Tomcat会自动从work目录下加载数据回Session中保证数据不会丢失。



#### Session销毁

1、默认情况下，在无操作情况下，Session的自动销毁时间为30min。或者通过在xml文件中设置：

```xml
<session-config>
	<session-timeout>30</session-timeout>
</session-config>
```

2、手动调用Session对象的invalidate()方法进行销毁。多用于退出登录操作。



# Filter

> 过滤器。可以把对资源的请求拦截下来，从而实现一些特殊的功能。作为Web三大组件之一。

过滤器一般完成一些通用的操作，比如权限控制、统一编码处理、敏感字符处理等。

## Filter快速入门

> Filter类需要实现Filter接口，并实现其中的init()、doFilter()、destroy()方法。其中，JDK8中无需再手动实现init和destroy方法，这两个方法成为了接口默认方法。

```java
//表示要拦截的路径，可以为一个资源或者一个Servlet请求
@WebFilter("/*")
public class WebFilterDemo implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
		//放行前逻辑
        
        //请求放行
        filterChain.doFilter(servletRequest, servletResponse);
        
        //放行后逻辑
    }
}
```

## Filter执行流程

1、当Tomcat启动时，会先创建Filter对象，然后调用init方法，该方法只执行一次，用于加载资源。

2、请求匹配上Filter的过滤时，拦截请求，执行doFilter方法。doFilter方法可以分为三部分：第一部分为放行前处理，第二部分为放行，第三部分为放行结束处理。第三部分会在请求放行结束后返回执行。

3、如果存在多个Filter对象，那么Filter的执行顺序将根据Filter类的名字字母排序执行。



# Listener

> 监听器。Web的三大组件之一。可以监听application、session，request三个对象创建、销毁或者往其中添加、修改，删除属性时自动执行代码的功能组件。

## Listener分类

JavaWeb共提供了八个监听器：

|     监听器分类     | 监听器名称                      | 作用                                           |
| :----------------: | ------------------------------- | ---------------------------------------------- |
| ServletContext监听 | ServletContextListener          | 用于对ServletContext对象进行监听（创建、销毁） |
|                    | ServletContextAttributeListener | 对ServletContext对象中属性的监听（增删改属性） |
|    Session监听     | HttpSessionListener             | 对Session对象的整体状态的监听（创建、销毁）    |
|                    | HttpSessionAttributeListener    | 对Session对象中的属性监听（增删改属性）        |
|                    | HttpSessionBindingListener      | 监听对象于Session的绑定和解除                  |
|                    | HttpSessionActivationListener   | 对Request对象进行监听（创建、销毁）            |
|    Request监听     | ServletRequestListener          | 对Request对象进行监听（创建、销毁）            |
|                    | ServletRequestAttributeListener | 对Request对象中属性的监听（增删改属性）        |

## Listener快速入门

1、定义一个类实现以上八个监听器之一，并覆写其中的方法，如：

```java
@WebListener
public class WebListenerDemo1 implements ServletRequestListener {
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {2\
        System.out.println("监听对象销毁");
    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("监听对象创建");
    }
}
```

2、在类上添加@WebListener注解，因为对指定对象监听，所以不需要参数。



