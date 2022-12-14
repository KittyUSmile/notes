过程

> SpringBoot 是基于 Spring 的，所以引入 Spring 的相关依赖是必要的。

1. 创建启动注解 @ZhouyuSpringBootApplication ，给它设置好基本元注解，并设置上 @ComponentScan注解，表示扫描标注类包下所有类。
2. 创建ZhouyuSpringBootApplication启动工具类及run静态方法，静态方法中最少需要用户主启动类的Class类参数，而方法体要有：
   1. 创建出一个 web 容器。
   2. Tomcat 启动方法。传入容器作为参数，设置服务器的分发控制器时设置上SpringMVC提供的控制器（创建这个控制器需要容器作为参数）。
      1. 需要注意的是选择哪种服务器（Tomcat、Jetty）启动不通过手动指定，而是通过判断项目中加载了哪个服务器的核心类来判断。