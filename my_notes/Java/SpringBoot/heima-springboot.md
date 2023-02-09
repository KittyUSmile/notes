# 基础篇




# 实用篇

## 运维实用篇

### 工程打包与运行
> 如果项目想要被长久访问，需要把项目打成 war 或者 jar 包，部署到服务器上，才能被永久访问，这里简单展示如何进行项目打包和在 windows 本机上运行，和需要的条件。

首先，一个 springboot 项目如果要打包，需要在 pom.xml 中加入 maven 的打包插件，因为打包是交给 maven 去执行的，插件依赖为：
```xml
<build>  
    <plugins>  
        <plugin>  
            <groupId>org.springframework.boot</groupId>  
            <artifactId>spring-boot-maven-plugin</artifactId>  
        </plugin>   
    </plugins>  
</build>
```
但是需要注意，以上的依赖插件只是 maven 打包所需的最基础的插件，有时候可能还需要加上其它插件。

插件依赖加入后刷新 maven，在 IDEA 中找到 maven，找到我们的项目，点开 LifeCycle，这是 maven 给项目定义的生命周期，我们如果只想打包，可以直接双击 package 即可，但是，需要注意，maven 提供的 package 操作会将项目打包前的生命周期全部执行一遍，包括 test，这个部分会执行一遍我们在 test 模块中定义的测试方法，我们通常不需要，所以需要禁用该生命周期，操作如下：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/2/Snipaste_2023-01-02_21-30-22.png">

禁用之后，双击 package 打包即可，不需要执行 clean，因为 package 操作会执行一次 clean 操作，打包成功后，可以在 target 编译目录中看到成功打包的 jar 文件：
<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/2/Snipaste_2023-01-02_21-29-16.png">

这里可以选择直接右键运行，或者打开文件地址，通过 cmd 命令：java -jar xxx.jar 运行：
<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/2/Snipaste_2023-01-02_21-33-15.png">

### 临时属性
> 有时候已经打成 jar 包的文件要修改运行配置，就可以通过临时属性来修改。修改方式为在运行命令后加上 -- 全属性名=xx 即可。

应用举例：
```xml
java -jar springboot.jar --server.port=9090
```

如果有多个运行配置，继续在后面加 -- 全属性名=xx 即可，多个属性之间用空格分隔。

以上可能更多是运维会进行的操作，但是作为开发，这些配置首先需要在本地代码中跑一遍再交给运维去执行，所以可以在 IDEA 中先进行配置，操作如下：

点击项目的启动设置（Edit Configurations） -> Environment -> Program arguments -> 编辑运行配置；在此处编辑的运行配置可以相当于是运维在运行 jar 包时添加运行配置：
<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/2/Snipaste_2023-01-02_22-13-01.png">

这里可以引出一个主启动类的知识点，主启动类的 args 参数其实就会被用来接收这些配置，并传递给 springboot 去执行，如果想要禁用或者自定义这些配置，可以尝试不传入 args 参数或者修改 args 中的参数，args 中的参数都是 String 类型，格式都是 --全属性名=xx ，以下是修改案例：
```java
@SpringBootApplication
public class Test1Application {  
    public static void main(String[] args) {  
        System.out.println(Arrays.toString(args));  
        // 自定义 args  
//        String[] args1 = new String[1];  
//        args1[0] = "--server.port=6060";  
        SpringApplication.run(Test1Application.class, args);  
//        不使用 args 配置  
//        SpringApplication.run(Test1Application.class);  
    }  
}
```



## 开发实用篇



# 原理篇

