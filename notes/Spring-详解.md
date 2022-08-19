# 一、初入Spring

Spring是一个轻量级的JavaEE框架，核心是IOC（控制反转）和AOP（面向切面）。可以解决企业开发级别的难题。

## 1、Spring特点

1. 通过IOC实现解耦，让程序更加灵活。
2. 支持面向切面编程，即支持面向方法级别的拦截器。
3. 能够更加方便地进行代码测试。
4. 由于容器技术，可以很方便的与其它技术融合。
5. 支持更方便的事务操作。
6. 可以依靠Spring大大降低企业级开发难度。



## 2、Spring入门案例

Spring的入门使用较为简单，首先去该网址下载Spring官网声明为GA的稳定版（SNAPSHOT为快照版）：

https://repo.spring.io/ui/native/release/org/springframework/spring/

然后在解压文件中找到核心的四个包：core、beans、context、expression和自己找一个日志jar包commons-logging。

创建一个普通Java项目，并创建出一个lib包，将这五个jar包导入并add in library。

然后可以尝试使用Spring的IOC方式创建一个对象，首先在src目录下创建一个xml配置文件，这个文件是Spring通过配置文件管理bean的核心配置文件，然后在该xml文件中通过bean标签完成一个对象的创建：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="user" class="com.test1.pojo.UserInfo"></bean>
    
</beans>
```

然后创建一个测试类，使用Spring提供的加载类来加载这个xml文件，接着就能在容器中找到这个对象了：

```java
public class Test01 {

    @Test
    public void test1(){
        //加载配置文件，获取到容器对象
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        //从容器对象中获取通过IOC产生的对象
        UserInfo bean = applicationContext.getBean(UserInfo.class);
        System.out.println(bean);
    }

}
```

# 二、IOC

控制反转机制，灵活度极高，相当强大，Spring的核心之一。通俗来讲就是Spring剥夺你自行创建某些对象的能力，而让这些对象都由Spring来创建，当你需要使用时，通过DI（依赖注入）来获取对象。

每项技术的出现都是有原因的，IOC的出现就是为了解决三个企业级开发难以解决的问题：

​	1、对象使用范围局部单一。如果要在方法中使用其它类对象，传统解决就是在需要的时候手动 new 一个，如果要让类中所有方法使用其它类对象，传统解决方案就是在类中 new 一个普通 or 静态对象。那么如果许多类都要用到同一个对象，那么使用传统方案处处 new 就会显得非常占用空间资源且冗余，且这些对象只能由类或者方法单独管理，在使用上也没有统一的规律可言。

​	2、对象生命周期难以统一管理。对象创建前需要做什么工作？创建后需要做什么工作？初始化前需要做什么工作？后续有没有对对象的其它操作？这些工作如果由我们来做，那么工作量将极大，难度也高。因此Spring帮我们完成了这部分工作，我们只需要通过少量代码就能完成对Spring容器对象的生命周期的管理。

​	3、让对象的创建和业务代码解耦。业务代码与创建对象分离，降低了代码之间的耦合性。让业务代码和对象之间为组合关系，而不是聚合关系。

## 1、IOC实现

IOC底层实现通过xml解析、工厂模式、反射来创建对象的，具体说就是解析applicationContext.xml配置文件，然后通过工厂模式和反射来创建对象。

Spring提供IOC容器创建对象的两种方式（两个接口）：

1、**BeanFactory**：IOC容器的基本实现，是Spring内部的使用接口，不提供给开发人员使用。使用BeanFactory加载配置文件时不会创建对象，而是在获取对象时才进行创建。

2、**ApplicationContext**：BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员使用。加载配置文件时就会将配置文件中定义的对象进行创建。



## 2、Bean管理方式

Spring管理Bean共有两种操作方式：

### 	1、**xml配置方式**

（通过 new ClassPathXmlApplicationContext(“bean.xml”);  扫描配置文件）

#### 1、xml方式向容器添加对象

基于xml配置bean如：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo"></bean>
</beans>
```

其中，<bean>标签中的属性有多种，常见的如id，class等。

​	id属性：该bean的唯一标识，也是作为存储容器的bean的名字。

​	class属性：该bean对象的类的全路径。

Spring通过反射创建对象时默认是通过无参构造进行创建的，除非使用构造注入方式定义bean。

基于xml注入属性如：

#### 2、xml方式的Bean属性注入

##### 1、通过set属性注入

（必须存在对应的setter方法）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo">
        <property name="name" value="小明"></property>
        <property name="password" value="123"></property>
    </bean>
</beans>
```

##### 2、通过构造属性注入

（必须存在对应的构造方法）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo">
        <constructor-arg name="name" value="小明"/>
        <constructor-arg name="password" value="123"/>
    </bean>
</beans>
```

##### 3、p名称空间属性注入

（了解即可，简化setter注入）

先在<beans>属性中加入p名称空间的xmlns。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo" p:name="张三" p:password="123">
    </bean>
</beans>
```

##### 4、注入空值和特殊符号

通过xml配置方式为属性赋空值：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo">
        <property name="name">
            <null/>
        </property>
    </bean>
</beans>
```

属性值包括特殊符号（转义，或者将特殊符号写到CDATA中）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.test1.pojo.UserInfo">
        <property name="name">
            <value>1</value>
        </property>
        <property name="password">
            <value><![CDATA[<红楼梦>]]></value>
        </property>
    </bean>
</beans>
```

##### 5、注入对象

通过 ref 外部注入其它对象：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="uploadController" class="com.test1.controller.UploadController">
        <property name="uploadService" ref="uploadServiceImpl"/>
    </bean>
    <bean name="uploadServiceImpl" class="com.test1.service.UploadServiceImpl"></bean>
</beans>
```

通过内部注入其它对象：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="zs"/>
        <property name="password" value="123"/>
        <property name="orderInfo">
            <bean id="orderInfo" class="com.test1.pojo.OrderInfo">
                <property name="orderId" value="10001"/>
            </bean>
        </property>
    </bean>

</beans>
```

##### 6、注入集合属性

当集合泛型为其它对象时，一般会将其提取出来，用<ref>进行赋值。

###### 1、注入数组类型属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="zs"/>
        <property name="password" value="123"/>
        <property name="params">
            <array>
                <value>东</value>
                <value>南</value>
                <value>西</value>
                <value>北</value>
            </array>
        </property>
    </bean>

</beans>
```

###### 2、注入List类型属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="zs"/>
        <property name="password" value="123"/>
        <property name="params">
            <list>
                <value>东</value>
                <value>南</value>
                <value>西</value>
                <value>北</value>
            </list>
        </property>
    </bean>

</beans>
```

###### 3、注入Map类型属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="zs"/>
        <property name="password" value="123"/>
        <property name="params">
            <map>
                <entry key="东" value="East"></entry>
                <entry key="南" value="South"></entry>
                <entry key="西" value="West"></entry>
                <entry key="北" value="North"></entry>
            </map>
        </property>
    </bean>

</beans>
```

###### 4、注入Ser类型属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="zs"/>
        <property name="password" value="123"/>
        <property name="params">
            <set>
                <value>东</value>
                <value>南</value>
                <value>西</value>
                <value>北</value>
            </set>
        </property>
    </bean>

</beans>
```



##### 7、属性自动注入

在Spring中，提供了另一种对属性的自动注入方式，这种方式可以在面对引用类型属性（容器中存在的）时无需手动设置或创建，而是通过从容器中寻找并注入。属性自动注入常用的有两种：byType 和 byName。

```xml
<!--byName注入的前提是Bean的属性的名字与容器中对象名字相同-->
<bean id="test1" class="com.test1.pojo.Test1" autowire="byName"></bean>

<!--byType注入的前提是Bean的属性的类型的对象在容器中只有一份-->
<bean id="test1" class="com.test1.pojo.Test1" autowire="byType"></bean>
```

但是在实际情况中，为了方便直观，我们一般直接在属性中通过注解@Autowired进行属性注入，而不是使用相同效果的xml属性注入。



##### 8、通过外部属性文件为Bean属性赋值

一般如果要对Bean的属性赋更灵活的值时，可以采用外部属性文件的方式为属性赋值。更解耦。

使用方式为先在xml文件中引入context名称空间，再引入外部属性文件，最后可以通过${}引入外部属性文件中的键值属性。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--    配置后置处理器：会为所有bean添加上后置处理器功能-->
    <bean id="beanPost" class="com.test1.pojo.BeanPost"></bean>

<!--    引入外部属性文件-->
    <context:property-placeholder location="classpath*:application.properties"/>

    <bean id="userInfo" class="com.test1.pojo.UserInfo">
        <property name="name" value="${name}"/>
        <property name="password" value="${password}"/>
    </bean>

</beans>
```



### 	2、**注解方式**

注解即通过标记的方式实现对象加入容器，实现属性注入等功能。注解的好处是简单易懂，方便快捷。本质上使用注解配置与使用xml配置没有区别。需要理解的是注解功能被实现是通过反射，如果没有处理该标记的方法，那么注解就不会生效。

#### 1、注解方式向容器中添加对象

通过注解向容器中添加对象有四种方式：

@Component、@Controller、@Service、@Repository。

后三个注解本质上都是@Component注解，因此它们作用都一样，区别在于@Controller建议声明在控制层，@Service建议声明在业务层，@Repository建议声明在持久层。这么做好处是直观，容易分辨。

将这四个注解之一标注在类上，并确保类所在路径能被扫描到，那么类对应的对象就会被加入到容器中。

使用注解方式之前需要引入Spring的aop依赖，然后开启扫描功能（也要引入context名称空间）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--    如果需要扫描多个包，中间使用逗号隔开-->
    <context:component-scan base-package="com.test1.test, com.test1.pojo"/>

</beans>
```

在类上使用其中一个注解：

```java
@Component
//@Controller
//@Service
//@Repository
public class Test1 {
}
```

当不指定这四个注解的名称时，默认加入容器的对象名称是本类的名字的首字母小写。

#### 2、自定义扫描注解规则

当使用xml配置组件扫描路径时，允许通过配置排除某些注解标注的组件，或者只扫描某些注解标注的组件。但使用前要先关闭默认的排除规则（默认规则为不进行排除）。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--    排除扫描@Controller注解标注的类-->
    <context:component-scan base-package="com.test1.test, com.test1.pojo" use-default-filters="false">
        <context:exclude-filter type="annotation" 
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    
    <!--    只扫描@Controller注解标注的类-->
        <context:component-scan base-package="com.test1.test" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

</beans>
```

由于注解之间存在相互标注，如@Component注解是其它三个注解的底层注解，在排除注解时目标注解的底层注解也会一同进行排除。而只扫描则不同，除非目标注解是其它注解的底层注解，否则不会扫描。



#### 3、注解方式Bean属性注入

##### 1、@Autowired

默认根据类型进行属性注入，由Spring提供。当容器中存在多个同类型对象时，就需要与注解@Qualifier一起使用，一般@Qualifier注解会用在注入接口实现对象时，如果容器中存在多个实现类，则无法判断出哪个是我们需要的。因此需要使用名称指定。

```java
@Component
public class Test1 {

    @Qualifier("test2DaoImpl1")
    @Autowired
    public Test2Dao test2;

}
```



##### 3、@Resource

是Java提供的注解。



##### 4、@Value

对属性进行直接注入，不通过从容器中寻找的方式。

```java
@Component
public class Test1 {

    @Value("abc")
    public String str;

}
```



#### 4、通过注解完全替代xml配置

（通过 new AnnotationConfigApplicationContext(SpringConfig.class); 的方式）

推荐使用的方式。使用注解完全替代传统繁琐的xml配置。

```java
//表明这是一个配置类文件，底层为@Component注解
@Configuration
//声明要扫描的组件路径，等同于xml配置中的 <context:component-scan base-package="com.test1.test"/>
@ComponentScan(basePackages = "com.test1.test")
public class ConfigTest1 {

}
```





## 3、Bean分类

在Spring中，bean指的是容器管理的对象，而这些存入的对象，分为两类，一类是我们自己创建的bean，称为普通bean，另一种是实现了FactoryBean接口并重写了方法的bean，被称为工厂bean（FactoryBean）。

这二者的区别为，普通bean是怎么定义的，就会被怎么放入容器并被获取。而工厂bean则可能定义的和放入容器的有区别。

实现一个工厂bean：

```java
public class FactoryBeanTest1 implements FactoryBean<UserInfo> {

    @Override
    public UserInfo getObject() throws Exception {
        return new UserInfo();
    }

    @Override
    public Class<?> getObjectType() {
        return UserInfo.class;
    }
    
    @Override
    public boolean isSingleton() {
        return FactoryBean.super.isSingleton();
    }
}
```

将其加入容器中时看起来像是FactoryBeanTest1类的对象：

```xml
<bean id="bean1" class="com.test1.test.FactoryBeanTest1"></bean>
```

但实际上这是一个UserInfo对象。FactoryBean产生的对象由方法getObject（）决定，而不由类本身决定。









## 4、Bean对象作用域

即Bean对象作用域常用的可以分为单例（singleton）和多例（prototype）（Java共有四种，其它两种较少见）。单例是指在整个容器启动期间，该对象始终只有一个。多例则对象不归容器管理，想创建几个就创建几个。默认情况下bean是单实例的。

```java
UserInfo bean1 = applicationContext.getBean("bean1", UserInfo.class);
UserInfo bean2 = applicationContext.getBean("bean1", UserInfo.class);
System.out.println(bean1 == bean2);
```

如何设置一个多例bean呢：

```xml
<bean id="bean1" class="com.test1.test.FactoryBeanTest1" scope="prototype"></bean>
```

当一个bean的作用域为单例（singleton）时，它会随着容器的启动而加载创建。当一个bean为多例（prototype）时，只有当getBean时，该对象才会被创建。



## 5、Bean的生命周期

即bean从被创建到被销毁的完成过程。完成而言一共有七步。bean的生命周期可以理解为Spring创建这个bean时的操作顺序。

以单例bean为例：

1. 首先bean随着容器启动而加载创建对象，默认调用无参创建对象。
2. 按照设置为bean的属性赋值（set注入等）。
3. 把bean实例传递给bean后置处理器前置处理方法（如果配置了的话）
4. 调用bean的初始化方法（如果配置了的话）。
5. 把bean实例传递给bean后置处理器后置处理方法（如果配置了的话）
6. bean基本可以使用了（可以被获取到并使用）。
7. 当容器关闭时，会调用bean的销毁方法（如果配置了的话）。

看看代码：

```java
@Data
public class Order {

    private String id;

    public Order() {
        System.out.println("第一步、调用无参创建对象");
    }

    public void setId(String id) {
        System.out.println("第二步、为属性赋值");
        this.id = id;
    }

    public void initMethod(){
        System.out.println("第三步、调用初始化方法");
    }

    public void destroyMethod(){
        System.out.println("最后一步、调用销毁方法");
    }
}
```

xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="order" class="com.test1.pojo.Order" init-method="initMethod"
          destroy-method="destroyMethod">
        <property name="id" value="10001"/>
    </bean>

</beans>
```

测试代码：

```java
@Test
public void test1(){
    //加载配置文件，获取到容器对象
    ClassPathXmlApplicationContext context =
            new ClassPathXmlApplicationContext("spring.xml");
    //从容器对象中获取通过IOC产生的对象
    context.getBean(Order.class);
    context.close();
}
```

结果：

xxx（BeanPostProcessor无法执行）



有时候这七步很不好理解，但其实只要掌握了bean的根本流程，其它步骤都是在根本流程上的操作。bean的根本流程其实可以就分为四步：对象创建完成、创建完成后的初始化操作、对象能被使用、对象销毁。

其中，对象创建完成分为两步：通过无参或有参**创建出对象**，然后通过setter或其它方式为**属性赋值**。

创建对象完成后的初始化操作可以分为三步：**前初始化操作**，**初始化操作**、**后初始化操作**。这里的初始化操作可以理解为一个功能完整的普通对象被创建出来后要对它做些什么。这里的三步中初始化操作权限没有前初始化操作和后初始化操作权限大，这两个前后初始化操作可以对这个对象进行直接操作，甚至替换这个对象。而普通的初始化操作拿不到这个对象，只能做些别的操作。

然后就是对象**放入容器**被使用，容器关闭**对象被销毁**。



# 三、AOP

面向切面编程，Spring的核心功能之一。简要来说就是增强功能的同时不影响原有代码，让功能成为可拔插式，大大降低功能之间耦合。

## 1、AOP底层原理

AOP底层原理是通过动态代理实现的，具体实现即Spring对需要增强的方法的对象创建代理对象，然后在代理兑现实际调用该方法前后进行功能增强。

手撸一个 JDK 动态代理：

```java
public class Test01 {

    @Test
    public void test1(){
        //传入被代理对象
        TestProxy proxy = new TestProxy(new TestDaoImpl());
        //获取代理接口
        TestDao testDao = (TestDao) Proxy.newProxyInstance(Test01.class.getClassLoader(),
                new Class[]{TestDao.class}, proxy);
        //执行login方法
        testDao.login();
    }

}

class TestProxy implements InvocationHandler{

    private TestDaoImpl testDaoImpl;

    public TestProxy(TestDaoImpl testDaoImpl) {
        this.testDaoImpl = testDaoImpl;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("检查浏览器...");
        System.out.println("检查安全状态...");
        //执行被代理对象的方法
        method.invoke(testDaoImpl, args);
        return null;
    }
}
```

## 2、AOP中的术语

学习AOP需要先学习其中的术语，各个术语都表示各自的含义，都有各自的作用。

### 1、连接点

可以被增强的方法都被称为连接点。



### 2、切入点

实际增强的方法就被称为切入点。



### 3、通知（增强）

所增强的功能就被称为通知（增强）。

通知可以根据增强位置分为：

1. 前置通知：在方法执行前增强。
2. 后置通知：在方法执行后增强。
3. 环绕通知：在方法执行前后增强。
4. 异常通知：在方法出现异常时增强。
5. 最终通知：等效于Java中try catch finally部分中的finally，为最终通知，一定会执行。



### 4、切面

名词做动词。即把通知实际应用到切点的行为叫做切面。



### 5、切入点表达式

使用AOP之前需要熟悉的一种表达式，作用是指定该通知的目标是哪些类中的哪些方法。

切入点表达式的格式比较固定：

```
execution([权限修饰符] [返回类型] [类全路径].[方法名称]([参数列表]))

如：
execution(public void com.run.dao.TestDao.login(String)): 指定到某具体目标方法
execution(* com.run.dao.TestDao.login(..)): 指定到该方法及其重载方法
execution(* com.run.dao.TestDao.*(..)): 指定该类下的所有方法
execution(*): 指定到所有方法
```





## 3、AOP使用

在Spring中，采用的是一种叫做AspectJ的框架进行AOP操作的，AspectJ并不是Spring的组成部分，而是一个独立的AOP框架，可以运用在不同框架或项目中。

使用AspjectJ框架需要先引入aop依赖（使用SpringBoot则只需要引入spring-boot-starter-aop一个依赖即可），和aspjects相关依赖（aspjects并不依赖于Spring，因此有自己独立依赖的jar，共有三个），然后通过注解（常用），或者xml方式进行AOP配置。

### 1、注解方式使用

#### 1、注解方式配置

通过注解方式配置AOP较为简单，首先可以创建出增强方法，和被增强方法，在这两个方法的类上加上@Component注解。然后通过注解扫描这两个类加入容器，然后在配置类上加上@EnableAspectJAutoProxy注解开启AOP自动代理功能，同时需要在增强方法的增强类上添加@Aspject依赖。

然后在增强方法上使用通知和切入点表达式进行指定方法和指定位置的增强。

#### 2、使用举例

增强类及增强方法：

```java
@Component
@Aspect
public class AppendTest {

    @Before(value = "execution(* com.test1.test.TargetTest.login())")
    public void append(){
        System.out.println("检查操作执行...");
    }

}
```

被增强类及增强方法：

```java
@Component
public class TargetTest {

    public void login(){
        System.out.println("登录方法...");
    }

}
```

配置类：

```java
@Configuration
@ComponentScan(basePackages = "com.test1.test")
@EnableAspectJAutoProxy
public class ConfigTest1 {

}
```

给目标方法的其它位置也加上通知：

```java
@Component
@Aspect
public class AppendTest {

    @Before(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void append(){
        System.out.println("检查操作执行...");
    }

    @After(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void appendAfter(){
        System.out.println("检查关闭操作...");
    }

    @AfterThrowing(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void appendAfterThrowing(){
        System.out.println("异常检查...");
    }

    @AfterReturning(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void appendAfterReturning(){
        System.out.println("最终检查连接资源释放...");
    }

    @Around(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void appendAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("执行前环绕操作...");
        //执行目标方法
        proceedingJoinPoint.proceed();
        System.out.println("执行后环绕操作...");
    }
}
```



#### 3、公共切入点抽取

即如果出现较多的切入点表达式都相同，就可以将这些切入点表达式抽取出来，进行公用。

使用如：

```java
@Component
@Aspect
public class AppendTest {
    
    @Pointcut(value = "execution(* com.test1.test.TargetTest.login(..))")
    public void publicPoint(){}

    @Before(value = "publicPoint()")
    public void append(){
        System.out.println("检查操作执行...");
    }
}
```

#### 4、设置增强方法优先级

当同时存在多个增强方法对同一个被增强方法进行增强时，可以设置增强方法所在类的优先级，让增强方法先后执行，其中，@Order注解中的值越小，越先执行。

使用如：

```java
@Component
@Aspect
@Order(2)
public class AppendTest {

}

@Component
@Aspect
@Order(10)
public class AppendTest2 {
}
```



### 2、xml方式使用置（了解即可）



# 四、JdbcTemplate

Spring提供的对数据库进行操作的模块，现在一般都由Mybatis取代了。JdbcTemplate其实就是Spring对JDBC的封装，允许我们使用JdbcTemplate对数据库进行方便的操作。

## 1、准备工作

使用JdbcTemplate除了需要引入Spring核心依赖、Spring提供的spring-jdbc依赖、spring-tx依赖（事务相关）、spring-orm依赖（用来整合其它框架的依赖），还需要额外引入两个依赖：druid连接池依赖、mysql-connector依赖。

然后在spring配置文件中配置数据源：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
        <property name="url" value="jdbc:mysql://localhost:3306/user?serverTimezone=UTC"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    </bean>

</beans>
```

然后再配置 JdbcTemplate对象，并将配置的数据源注入其中：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="jdbc:mysql://localhost:3306/user?serverTimezone=UTC"/>
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="username" value="root"/>
        <property name="password" value="2002"/>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```

然后就可以使用JdbcTemplate进行操作了，具体就不进行记录了。



# 五、事务

事务简单说就是数据库操作的基本单元。一组事务要么都成功，要么都失败。只要一组操作成为了事务操作，就不能存在部分成功，部分失败，只能同时成功，同时失败。

## 1、事务的四大特性

事务也有自己的特性，即事务表现出来的特征：

1. 原子性（Atomicity）：事务就是一组操作，一个操作的基本单元，要么都成功，要么都失败。
2. 一致性（Consistency）：事务开始操作到结束，数据库的完整性没有被破坏。
3. 隔离性（Isolation）：多个事务之间不会相互影响。
4. 持久性（Durability）：事务操作结束后，其对数据的操作结果是永久性的。



## 2、事务的流程

举个事务的例子，A向B转账100元，在后台操作上应该是A先减去100元，B再加上100元，中途没有出现异常的话，转账是能正常进行的，如果当A减去100元后，系统出现异常，那么B并没有加上100元，这个问题就很大。因此就需要使用事务包裹A操作和B操作了。

事务的使用流程为：

1. 开启事务
2. 进行业务操作
3. 提交事务 / 出现异常，回滚事务



## 3、事务使用介绍

一般Web项目都分为三层架构，事务一般是添加在Service层，添加在Dao层也可以，但是Service层可能调用多个Dao类进行操作，所以一般添加到Service层。

通过Spring进行事务操作共有两种方式：

1. 编程式事务管理：在代码中手动开启事务，并在 try 中编写业务代码和事务提交代码，并通过 catch 捕捉未知异常，然后在catch中进行手动回滚操作。（不推荐，很不方便，不易维护）
2. 声明式事务管理：有两种声明事务方式（xml 和 注解）
   1. 基于注解方式
   2. 基于xml方式

声明式事务管理的实现是通过AOP的方式实现的。

Spring通过一个名为PlatformTransactionManager的接口组件（事务管理器）来提供事务服务，这个组件对不同框架提供了不同的实现类，若使用Mybatis作持久层，则可以使用DataSourceTransactionManager作为事务管理器。



## 4、声明式事务使用步骤

首先先在容器中加入事务管理器对象：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="jdbc:mysql://localhost:3306/user?serverTimezone=UTC"/>
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="username" value="root"/>
        <property name="password" value="2002"/>
    </bean>

    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```

然后开启事务注解（xml需要先引入tx名称空间）：

注解方式即在配置类上标注@EnableTransactionManagement。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="jdbc:mysql://localhost:3306/user?serverTimezone=UTC"/>
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="username" value="root"/>
        <property name="password" value="2002"/>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    
    <tx:annotation-driven transaction-manager="transactionManager"/>
    

</beans>
```

然后就可以通过在需要开启事务的类或方法上标注@Transactional注解进行事务管理了。该注解标注在方法和类上有区别，标注在方法上时，仅对该方法开启事务，当标注在类上时，对类中所有方法开启事务。

```java
@Transactional
public interface UserService {

    void add(User user);

    void transferMoney();

}
```

## 5、声明式事务参数配置

即针对声明式事务的参数以及行为进行配置，在@Transactional（）注解的参数属性中配置。

常用的配置熟悉有三种：

1. propagation：事务的传播行为
2. isolation：事务的隔离级别
3. timeout：超时时间
4. rollbackFor：遇到哪些异常回滚



### 1、事务的传播行为（propagation）

Spring特有的规范。即一个事务方法被另一个事务方法调用时，这个事务方法该如何进行处理。

Spring定义了七种事务的传播行为，每种传播行为对应一种处理方式：

1. **REQUIRED**：即事务A中的方法1和事务B中的方法2，若方法1调用方法2，则方法2加入方法1所在事务。即当调用方存在传播行为为REQUIRED的事务时，二者共用一个事务。

2. **REQUIRED_NEW**：即无论被调用方法是否存在事务，总会新建一个新的事务，且新事务与调用方法的事务不属于同一事务。

3. **SUPPORTS**：如果有有事务在运行，当前方法就在这个事务中运行，否则它可以不运行在事务中。

   …

使用即：

```java
@Transactional(propagation = Propagation.REQUIRED)
public interface UserService {

    void add(User user);

    void transferMoney();

}
```



### 2、事务的隔离级别（ioslation）

ANSI SQL标准规范。事务隔离级别的出现，是为了解决并发事务可能会出现的问题。并发事务可能会出现的问题有读问题和写问题。

其中，读问题有脏读、不可重复读、幻读。

1. **脏读**：指读取到其它事务未提交的数据，未提交事务意味着能回滚导致读取异常。
2. **不可重复读**：学习这个概念前先介绍可重复读概念，即在一个事务中，最开始读到的数据和事务结束前的任意时刻读到的都是同一个数据，这就是可重复读。而不可重复值即值得就是在同一事务中，不同时刻读取到的同一个数据可能是不一样的，可能会受到其它事务的影响，如其它事务修改了这批数据并进行了提交。不可重复读主要是针对update操作（不可重复读较为常见，可以被看作一种现象而不是问题）
3. **幻读**：即可以理解为事务A对一批数据进行统计，然后事务B对这批数据中某些部分进行增或删，事务A再次读取时就会发现数据总量前后不一致。这就是幻读。

读问题都能通过设置事务的隔离级别来解决（MySQL的InnoDB同样实现了事务隔离机制）。共可以设置四种事务的隔离级别：

|                              | 脏读 | 不可重复读 | 幻读 |
| :--------------------------: | ---- | ---------- | ---- |
| READ UNCOMMITTED（读未提交） | 有   | 有         | 有   |
|  READ COMMITTED（读已提交）  | 无   | 有         | 有   |
| REPEATABLE READ（可重复读）  | 无   | 无         | 有   |
|    SERIALIZABLE（串行化）    | 无   | 无         | 无   |

使用即：

```java
@Transactional(propagation = Propagation.REQUIRED,
        isolation = Isolation.REPEATABLE_READ)
public interface UserService {

    void add(User user);

    void transferMoney();

}
```

### 3、其它参数

剩下可设置的声明式事务参数还有 

1. timeout（超时时间）：设置事务的提交时间，超过提交时间会回滚。Spring默认事务不超时（-1）。
2. readOnly（是否只读）：即设置为事务中的操作是否只能进行读操作，不能进行增删改操作。Spring默认为false。
3. rollbackFor（回滚）：设置事务操作中出现哪些异常进行回滚。
4. noRollbackFor（不会滚）：设置事务操作中出现哪些异常不进行回滚。



# 六、Spring5.0新功能

## 1、日志

Spring不再提供log4j的配置类，转而推荐使用log4j2作为日志使用。



## 2、@Nullable注解

Spring5.0提供的新注解，可以标注再方法、属性、参数上，表示允许它们的返回值为空。



## 3、函数式注册对象

Spring5.0提供的新功能，允许自定义对象然后注册进容器中。Spring为此提供了一个类GenericApplicationContext来进行注册操作。

```java
@Test
public void test1(){
    GenericApplicationContext context = new GenericApplicationContext();
    //清空该容器对象
    context.refresh();
    context.registerBean(UserInfo.class, () -> new UserInfo());
}
```



## 4、Junit单元测试框架

将Junit5新单元测试之前先将一下Junit4，如果要在Spring中使用测试功能，需要引入Spring提供的test的jar包，然后可以通过注解方式简化测试流程。





