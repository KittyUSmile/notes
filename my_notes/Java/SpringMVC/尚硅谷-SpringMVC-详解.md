# 一、SpringMVC入门

> SpringMVC其实就是Spring框架的后续产品，为开发模型中的表述层提供服务的，表述层即前台页面层 + 后台Servlet。

## 1、SpringMVC的特点

- Spring家族原生产品，与IOC容器等基础设施**无缝对接**
- 基于**原生的Servlet**，通过功能强大的DispatcherServlet，对请求和响应进行统一处理
- 表述层各细分领域需要解决的问题**全方位覆盖**，提供全面解决方案
- 代码清新简洁，大幅度**提升开发效率**
- 内部组件化程度高，可拔插式组件**即插即用**，想要什么功能配置相应组件即可
- **性能卓越**，尤其适合现代大型、超大型互联网项目要求

## 2、SpringMVC启动配置

1、首先创建一个maven工程（可以使用quickstart骨架创建）。

2、在pom.xml中导入相关依赖同时设置打包方式为war包（最新的Tomcat默认导入了servlet和jsp依赖）：

```xml
<!--打包方式为war包-->
<package>war</package>

<!--SpringMVC依赖-->
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.1</version>	
</dependency>
<!--日志依赖-->
<dependency>
	  <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
</dependency>
<!--servletAPI依赖-->
<dependency>
	  <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    <!--scope标签的provided意思是在打包时服务器会提供，不需要打包进war包-->
      <scope>provided</scope>
</dependency>
```

3、配置SpringMVC的前端控制器，对浏览器发送的请求进行统一处理。以下为xml方式：

当使用以下方式进行配置时，SpringMVC的配置文件必须位于WEB-INF下，名称必须为<servlet-name>-servlet.xml。如：按以下配置SpringMVC配置文件的名字必须是springmvc-servlet.xml。

```xml
<servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>/</url-pattern>
</servlet-mapping>
```

或者通过扩展配置方式进行配置（该配置方式与普通配置方式区别为这种配置方式可以指定SpringMVC配置文件的位置，一般使用这种配置方式）：

```xml
<servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <!--固定值-->
    	<param-name>contextConfigLocation</param-name>
        <!--使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources-->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!--让前端控制器初始化时间提前到服务器启动时，提前进行初始化操作，而不是等到接收到Servlet请求时-->
    <load-on-startup>1</load-on-startup>
</servlet>
  
<servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>/</url-pattern>
</servlet-mapping>
```

4、在springMVC.xml中配置扫描组件（扫描固定包下的Component）、配置视图解析器。

```xml
<!--扫描包组件-->
<context:component-scan base-package="com.xxx.controller"></context:component-scan>

<!--视图解析器-->

```

Spring项目通过Tomcat启动时，若Deployment中 + 号中没有Artifact选项，则到pom.xml中查看是否配置了打包方式为war：<package>war</package>

  

# 二、@RequestMapping

知识点：

1、@RequestMapping注解参数中除了value，其它属性都是数组类型。

2、当不指定method时，表示任何类型的请求都能接收。当method指定多个时，表示满足这些的请求中的一个都能接收。

```java
@RequestMapping(value = "/mvc", method = {RequestMethod.POST, RequestMethod.GET})
```

3、params参数表示请求参数进行条件传入，如同时满足需要该参数、不需要参数或者该参数的值必须为xx。

```java
@RequestMapping(value = "/mvc", method = {RequestMethod.POST, RequestMethod.GET},
            params = {"username", "!age", "num=1000", "num!=800"})
```

4、headers参数用法与params类似，区别为其请求参数为请求头参数。当请求满足@RequestMapping注解中的value和method属性，但不满足headers属性，页面会显示404错误。

## 1、SpringMVC支持ant风格的路径

即类似模糊匹配的路径匹配规则。

```
？：表示任意的单个字符（请求路径中除/、?、.三个字符）
*：表示任意的0个或多个字符
**：表示任意的一层或多层目录（在使用**时）,只能使用/**/xxx的方式
```

使用如：

```java
@GetMapping("/testAnt/?a")
@GetMapping("/testAnt/*b")
@GetMapping("/testAnt/**")
```

## 2、SpringMVC支持路径中的占位符

这种传参方式常用于restful风格中。即将请求参数以请求路径的方式传入。

使用如：

```java
@ApiOperation(value = "测试Rest风格路径")
@GetMapping("/testRest/{id}/{name}")
@ResponseBody
public R testRest(@PathVariable("id") Long id, @PathVariable("name") String name){
    return R.success().data(id).data(name);
}
```

# 三、SpringMVC获取请求参数

## 1、通过ServletAPI获取

通过原生Servelt对象手动获取请求参数的方式。

```java
@GetMapping("/testServletAPI")
@ApiOperation(value = "测试通过ServletAPI获取请求参数")
//SpringMVC的核心组件DispatcherServlet会在间接调用请求对应映射处理方法前对方法参数进行注入，因此该HttpServletRequest参数也有值，即代理对象。
public R testServletAPI(HttpServletRequest request){
    String name = request.getParameter("name");
    String password = request.getParameter("password");
    return R.success().data("name", name).data("password", password);
}
```

这种通过原生Servlet获取请求参数的方式已不推荐。SpringMVC为我们提供了更方便的获取请求参数的方式。

## 2、通过控制器方法形参获取请求参数

SpringMVC提供的前端控制器DispatcherServlet会识别请求对应的处理方法中的参数，并将请求参数赋值给方法中名字相同的参数。

```java
@GetMapping("/testMVCParam")
@ApiOperation(value = "测试通过控制器方法形参方式获取请求参数")
public R testMVCParam(String name, String password, String[] hobby){
    return R.success().data("name", name).
            data("password", password).
            data("hobby", Arrays.toString(hobby));
}
```

这种方式较为常用。

## 3、@RequestParam

该参数的出现是为了解决通过控制器方法形参获取请求参数的弊端，即如果处理方法中的形参名字与请求参数中的名字不相同则无法匹配上的问题。

@RequestParam(“user_name”) String username 表示获取请求参数中名字为user_name的参数并赋值给处理方法中的参数username。

```java
@GetMapping("/testMVCParamRequest")
@ApiOperation(value = "测试通过控制器方法形参方式获取请求参数@RequestParam版")
public R testMVCParamRequest(@RequestParam("user_name") String name,
                             @RequestParam("word") String password,
                             @RequestParam("hobbies") String[] hobby){
    return R.success().data("name", name).
        data("password", password).
        data("hobby", Arrays.toString(hobby));
}
```

### @RequestParam的使用如下

1、@RequestParam为请求参数和控制器方法的形参创建映射关系

2、@RequestParam注解一共有三个属性：

​	1、value：指定为形参赋值的请求参数的名字

​	2、required：设置是否必须传输此请求参数，默认值为true。当设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置defaultValue属性，则页面报错400。若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为null。

​	3、defaultValue：不管required属性值为true或false，当value所指定的参数请求没有传输或传输的值为“”时，则使用默认值为形参赋值。



## 4、@RequestHeader

@RequestHeader为请求头信息和控制器方法的形参创建映射关系

@RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@RequestParam。

```java
@GetMapping("/testRequestHeader")
@ApiOperation(value = "通过@RequestHeader获取请求头参数")
public R testRequestHeader(@RequestHeader("Host") String host,
                           @RequestHeader("User-Agent") String userAgent){
    return R.success().data("host", host).
            data("User-Agent", userAgent);
}
```



## 5、@CookieValue

@CookieValue为cookie数据和控制器方法的形参创建映射关系

@CookieValue注解一共有三个属性：value、required、defaultValue，用法同@RequestParam。

```java
@GetMapping("/testCookieValue")
@ApiOperation(value = "通过@CookieValue获取cookie参数")
public R testCookieValue(@CookieValue("JSESSIONID") String cookie){
    return R.success().data("JSESSIONID", cookie);
}
```



## 6、通过pojo对象接收请求参数

即当请求参数过多或者具有某种特征，用这些参数整合成一个实体类专门用来接收参数。若浏览器传输的请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值。

```java
@GetMapping("/testPOJOParam")
@ApiOperation(value = "通过POJO形参对象接收请求参数")
public R testPOJOParam(UserInfo userInfo){
    return R.success().data("userInfo", userInfo);
}
```

## 7、请求参数乱码问题

一般GET请求不会出现乱码，如果GET出现乱码，则是Tomcat造成的，找到Tomcat的server.xml配置文件，找到：

```xml
<Connector port="8080" URIEncoding="UTF-8" protocol="HTTP/1.1" connectionTimeout="20000" .../>
```

添加上URIEncoding=“UTF-8”，GET请求就不会再出现乱码问题。

如果POST请求出现乱码，则需要在请求进来被DispatcherServlet管理，找到处理方法获取请求参数赋值给方法形参之前设置编码方式为UTF-8。

如果是在Servlet中，需要我们手写一个过滤器，过滤所有请求，将请求中的request编码方式设置为UTF-8再将请求放行。

但是在SpringMVC中，MVC为我们提供了一个编码过滤器组件，我们只需要将其加入容器即可。

通过xml方式配置：

```xml
<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFillter</filter-class>
    <!--设置请求编码方式-->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <!--设置响应编码方式-->
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
	<filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

# 四、域对象共享数据

即将数据共享到各种域之间，如请求转发时将数据放入request域中。以下所有代码仅供参考。

## 1、使用servletAPI向request域对象共享数据

使用原生的Servlet向request域中存放共享数据。推荐尽量不要使用原生Servlet。

```java
@GetMapping("/testServletAPIShareData")
@ApiOperation(value = "通过ServletAPI向request域对象中共享数据")
public String testServletAPIShareData(HttpServletRequest request){
    request.setAttribute("data","data");
    return "redirect:hello.html";
}
```

## 2、使用ModelAndView向request域对象共享数据

通过MVC提供的ModelAndView向request域中存放共享数据。该类是MVC中最为重要的类之一，所有向request域中共享数据的操作最后都是通过ModelAndView实现的。

```java
@GetMapping("/testModelAndViewShareData")
@ApiOperation(value = "通过ModelAndView向request域对象中共享数据")
public ModelAndView testModelAndViewShareData(){
    /**
         * ModelAndView有Model和View功能
         * Model主要用于向请求域共享数据
         * View主要用于设置视图，实现页面跳转
         */
    ModelAndView mav = new ModelAndView();
    //向请求域中共享数据
    mav.addObject("data","data");
    //设置视图，实现页面跳转
    mav.setViewName("hello");
    return mav;
}
```

## 3、使用Model向request域对象共享数据

通过MVC提供的Model向request域中存放共享数据。

```java
@GetMapping("/testModelShareData")
@ApiOperation(value = "通过Model向request域对象中共享数据")
public String testModelShareData(Model model){
    model.addAttribute("data", "data");
    return "redirect:hello.html";
}
```

## 4、使用map向request域对象共享数据

直接通过map向request域对象中共享数据。

```java
@GetMapping("/testMapShareData")
@ApiOperation(value = "通过Map向request域对象中共享数据")
public String testMapShareData(Map<String, Object> map){
    map.put("data", "data");
    return "redirect:hello.html";
}
```

## 5、使用ModelMap向request域对象共享数据

通过ModelMap向request域对象共享数据

```java
@GetMapping("/testModelMapShareData")
@ApiOperation(value = "通过ModelMap向request域对象中共享数据")
public String testModelMapShareData(ModelMap modelMap){
    modelMap.put("data", "data");
    return "redirect:hello.html";
}
```

## 6、Model、ModelMap、Map之间的关系

在MVC的中这三种类型都可以向request域中存放数据。且Model、ModelMap、Map类型的参数其实本质上都是BindingAwareModelMap类型。

## 7、使用servletAPI向session域共享数据

通过原生ServletAPI向session域中共享数据。

```java
@GetMapping("/testServletAPIShareSession")
@ApiOperation(value = "通过Servlet原生API向session域对象中共享数据")
public String testServletAPIShareSession(HttpSession session){
    session.setAttribute("data", "data");
    return "redirect:hello.html";
}
```

## 8、向application域共享数据

```java
@GetMapping("/testShareApplication")
@ApiOperation(value = "向application域对象中共享数据")
public String testShareApplication(HttpSession session){
    ServletContext application = session.getServletContext();
    application.setAttribute("data", "data");
    return "redirect:hello.html";
}
```



# 五、SpringMVC视图



# 六、RESTFul



# 七、HttpMessageConverter

> 报文信息转换器。将接收的请求报文转换为Java对象，或Java对象转换为响应报文。
>

HttpMessageConverter为此提供了两个注解和两个类型：@RequestBody、@ResponseBody、RequestEntity、ResponseEntity。

## 1、@RequestBody

@RequestBody用于获取请求报文中的**请求体数据**（因此只适用于POST请求）。使用时需要在控制器方法参数处设置一个形参，并使用@RequestBody进行标识，报文信息转换器就会通过请求中的请求体数据名字为形参对象中对应名字的属性赋请求体数据值。

```java
@PostMapping("/testRequestBody")
@ApiOperation(value = "测试@RequestBody请求参数为对象赋值")
public R testRequestBody(@RequestBody UserInfo userInfo){
    return R.success().data(userInfo);
}
```

## 2、RequestEntity

RequestEntity被设计用来接收整个请求报文数据。使用时需要在控制器方法的形参中设置该类型作为形参，报文信息转换器会将报文信息转换为RequestEntity中的属性。可以通过该类对象的**getHeaders()**获取请求头信息，通过**getBody()**获取请求体信息。

```java
@GetMapping("/testRequestEntity")
@ApiOperation(value = "测试testRequestEntity接收请求报文数据")
public R testRequestEntity(RequestEntity entity){
    return R.success().data(entity);
}
```

## 3、@ResponseBody

@RequestBody用于标识一个控制器方法，允许该方法的返回值直接作为响应报文的响应体响应到浏览器。

```java
@GetMapping("/testResponseBody")
@ApiOperation(value = "测试@ResponseBody返回JSON格式数据")
@ResponseBody
public R testResponseBody(){
    return R.success();
}
```

### @ResponseBody处理json

@ResponseBody虽然能将返回值直接作为响应体的响应报文返回，但是存在一个问题，HTTP中的相应报文只能是字符串类型，当返回值是一个对象时，就需要将对象转换为字符串。而 JSON 就能完美解决转换和格式不丢失的问题。

DispatcherServlet处理返回值时需要通过消息转换器进行转换。因此使用 json 处理返回值的步骤为：

1、导入jackson依赖（现在一般是fastjson依赖）

2、在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdaptor中会自动装配一个消息消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的Java对象转换为JSON格式的字符转。

```xml
<mvc:annotation-driven/>
```

3、在处理器方法上标注@ResponseBoey注解。

4、将Java对象直接作为控制器方法返回值返回，就会自动转换为Json格式的字符串。

## 4、@RestController

@RestController注解是SpringMVC提供的一个复合注解，标注在控制器的类上，相当于为类添加上@Controller注解和为类中每一个方法添加上@ResponseBody注解。

## 5、ResponseEntity

ResponseEntity用于控制器方法的返回值类型，表示该控制器方法的返回值就是响应到浏览器的响应报文。也可看作该返回值为自定义的响应报文。常用于文件下载功能。



# 八、文件上传和下载

## 文件下载

```java
@GetMapping("/downPictureByResponseEntity")
@ApiOperation("通过ResponseEntity下载图片")
public ResponseEntity<byte[]> downPictureByResponseEntity() throws Exception {
    String filename = "C:\\Users\\Lenovo\\Pictures\\壁纸\\居家 小清新 可爱清纯闭眼美女4k壁纸3840x2160_彼岸图网.jpg";
    FileInputStream inputStream = new FileInputStream(filename);
    byte[] bytes = new byte[inputStream.available()];
    inputStream.read(bytes);
    //创建HttpHeaders对象设置响应头消息
    MultiValueMap<String, String> headers = new HttpHeaders();
    headers.add("Content-Disposition", "attachment;filename=1.jpg");
    //设置响应状态码
    ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, HttpStatus.OK);
    inputStream.close();
    return responseEntity;
}
```

## 文件上传

文件上传前端需要确保上传方式为POST，且enctype=“multipart/form-data”。

后端要确保存在依赖：

```xml
<dependency>
	<groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>xxxxx</version>
</dependency>
```

且保证容器中存在名字为multipartResolver的文件上传解析器：

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```

控制器接收上传文件代码：

```java
@PostMapping("/pictureUpload")
@ApiOperation(value = "图片上传接口")
public R pictureUpload(@ApiParam("图片文件") @RequestParam("file") MultipartFile multipartFile) throws IOException {
    //获取文件名字
    String filename = multipartFile.getOriginalFilename();
    //获取文件大小
    long size = multipartFile.getSize();
    //将文件存储到计算机另外位置
    File file = new File("D:\\" + filename);
    if (!file.exists())file.createNewFile();
    multipartFile.transferTo(file);
    return R.success().data(filename);
}
```

## 解决文件重名问题

文件上传时若出现重名会直接进行覆盖操作。为防止覆盖，可以使用UUID作为名字或加入时间戳。



# 九、拦截器

SpringMVC中的拦截器作用上与Servlet中的过滤器一致。只不过过滤器作用在浏览器发送请求到达DispatcherServlet之间这段过程，用于对请求进行过滤，由Tomcat服务器调用。SpringMVC的拦截器作用在DispatcherServlet找到处理控制器方法之前，由MVC调用。

## 使用步骤

1、SpringMVC中定义自己的拦截器需要实现HandlerInterceptor或者继承HandlerInterceptorAdapter类（已过时），然后将其加入容器中：

```xml
<bean class="com.run.interceptor.MyIntercceptor"></bean>
```

拦截器接口HandlerInterceptor中共有三个抽象方法：

​	1、preHandler()：目标方法前执行。返回true表示放行，返回false表示拦截。

​	2、postHandler()：目标方法后执行。

​	3、afterCompletion()：视图渲染完毕执行。

DispatcherServlet分别在执行目标控制器方法前执行所有拦截器的preHandler()方法，在执行完目标方法后再执行所有拦截器的postHandler()方法，最后在渲染完视图后执行所有拦截器的afterCompletion()方法。

2、加入容器后，还需要配置进拦截器配置，让Spring知道这是一个拦截器：

```xml
<mvc:interceptors>
    <!--以下两种方式配置的拦截器作用范围都是/**-->
    <!--<ref bean="com.run.interceptor.MyIntercceptor"/>>-->
    <!--<bean class="com.run.interceptor.MyIntercceptor"/>>-->
    
    <!--配置一个具有过滤路径和排除路径的拦截器-->
    <mvc:interceptors>
        <!--拦截路径，/**仍是ant风格，表示拦截所有目录下所有资源-->
    	<mvc:mapping path="/**"/>
        <!--排除路径，/表示没有排除路径-->
        <mvc:exclude-mapping path="/"/>
        <!--拦截器-->
		<bean class="com.run.interceptor.MyIntercceptor"></bean>
	</mvc:interceptors>
</mvc:interceptors>
```

SpringBoot则通过加入WebMvcConfigurer告诉Spring这是一个拦截器：

```java
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
	
    //自己定义的拦截器
    @Autowired
    HandlerInterceptor handlerInterceptor;

    /**
     * 向WebMvcConfigurer中添加拦截器
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //向拦截器配置中添加该拦截器并设置拦截路径为所有文件
        registry.addInterceptor(handlerInterceptor).addPathPatterns("/**");
    }
}
```

## 执行循序

当拦截器配置中存在多个拦截器时，首先拦截器的preHandler()方法会按照加入的顺序进行执行（xml配置按从上到下，bean配置按照addInterceptor的顺序），拦截器的postHandler()方法会按照配置顺序的反序执行，拦截器的afterCompletion()也是按照配置顺序的反序执行。

以上情况是所有拦截器都正常执行的结果，若出现多个拦截器中的某个拦截器preHandler()方法返回false，则请求只会执行到拒绝拦截器，且所有拦截器的postHandler()方法都不会执行，而afterCompletion()能够执行到拒绝拦截器的前一个拦截器的afterCompletion()方法。



# 十、异常处理器

SpringMVC提供的处理控制器方法执行过程中出现异常的接口：HandlerExceptionResolver。简单说就是当控制器方法出现异常时，SpringMVC可以为我们获取到异常并允许我们对其处理，且仍对请求做出正常响应（如返回某个页面）而不是直接让页面展示500错误。

该接口常用的实现类有：**DefaultHandlerExceptionResolver**和**SimpleMappingExceptionResolver**。

DefaultHandlerExceptionResolver实现类是SpringMVC自己用的异常处理器，用来处理一些请求中的请求问题，如405等。

SimpleMappingExceptionResolver实现类允许我们用来定义自定义异常处理器，使用步骤为：

1、首先向容器中加入该异常处理类（xml方式）：

```xml
<bean class="org.framework.web.servlet.handler.SimpleMappingExceptionResolver">
	<property name="exceptionMappings">
        <!--键表示处理方法执行过程中出现的异常，值表示出现指定异常时设置一个新的视图名称，并跳转到指定页面-->
    	<props>
        	<prop key="java.lang.NullPointException">error</prop>
        </props>
    </property>
    <!--向请求域中放入错误信息-->
    <property name="exceptionAttribute" value="ex"></property>
</bean>
```

注解方式：

```java
@RestControllerAdvice
public class ExceptionHandlerConfig {

    @ExceptionHandler(NullPointerException.class)
    public R nullPointExceptionHandler(NullPointerException e){
        System.out.println("异常信息："+e.getCause());
        return R.fail(500, "空指针异常");
    }

    @ExceptionHandler(Exception.class)
    public R allExceptionHandler(Exception e){
        System.out.println("出错:" + e.getCause().getMessage());
        return R.fail(500, "出现全局异常");
    }

}
```



# 十一、注解配置SpringMVC

即通过配置类和注解替代繁琐的web.xml和SpringMVC配置文件的功能。

使用步骤如：

## 1、创建初始化类、代替web.xml文件

在Servlet3.0以后，容器提供了一个javax.servlet.ServletContainerInitializer接口，容器在启动时会自动寻找该类的实现类（同时也会加载web.xml文件），若找到，就是用其配置Tomcat的Servlet容器。

Spring为我们提供了一个该类的实现类SpringServletContainerInitializer，该类会寻找实现了WebApplicationInitializer的类然后将配置任务交给它们实现。在Spring3.2引入了一个较为方便的WebApplicationInitializer实现类：AbstractAnnotationConfigDispatcherServletInitializer，当我们的类继承了该类并将其部署到容器中时，容器会自动发现它，用其来配置Servlet的上下文。

```java
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * 指定Spring的配置类
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    /**
     * 指定SpringMVC的配置类
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebMvcConfig.class};
    }

    /**
     * 指定DispatcherServlet的映射规则，即url-pattern
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

## 2、配置SpringMVC配置类

使用了注解配置，那么就需要通过注解将需要的组件加入容器中。

有些功能并不能通过注解进行配置，因此SpringMVC提供了一个类WebMvcConfigurer。允许我们通过继承该类，实现其中的方法对某些功能进行操作（比如default-servlet-handler、拦截器、view-controller、异常处理）。

代替SpringMVC配置文件的组件有：

​	1、扫描组件（@ComponentScan（）替代）

```java
@ComponentScan("com.run.controller")
```

​	2、视图解析器（通过@Bean加入容器替代）

​	3、view-controller（通过继承WebMvcConfigurer实现方法替代）

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //即/hello对应的路径跳转的视图名为hello
        registry.addViewController("/hello").setViewName("hello");
    }
}
```

​	4、default-servlet-handler（通过继承WebMvcConfigurer实现方法替代）

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}
```

​	5、mvc注解驱动（在配置上标注@EnableWebMvc注解替代）

```java
@EnableWebMvc
```

​	6、文件上传解析器（通过@Bean加入容器替代）

```java
@Bean
public MultipartResolver multipartResolver(){
    return new CommonsMultipartResolver();
}
```

​	7、异常处理（通过继承WebMvcConfigurer实现方法替代，或者通过@Bean加入容器替代）

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        Properties properties = new Properties();
        properties.setProperty("NullPointException", "error");
        exceptionResolver.setExceptionMappings(properties);
        resolvers.add(exceptionResolver);
    }
}
```

​	8、拦截器（通过继承WebMvcConfigurer实现方法替代）

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private HandlerInterceptor handlerInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(handlerInterceptor).addPathPatterns("/**")
    }
}
```

# 十二、SpringMVC执行流程

## 1、SpringMVC常用组件

1. DispatcherServlet：**前端控制器**，不需要工程师开发，由框架提供。作用是统一处理请求和响应，是整个MVC请求流程的控制中心，由它来调度其它组件处理用户的请求。
2. HandlerMapping：**处理器映射器**，即控制器映射器，不需要工程师开发，由框架提供。作用是根据请求的url、method等信息查找对应的处理器方法（控制器方法）。
3. Handler：**处理器**，即控制器，需要工程师开发。作用是在对用户的请求进行处理。
4. HandlerAdapter：**处理器适配器**。不需要工程师开发，由框架提供。作用是控制器方法实际上由其调用执行。
5. ViewResolver：**视图解析器**。不需要工程师开发，由框架提供。作用是进行视图解析，得到相应的视图。
6. View：**视图**。即页面部分，需要工程师开发部分。作用是将模型数据通过页面展示给用户。

## 2、DispatcherServlet初始化过程

