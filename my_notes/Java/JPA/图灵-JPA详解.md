#  Spring Data

> 谈论Spring Data JPA前，先了解下Spring Data，Spring Data是什么？ 在传统印象中，如果要使用一门新技术，就要去学习它的理论和操作，比如学习MySQL，Redis，ElasticSearch，它们都有自己独特的执行命令，ES有自己的脚本，Redis有自己的操作命令，如果要在项目中运用这些技术，将其整合进Spring，就会产生各自的框架，又有各自的操作方式，又会增加学习成本，Spring Data就是为了解决这个问题而出现的，它对于不同的持久化存储技术，提供了各自通用的接口（CrudRepository、PagingAndSortingRepository）和模板（JDBCTemplate、RedisTemplate、RestTemplate、MongoTemplate…），我们可以只学习Spring Data对应模块，就能去运用对应技术，很大程度上减少了开发时的学习成本。

SpringData支持的持久层技术非常多，常见的有：

- Spring Data common - 用于支持每个Spring Data模块的**核心公共模块**。
- Spring Data JDBC - 对 JDBC 的Spring Data存储库支持。
- Spring Data JPA - 对 JPA 的Spring Data存储库支持。
- Spring Data MongoDB - 基于Spring的对象文档支持和 MongoDB 存储库。
- Spring Data Redis - 从 Spring 应用程序轻松配置和访问Redis。
- Spring Data REST - 将Spring Data存储库导出为超媒体驱动的RESTful资源。



# JPA

## JPA 简介

> JPA 可以看作是一种 ORM 框架层面的规范，由 SUN 公司提出，为我们提供了一些较为实用的操作，比如支持XML和注解两种方式对对象属性和表字段之间的映射、提供了CURD对象来操作数据库、提供了面向对象的查询语言（JPQL查询语言）来查询数据。

[^注]: 由SpringBoot提供的 JPA 依赖底层引入的具体实现 ORM 其实是 Hibernate，Hibernate实现了具体的 JPA 的规范。



### JPA 和 JDBC 异同

> JPA 和 JDBC 有一定关系，但关系不多。

相同处：

1. 都和数据库操作有关，JPA可以看作是 JDBC 的升级版，它的上层建筑。
2. JDBC 和 JPA 都是一组规范接口。
3. 都是由 SUN 公司推出的。

不同处：

1. JDBC 是由各个关系型数据库实现的，JPA 是由 **ORM 框架**来实现的（这里的ORM指的是全自动持久层框架）。
2. JDBC 使用 SQL 语句和数据库进行通信，JPA 则使用**对象方式**（比如CRUDDao.find()，用对象.方法()方式发出指令），让 ORM 框架来生成对应的 SQL 语句来进行操作数据库。
3. JPA 在 JDBC 之上，也可以说是依赖于 JDBC 进行数据库操作。

[^注]: 使用 JDBC 意味着直接采用SQL语句与数据库通信，这可能会带来一定问题，比如开发人员学习成本更高、更换数据库难（不同数据库SQL规范不同）、Java对象属性与数据库字段映射难等问题。JPA 则像是一层中间件，架设在业务代码与SQL语句之间，我们可以通过业务代码操作 JPA ，而 JPA 根据指令去生成对应的 SQL 语句，去与数据库通信，这种设计好处较多，减少了SQL的学习成本、不同数据库可以进行统一操作、切换数据库更加方便等。



###  Hibernate 和 Mybatis 异同

> Hibernate 和 Mybatis 区别还是比较大的，无论是作用还是实用场景。

Mybatis特点：

1. **小巧**，**方便**。Mybatis其实可以看作 JDBC 的封装框架，提供了一些映射、缓存等操作。
2. **高效**，**简单**，**直接**，**半自动**。Mybatis 比较小，业务比较简单，因此执行效率较高，并且学习成本较低，一般直接手写 SQL 进行操作数据库，其实称其为半自动其实也是不太准确的，且Mybatis 也没有实现 JPA 规范。
3. Mybatis 的应用场景一般是在**业务比较复杂**的地方，比如SQL 语句长、联表多的业务场景下，可以采用Mybatis进行开发。

Hibernate特点：

1. **强大**、**方便**、**高效、复杂、全自动**。Hibernate 所提供的数据库操作方法实用起来十分方便，可以根据不同 ORM 生成不同SQL 语句，功能强大，其执行效率也高，但是Hibernate针对复杂SQL场景是难以应对的。
2. Hibernate实现了 JPA 规范，一般可以应用在 SQL 业务相对简单的场景中。



## Hibernate 入门

> JPA 最常用的实现就是Hibernate，Spring Boot 集成 JPA 底层默认引入的就是 Hibernate，所以需要对 Hibernate 先做一定的了解。

### 代码入门

> 使用Hibernate前需要先引入依赖（这里采用普通的 Maven 项目对 Hibernate 进行入门），然后还需要Hibernate的配置文件、实体类（Hibernate一般不需要手动建表，可以通过配置实体类让Hibernate替我们创建好数据库表，但数据库需要我们自己创建，这种方式称为code first）

#### 相关依赖

```xml
<dependencies>
    <!--        junit4-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>
    <!--        Hibernate-->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.4.32.Final</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.18</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
</dependencies>
```

#### 实体类

> 在 JPA 入门和Spring Data JPA中使用的实体类。

```java
package com.run.entity;

import lombok.Data;

import javax.persistence.*;

/**
 * @Date: 2022/10/18/13:35
 * @Description:
 */
@Entity //声明为Hibernate的实体类
@Table(name = "tb_customer")    //所映射的表
@Data
public class Customer {

    /**
     * @Id: 声明为主键
     * @GeneratedValue: 主键的生成策略
     *   strategy = GenerationType.IDENTITY : 自增，mysql，前提是底层数据库支持自动增长
     *              GenerationType.SEQUENCE : 序列，oracle，前提是底层数据库必须支持序列
     *              GenerationType.TABLE : JPA 提供的一种机制，通过一张数据库表的形式帮助我们完成主键自增
     *              GenerationType.AUTO : 由程序自动的帮助我们选择主键生成策略
     * @Column: 属性与哪个字段的映射
     *   name: 数据库中对应的表字段
     */
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id; //客户主键

    @Column(name = "cus_name")
    private String cusName;

    @Column(name = "cus_address")
    private String cusAddress;

}

```

#### 配置文件

（最好叫做hibernate.cfg.xml，并放到resources目录下）：

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!--配置数据库连接信息-->
        <property name="connection.url">jdbc:mysql://localhost:3306/springdata_jpa?serverTimezone=UTC</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">2002</property>
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>

        <!--hibernate 方言: 选择好数据库类型-->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</property>

        <!--日志打印sql语句 默认是false-->
        <property name="hibernate.show_sql">true</property>
        <!--格式化sql 默认也是false-->
        <property name="hibernate.format_sql">true</property>
<!--        表生成策略
                默认是none 不自动生成
                update     如果没有表则自动创建，有则检查做更新
                create     不管三七二十一直接创建表
            -->
        <property name="hibernate.hbm2ddl.auto">update</property>

        <!-- 加载映射文件，指定哪些实体类需要进行 ORM 映射 ，还有一种通过xml的方式进行映射，
            则使用 <mapping resource=""/> 进行配置
        -->
        <mapping class="com.run.entity.Customer"></mapping>

    </session-factory>
</hibernate-configuration>
```

#### 测试

> 对Hibernate的基本功能进行测试

##### 基础代码

> 增删改通用的一些代码，主要做一些会话连接操作。

```java
package com.run;

import com.run.entity.Customer;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.boot.MetadataSources;
import org.hibernate.boot.registry.StandardServiceRegistry;
import org.hibernate.boot.registry.StandardServiceRegistryBuilder;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

/**
 * @Date: 2022/10/18/15:05
 * @Description:
 */
public class HibernateTest {

    // Session工厂，Session: 数据库会话，代码持久化操作数据库的一个桥梁
    private SessionFactory sessionFactory;

    @Before
    public void init(){
        StandardServiceRegistry registry =
                new StandardServiceRegistryBuilder().configure("/hibernate.cfg.xml").build();

        //根据服务注册类创建一个元数据资源集，同时构建元数据并生成应用唯一的session工厂

        sessionFactory = new MetadataSources(registry).buildMetadata().buildSessionFactory();
    }

}
```

##### 数据新增

```java
/**
     * 数据插入测试
     */
@Test
public void testInsert(){
    // 使用session进行持久化操作
    try(Session session = sessionFactory.openSession()){
        Transaction tx = session.beginTransaction();

        Customer customer = new Customer();
        customer.setCusName("小明");
        customer.setCusAddress("北京");

        session.save(customer);

        tx.commit();
    }
}
```

<img src="https://img1.imgtp.com/2022/10/18/CpiEZPv6.png" alt="1666078065329.png" title="1666078065329.png" />

##### 数据查询

```java
/**
     * 数据查询测试
     */
@Test
public void testSelect(){
    // 使用session进行持久化操作
    try(Session session = sessionFactory.openSession()){
        Transaction tx = session.beginTransaction();

        System.out.println(session.find(Customer.class, 1L));
        //懒加载查询（什么时候用什么时候查）
        //            Customer load = session.load(Customer.class, 1L);
        //            System.out.println("=====");
        //            System.out.println(load);

        tx.commit();
    }
}
```

<img src="https://img1.imgtp.com/2022/10/18/1fQPGazM.png" alt="1666078139036.png" title="1666078139036.png" />

##### 数据更新

```java
/**
 * 数据更新测试
 */
@Test
public void testUpdate(){
    // 使用session进行持久化操作
    try(Session session = sessionFactory.openSession()){
        Transaction tx = session.beginTransaction();

        Customer customer = new Customer();
        customer.setCusName("小张");
        customer.setCusAddress("深圳");
        customer.setId(2L);

        //相同效果: session.save()、session.update()
        session.saveOrUpdate(customer);

        tx.commit();
    }
}
```

<img src="https://img1.imgtp.com/2022/10/18/ifrSKD5w.png" alt="1666078363853.png" title="1666078363853.png" />

##### 数据删除

```java
/**
 * 数据删除测试
 */
@Test
public void testDelete(){
    // 使用session进行持久化操作
    try(Session session = sessionFactory.openSession()){
        Transaction tx = session.beginTransaction();

        Customer customer = new Customer();
        customer.setId(2L);

        session.delete(customer);

        tx.commit();
    }
}
```

<img src="https://img1.imgtp.com/2022/10/18/pdkTTJHY.png" alt="1666078544514.png" title="1666078544514.png" />



##### HQL查询数据

> HQL是Hibernate提供的一种非SQL的查询语句，其比 JPA 定义的 JPQL 更加强大，支持增删改查，而 JPQL 不支持插入。

```java
/**
     * HQL数据查询测试
     */
@Test
public void testHQLSelect(){
    // 使用session进行持久化操作
    try(Session session = sessionFactory.openSession()){
        Transaction tx = session.beginTransaction();
        //查询出Customer中所有数据，select可以省略
        String hql = "FROM Customer WHERE id=:id AND cusName=:cusName";

        List<Customer> list = session.createQuery(hql, Customer.class)
            .setParameter("id", 1L)
            .setParameter("cusName", "小明")
            .getResultList();
        System.out.println(list);

        tx.commit();
    }
}
```

![image-20221018154507932](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221018154507932.png)



## JPA 入门

> 在引入Hibernate依赖的时候就自动引入了 JPA 的相关依赖，要知道 JPA 只是一项规范，一组接口。JPA 不只Hibernate一个实现，还有如openjpa等。
>
> 如果要使用 JPA ，需要在 resources/META-INF 文件夹下创建相应的配置文件persistence.xml，进行相应配置。

### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">

<!--    一个持久化单元，如果想更换持久化单元，不使用hibernate，再定义一个新持久化单元即可-->
    <persistence-unit name="hibernateJPA" transaction-type="RESOURCE_LOCAL">
<!--        JPA 的实现类-->
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
<!--        需要进行 ORM 的实体类-->
        <class>com.run.entity.Customer</class>

        <properties>
            <!-- 数据库信息
                用户名，javax.persistence.jdbc.user
                密码，  javax.persistence.jdbc.password
                驱动，  javax.persistence.jdbc.driver
                数据库地址   javax.persistence.jdbc.url
            -->
            <property name="javax.persistence.jdbc.user" value="root"/>
            <property name="javax.persistence.jdbc.password" value="2002"/>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/springdata_jpa?serverTimezone=UTC"/>

            <!--配置jpa实现方(hibernate)的配置信息
                            显示sql           ：   false|true
                            自动创建数据库表    ：  hibernate.hbm2ddl.auto
                                    create      : 程序运行时创建数据库表（如果有表，先删除表再创建）
                                    update      ：程序运行时创建表（如果有表，不会创建表）
                                    none        ：不会创建表

                        -->
            <property name="hibernate.show_sql" value="true" />
            <property name="hibernate.hbm2ddl.auto" value="update" />
            <!--hibernate 方言: 选择好数据库类型-->
            <property name="hibernate.dialect" value="org.hibernate.dialect.MySQL8Dialect"/>
        </properties>
    </persistence-unit>
</persistence>
```

### 测试

#### 基础代码

```java
package com.run;

import com.run.entity.Customer;
import org.junit.Before;
import org.junit.Test;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

/**
 * @Date: 2022/10/18/16:07
 * @Description:
 */
public class JPATest {

    EntityManagerFactory factory;

    @Before
    public void before(){
        factory =
//              第一个参数指定所使用的持久化单元名字（persistence.xml中定义的），要换底层
//                    实现的话直接在这里更换持久化单元即可。
//              第二个参数Map类型可以添加persistence.xml中可以配置的参数信息，如数据库配置
                Persistence.createEntityManagerFactory("hibernateJPA");
    }
}
```

#### 数据新增

```java
/**
 * 测试新增
 */
@Test
public void testInsert(){
    //类似于session
    EntityManager entityManager = factory.createEntityManager();

    EntityTransaction tx = entityManager.getTransaction();
    tx.begin();

    Customer customer = new Customer();
    customer.setCusName("张三");
    customer.setCusAddress("漳州");

    entityManager.persist(customer);
    tx.commit();
}
```

<img src="https://img1.imgtp.com/2022/10/18/pK61AuOV.png" alt="1666080966355.png" title="1666080966355.png" />

#### 数据查询

```java
/**
     * 测试查询
     */
@Test
public void testSelect(){
    //类似于session
    EntityManager entityManager = factory.createEntityManager();

    EntityTransaction tx = entityManager.getTransaction();
    tx.begin();
    //立即查询
    Customer customer = entityManager.find(Customer.class, 1L);
    //延迟查询
    //        Customer customer = entityManager.getReference(Customer.class, 1L);
    System.out.println("=====");
    System.out.println(customer);
    tx.commit();
}
```

<img src="https://img1.imgtp.com/2022/10/18/1qXNt6uZ.png" alt="1666081664322.png" title="1666081664322.png" />

#### 数据更新

```java
/**
     * 测试更新
     */
@Test
public void testUpdate(){
    //类似于session
    EntityManager entityManager = factory.createEntityManager();

    EntityTransaction tx = entityManager.getTransaction();
    tx.begin();

    Customer customer = new Customer();
    customer.setCusAddress("株洲");
    customer.setCusName("小贝");
    customer.setId(4L);

    //merge执行可以看到先查询，再更新，如果存在则更新，不存在则插入，相同则无操作。同时merge更新也是完整
    //        更新，不是非空字段更新。同时JPA不存在纯更新操作，如果希望做纯更新操作，则需要自己写JPQL语句。
    entityManager.merge(customer);

    //使用jpql语句实现
    //        String jpql = "UPDATE Customer set cusName=:cusName, cusAddress=:cusAddress where id=:id";
    //        entityManager.createQuery(jpql)
    //                .setParameter("cusName",customer.getCusName())
    //                .setParameter("cusAddress", customer.getCusAddress())
    //                .setParameter("id", customer.getId())
    //                .executeUpdate();

    //当然在业务复杂的场景下，也可以通过SQL语句进行实现
    //        String sql = "update tb_customer set cus_address=:address, cus_name=:name where id=:id";
    //        entityManager.createNativeQuery(sql)
    //                .setParameter("address", "厦门")
    //                .setParameter("name", "小琴")
    //                .setParameter("id", 4)
    //                .executeUpdate();

    tx.commit();
}
```

<img src="https://img1.imgtp.com/2022/10/18/5cMsRQl9.png" alt="1666083077388.png" title="1666083077388.png" />

#### 数据删除

```java
/**
     * 测试删除
     */
@Test
public void testDelete(){
    //类似于session
    EntityManager entityManager = factory.createEntityManager();

    EntityTransaction tx = entityManager.getTransaction();
    tx.begin();

    //JPA 中对象存在四种状态，而游离状态的对象是无法进行删除的，会报异常。
    //游离状态是指并不是从数据库中查询出来的数据，而是自己定义的，如果使用以下对象进行删除就会报异常
    //通过先查询出来对象，再进行删除就不会报异常，如果不希望先查再删，可以使用jpql语句。
    //        Customer customer = new Customer();
    //        customer.setId(4L);

    Customer customer = entityManager.find(Customer.class, 4L);
    entityManager.remove(customer);

    tx.commit();
}
```

![image-20221018165814770](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221018165814770.png)



### JPA 对象的四种状态

> JPA 对象的状态是十分重要的，因为 JPA 中一切操作都是基于对象的，数据库表也是和对象相联系，因此产生了对象的不同状态，以更好地进行管理。四种状态分别是：**临时状态、持久状态、删除状态、游离状态**。这四种状态中持久状态会直接对应数据库，对处于持久状态的对象进行操作会直接影响数据库。

#### 临时状态（new）

> 对象刚被创建出来，还没有和entityManager发生任何联系，没有被持久化，还不是entityManager的对象。



#### 持久状态（managed）

> 和entityManager以及发生了联系，具体表现为对象是其查询出来的，或者是进行了插入操作，进行了持久化，但事务还未提交，这时对象的状态就变为持久状态，对持久状态对象的修改会直接影响到数据库。



#### 删除状态（removed）

> 执行了remove方法，对象对应的数据库内容被删除了，对象的删除状态介于事务还未提交但删除操作以及执行之间。



#### 游离状态（detached）

> 对象的操作已经提交到数据库后，即事务commit后对象所处的状态，这之后实体的属性怎么改变，都不会对数据库产生影响，因为其已经进入了游离状态。



### JPA 中各状态的操作表现

> 对象处于各种状态时，其对 JPA 提供的增删改方法的具体表现。

#### persist

> public void persist(Object entity)：JPA 中提供的插入操作，可以将对象转换为managed状态，在调用flush()方法或者提交事务之后，对象会被插入数据库。

对于不同状态下的对象，persist方法会产生**不同的操作**：

1. 如果对象处于 **new** 状态，它将会被转换为 managed 状态。
2. 如果对象处于 **managed** 状态，它的状态则不会改变，但系统仍会执行 insert 操作。
3. 如果对象处于 **remove** 状态，那么它将会转换为受控状态。
4. 如果对象处于 **detached** 状态，则 persist 方法会抛出 IllegalArgumentException 异常，具体异常依不同的 JPA 实现。



#### merge

> public void merge(Object entity)：JPA 中提供的更新操作，可以对处于 detached 状态的对象的修改进行归档，归档后将产生一个新的 managed 对象。

对于不同状态下的对象，merge方法会产生**不同的操作**：

1. 如果对象处于 **detached** 状态，该方法会将修改提交到数据库，并返回一个新的 managed 状态的对象。
2. 如果对象处于 **new** 状态，该方法会产生一个根据该对象产生的 managed 状态的新对象。
3. 如果对象处于 **managed** 状态，它的状态不会发生任何改变，但是系统仍会在数据库执行 update 操作。
4. 如果对象处于 **removed** 状态，该方法会抛出 IllegalArgumentException异常，



#### refresh

> public void refresh(Object entity)：refresh方法可以保证当前的实例与数据库中的实例内容一致。

对于不同状态下的对象，refresh会产生**不同的操作**：

1. 如果对象处于 **new** 状态，不会发生任何操作，也可能会抛出异常，具体情况依据不同的 JPA 实现。
2. 如果对象处于 **managed** 状态，它的属性会和数据库中的数据同步。
3. 如果对象处于 **removed** 状态，该方法将会抛出 Entity not managed 异常。
4. 如果对象处于 **detached** 状态，该方法将抛出异常。



#### remove

> public void remove(Object entity)：remove方法可以将对象状态转换为 removed，并在调用flush()方法或提交事务后删除数据库中的数据。

对于不同状态下的对象，remove会产生**不同的操作**：

1. 如果对象处于 **new** 状态，它的状态不会发生任何改变，但系统仍然会在数据库中执行 DELETE 语句。
2. 如果对象处于 **managed** 状态，它的状态就会转换为 removed。
3. 如果对象处于 **removed** 状态，不会发生任何操作。
4. 如果对象处于 **detached** 状态，该方法将将抛出异常。



[^注]: JPA 中也存在查询缓存，有一级和二级缓存，二级缓存使用较少，一级缓存的范围是同一个 EntityManager，在同一个EntityManager下的查询会被缓存起来，减少下次相同查询的时间。



# Spring Data JPA

> Spring Data JPA 是 Spring 提供的一套简化 JPA 开发的框架，其最大的特点是可以在只写 DAO 接口而不用写其实现的情况下，完成对数据库的基本CRUD操作，以及分页、排序、复杂查询等操作。这些功能是通过对 DAO 层的方法进行规范化的命名而实现的，JPA 通过解析方法的名字来生成对应的 SQL 语句。
>
> 在 IDEA 中可以下载一个 **JPA Buddy**插件，会有 JPA 的一些工具。

## Spring Data JPA 入门

> Spring Data JPA 在使用上与 JPA 差别不大，步骤都是引入依赖、配置、编码测试。但是在配置方面有所区别，下面测试使用注解方式来对Spring Data JPA 进行基本的使用。

### 相关依赖

> Spring Data JPA 提供了一个依赖可以对整个Spring Data模块的项目进行版本控制，把它放到父项目中，子项目直接引入依赖就可以。

父项目依赖：

```xml
<!--    这种依赖管理类似于子项目声明parent，但在父项目中声明以下依赖后，子类如果要使用只要引入即可
       区别是这种可以引入多个，而parent只能引入一个，bom 依赖作用是管理Spring Data下各依赖的版本，
       不至于Spring Data技术因为版本问题而产生一些依赖冲突。
-->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-bom</artifactId>
            <version>2021.1.9</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子项目依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-jpa</artifactId>
    </dependency>
    <!--        junit4-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>
    <!--        Hibernate，Spring并不会在JPA依赖中集成Hibernate，需要自己引入-->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.4.32.Final</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.18</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
    <!--        连接池-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.11</version>
    </dependency>
    <!--        测试模块依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.3.10</version>
    </dependency>
</dependencies>
```

### 实体类

> 这里仍采用Hibernate入门笔记中使用的实体类。



### 配置

```java
package com.run.config;

/**
 * @Date: 2022/10/19/13:44
 * @Description:
 */
@Configuration  //配置类
@EnableJpaRepositories(basePackages = "com.run.dao")  //启动JPA
@EnableTransactionManagement //开启事务
public class SpringJPAConfig {

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUsername("root");
        dataSource.setPassword("2002");
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/springdata_jpa?serverTimezone=UTC");
        return dataSource;
    }

    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory() {

        HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
        //设置是否自动创建表
        vendorAdapter.setGenerateDdl(true);
//        是否显示sql
        vendorAdapter.setShowSql(true);
        
        LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();
//        设置JPA底层实现
        factory.setJpaVendorAdapter(vendorAdapter);
//        设置扫描包
        factory.setPackagesToScan("com.run.entity");
        //设置数据源
        factory.setDataSource(dataSource());
        return factory;
    }

    @Bean
    public PlatformTransactionManager transactionManager(EntityManagerFactory entityManagerFactory) {

        JpaTransactionManager txManager = new JpaTransactionManager();
        txManager.setEntityManagerFactory(entityManagerFactory);
        return txManager;
    }
}

```

### DAO类

> 这里直接继承CrudRepository来进行操作，泛型两个参数一个是对象类，一个是主键类型。当然还可以继承PagingAndSortingRepository，功能比CrudRepository多了排序和分页两个功能。

```java
package com.run.dao;

import com.run.entity.Customer;
import org.springframework.data.repository.CrudRepository;

/**
 * @Date: 2022/10/19/13:46
 * @Description:
 */
public interface CustomerDao extends CrudRepository<Customer, Long> {

}
```

### 测试

> 进行增删改查测试

#### 通用代码

```java
@ContextConfiguration(classes = SpringJPAConfig.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class SpringDataJPATest {

    @Autowired
    CustomerDao customerDao;
    
}
```

#### 插入测试

```java
/**
     * 测试插入
     */
@Test
public void testInsert(){
    Customer customer = new Customer();
    customer.setCusName(UUID.randomUUID().toString().substring(0,5));
    customer.setCusAddress(UUID.randomUUID().toString().substring(0,5));
	//插入返回对象会带有主键id
    customerDao.save(customer);
}
```

<img src="https://img1.imgtp.com/2022/10/19/L1gqXNWs.png" alt="1666159603582.png" title="1666159603582.png" />

#### 查询测试

```java
/**
 * 测试查询
 */
@Test
public void testSelect(){
    Optional<Customer> byId = customerDao.findById(1L);
    System.out.println(byId.get());
}
```

<img src="https://img1.imgtp.com/2022/10/19/hgDPM6Op.png" alt="1666159654398.png" title="1666159654398.png" />

#### 修改测试

```java
/**
 * 测试修改
 */
@Test
public void testUpdate(){
    Customer customer = new Customer();
    customer.setId(8L);
    customer.setCusName(UUID.randomUUID().toString().substring(0,5));
    customer.setCusAddress(UUID.randomUUID().toString().substring(0,5));
    //save 方法，当有主键时是更新，没有主键时是新增
    customerDao.save(customer);
}
```

<img src="https://img1.imgtp.com/2022/10/19/ztEPdMk6.png" alt="1666159957641.png" title="1666159957641.png" />

#### 删除测试

```java
/**
 * 测试删除
 */
@Test
public void testDelete(){
    Customer customer = new Customer();
    customer.setId(6L);
    //Spring Data JPA解决了游离状态的删除问题，其底层通过JDK动态代理先进行了一次查询
    customerDao.delete(customer);
}
```

<img src="https://img1.imgtp.com/2022/10/19/uOisYR1S.png" alt="1666159709564.png" title="1666159709564.png" />



#### 分页测试

> 分页要确保dao类直接或间接继承了PagingAndSortingRepository类。

```java
/**
     * 测试分页
     */
@Test
public void testPage(){
    //查询第二页的五条数据，如果还需要排序，在第三个参数处传入Sort对象即可
    PageRequest page = PageRequest.of(2, 5);
    Page<Customer> all = customerDao.findAll(page);
    //拿到查询到的内容
    System.out.println("查询内容：" + all.getContent());
    System.out.println("总页数：" + all.getTotalPages());
    System.out.println("总内容条数：" + all.getTotalElements());
}
```

![image-20221019143712053](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221019143712053.png)

#### 排序测试

> 排序要确保dao类直接或间接继承了PagingAndSortingRepository类。

```java
/**
     * 测试排序
     */
@Test
public void testSort(){
    //        降序排序，如果多个字段排序，.and即可
    Sort sort = Sort.by("id").descending();
    Iterable<Customer> all = customerDao.findAll(sort);
    System.out.println(all);
}
```

<img src="https://img1.imgtp.com/2022/10/19/8VykRjY6.png" alt="1666161815771.png" title="1666161815771.png" />



### 自定义操作

> 通过继承CrudRepository和PagingAndSortingRepository有时候不能很好符合我们的需求，我们就需要自定义一些 jpql 语句来进行查询。

#### JPQL

> 通过注解@Query（“JPQL语句”）进行操作

##### 查询操作

> 根据查询返回对象设置List或者具体对象。

dao

```java
public interface CustomerDao extends PagingAndSortingRepository<Customer, Long> {

    /**
     *     JPQL语句有两种绑定值的方式，
     *     一种通过(?索引)的方式绑定值
     *          @Query("FROM Customer WHERE id<?1")
     *          List<Customer> findSomething(Long id);
     *     一种通过@Param("")和:来绑定值
     *          @Query("FROM Customer WHERE id=:id")
     *          Customer findSomething(@Param("id") Long id);
     */
    @Query("FROM Customer WHERE id<:id")
    List<Customer> findSomething(@Param("id") Long id);

}
```

测试

```java
/**
 * 测试JPQL
 */
@Test
public void testJPQL(){
    List<Customer> id = customerDao.findSomething(8L);
    System.out.println(id);
}
```

<img src="https://img1.imgtp.com/2022/10/19/06RKmtxV.png" alt="1666163243139.png" title="1666163243139.png" />



##### 修改操作

> 修改操作返回值只能是int类型，注意：JPA中的增删改都需要添加事务，并且要加上@Modifying注解告诉SpringDataJPA这是增删改操作。

dao：

```java
//    JPA 中的增删改都需要开启事务
@Transactional
//    通知SpringDataJPA这时增删改操作
@Modifying
@Query("update Customer c set c.cusName=:cusName where c.id=:id")
int updateSomething(@Param("cusName")String cusName, @Param("id")Long id);
```

测试：

```java
/**
 * 测试JPQL
 */
@Test
public void testJPQL(){
    int i = customerDao.updateSomething("小剑", 9L);
    System.out.println(i);
}
```

<img src="https://img1.imgtp.com/2022/10/19/HMjIcXj0.png" alt="1666163853016.png" title="1666163853016.png" />



##### 删除操作

dao

```java
@Transactional
//    通知SpringDataJPA这时增删改操作
@Modifying
@Query("delete from Customer where id=:id")
int deleteSomething(@Param("id")Long id);
```

测试：

```java
/**
 * 测试JPQL
 */
@Test
public void testJPQL(){
    int i = customerDao.deleteSomething(15L);
    System.out.println(i);
}
```

<img src="https://img1.imgtp.com/2022/10/19/6wujMwyd.png" alt="1666164447865.png" title="1666164447865.png" />



##### 插入操作

> JPA 本身不支持插入操作，但Hibernate提供了伪插入的一种语法，即先查出数据再插入，相当于只能插入已有数据。

dao：

```java
@Transactional
//    通知SpringDataJPA这时增删改操作
@Modifying
@Query("insert into Customer(cusName,cusAddress) select c.cusName,c.cusAddress from Customer c where c.id=:id")
int insertSomethingBySelect(@Param("id")Long id);
```

测试：

```java
/**
 * 测试JPQL
 */
@Test
public void testJPQL(){
    int i = customerDao.insertSomethingBySelect(3L);
    System.out.println(i);
}
```

<img src="https://img1.imgtp.com/2022/10/19/W6t6UZ8l.png" alt="1666165097101.png" title="1666165097101.png" />



#### SQL

> JPA 也提供了SQL 方式进行操作数据库，和 JPQL 在使用上的区别是 SQL 需要在 @Query() 中加上一个参数 nativeQuery = true。

##### 查询操作

dao：

```java
@Query(value = "select * from tb_customer where id >:id", nativeQuery = true)
List<Customer> findSomethingBySQL(@Param("id") Long id);
```

测试：

```java
/**
 * 测试SQL
 */
@Test
public void testSQL(){
    List<Customer> bySQL = customerDao.findSomethingBySQL(16L);
    System.out.println(bySQL);
}
```

<img src="https://img1.imgtp.com/2022/10/19/x06jzpRG.png" alt="1666165536503.png" title="1666165536503.png" />

[^注]: 其它增删改操作类似，当然增删改都要加上 @Transactional 和 @Modifying 注解。



#### 规定方法名

> 规定方法名是 JPA 提供的通过方法名来操作数据库的一种方式，只要按照给定格式命名，JPA 就能直接生成 SQL 语句执行。规定方法名用来针对一些简单的CRUD操作。这如果要写这一块建议看官网。这里做两个简单的举例。
>
> https://docs.spring.io/spring-data/jpa/docs/2.6.9/reference/html/#reference

###### like 查询

dao：

```java
List<Customer> findCustomerByCusAddressLike(String cusAddress);
```

测试：

```java
/**
     * 测试规定方法名
     */
@Test
public void testMethod(){
    List<Customer> customers = customerDao.findCustomerByCusAddressLike("南%");
    System.out.println(customers);
}
```

<img src="https://img1.imgtp.com/2022/10/19/5gGjELlF.png" alt="1666167051544.png" title="1666167051544.png" />



##### 删除操作

dao:

```java
@Transactional
//    通知SpringDataJPA这时增删改操作
@Modifying
int deleteCustomerByCusAddress(String cusAddress);
```

测试：

```java
/**
 * 测试规定方法名
 */
@Test
public void testMethod(){
    int i = customerDao.deleteCustomerByCusAddress("南宁");
    System.out.println(i);
}
```

<img src="https://img1.imgtp.com/2022/10/19/CLv6ByK0.png" alt="1666167196504.png" title="1666167196504.png" />



#### QueryByExample

> 这种查询是所谓的动态查询，即传入哪个参数就使用哪个参数查询，传入多个就用多个，且还可以使用match设置是否禁用某字段查询等，但只支持查询操作，并且不支持嵌套或者分组的属性约束，并且只支持**字符串匹配**。使用这种查询需要继承QueryByExampleExecutor接口。

dao：

```java
public interface CustomerQBEDao
        extends PagingAndSortingRepository<Customer, Long>
        , QueryByExampleExecutor<Customer> {
}
```

测试类：

```java
@ContextConfiguration(classes = SpringJPAConfig.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class QBETest {

    @Autowired
    public CustomerQBEDao customerQBEDao;

    @Test
    public void testQBE(){
        //允许动态匹配，有值的属性参与匹配
        Customer customer = new Customer();
        customer.setCusName("瓜");
        customer.setCusAddress("zhen");

        //构建条件匹配器
        ExampleMatcher matcher = ExampleMatcher.matching()
//                忽略某一个属性
                .withIgnorePaths("cusName")
                //忽略进行匹配的属性的大小写，如果不指定，表示忽略所有加入匹配的属性的大小写
                .withIgnoreCase("cusAddress")
//                设置进入匹配的目标字段 like '%ZHEN' and like '%瓜'
//                .withStringMatcher(ExampleMatcher.StringMatcher.ENDING)
//                仅针对某个字段进行条件匹配 like '%zhen'
                .withMatcher("cusAddress", matcher1 -> matcher1.endsWith());

        //通过 Example 构建查询属性
        Example<Customer> example = Example.of(customer, matcher);

        List<Customer> list = (List<Customer>) customerQBEDao.findAll(example);
        System.out.println(list);
    }

}
```

<img src="https://img1.imgtp.com/2022/10/19/B5at6vss.png" alt="1666175251675.png" title="1666175251675.png" />



#### Specifications

> Specifications可以看作QueryByExample的升级版本，QueryByExample只支持字符串类型，而Specifications则支持所有类型。使用Specifications需要继承 JpaSpecificationExecutor 接口。并且不支持分组、聚合函数，如果有需要，则自己通过entityManage定义。

dao类：

```java
public interface SpecificationDao extends
        PagingAndSortingRepository<Customer, Long>,
        JpaSpecificationExecutor<Customer> {
}
```

测试类：

##### 基础代码

```java
@ContextConfiguration(classes = SpringJPAConfig.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class SpecificationTest {

    @Autowired
    SpecificationDao specificationDao;
}
```

##### 精确匹配

```java
@Test
public void testSpecification(){
    //Specification是个函数式接口
    /**
         * root: 可以理解为根据目标对象生成的，可以获取对象的列属性
         * query: 组合条件 (order by, where ...)
         * criteriaBuilder: 可以用来设置各种条件(> < in ...)
         */
    List<Customer> list = specificationDao.findAll((root, query, criteriaBuilder) -> {

        //获取列属性
        Path<Long> id = root.get("id");
        Path<String> cusName = root.get("cusName");
        Path<String> cusAddress = root.get("cusAddress");

        /**
             * 参数一: 传入需要操作的属性，Expression类型对象。(Path 继承了 Expression接口)
             * 参数二: 需要匹配的值
             */
        Predicate predicateAddress = criteriaBuilder.equal(cusAddress, "SHENZHEN");
        Predicate predicateName = criteriaBuilder.equal(cusName, "冬瓜");

        //如果要拼接多个条件，则使用or 或者 and。
        Predicate predicate = criteriaBuilder.and(predicateAddress, predicateName);

        return predicate;
    });
    System.out.println(list);
}
```

<img src="https://img1.imgtp.com/2022/10/20/p4NXfFy4.png" alt="1666232548954.png" title="1666232548954.png" />

##### 范围匹配

```java
@Test
public void testSpecification(){
    //Specification是个函数式接口
    /**
         * root: 可以理解为根据目标对象生成的，可以获取对象的列属性
         * query: 组合条件 (order by, where ...)
         * criteriaBuilder: 可以用来设置各种条件(> < in ...)
         */
    List<Customer> list = specificationDao.findAll((root, query, criteriaBuilder) -> {
        //获取列属性
        Path<Long> id = root.get("id");
        Path<String> cusName = root.get("cusName");
        Path<String> cusAddress = root.get("cusAddress");

        /**
             * 参数一: 传入需要操作的属性，Expression类型对象。(Path 继承了 Expression接口)
             * 参数二: 需要匹配的值
             */
        Predicate greaterThan = criteriaBuilder.greaterThan(id, 10L); //大于
        Predicate lessThan = criteriaBuilder.lessThan(id, 12L);  //小于

        //如果要拼接多个条件，则使用or 或者 and。
        Predicate predicate = criteriaBuilder.and(greaterThan, lessThan);

        return predicate;
    });
    System.out.println(list);
}
```

<img src="https://img1.imgtp.com/2022/10/20/QGFIHxiv.png" alt="1666232724402.png" title="1666232724402.png" />

##### 排序

> 这里就需要使用到query进行组合。

```java
@Test
public void testSpecification(){
    //Specification是个函数式接口
    /**
         * root: 可以理解为根据目标对象生成的，可以获取对象的列属性
         * query: 组合条件 (order by, where ...)
         * criteriaBuilder: 可以用来设置各种条件(> < in ...)
         */
    List<Customer> list = specificationDao.findAll((root, query, criteriaBuilder) -> {
        //获取列属性
        Path<Long> id = root.get("id");
        Path<String> cusName = root.get("cusName");
        Path<String> cusAddress = root.get("cusAddress");

        /**
             * 参数一: 传入需要操作的属性，Expression类型对象。(Path 继承了 Expression接口)
             * 参数二: 需要匹配的值
             */
        Predicate greaterThan = criteriaBuilder.greaterThan(id, 10L); //大于

        Order desc = criteriaBuilder.desc(id);

        return query.where(greaterThan).orderBy(desc).getRestriction();
    });
    System.out.println(list);
}
```

<img src="https://img1.imgtp.com/2022/10/20/3dbAD5jL.png" alt="1666233313588.png" title="1666233313588.png" />

##### in查询

```java
/**
     * in 测试
     */
@Test
public void testIn(){
    List<Customer> all = specificationDao.findAll((Specification<Customer>) (root, query, criteriaBuilder) -> {
        //Path<Long> id = root.get("id");
        Expression<Long> id = root.get("id").as(Long.class);
        //传入需要 in 的属性字段
        CriteriaBuilder.In<Long> in = criteriaBuilder.in(id);
        // 将具体属性值传入
        for (long i = 0; i < 15; i++) {
            in.value(i);
        }

        Predicate predicate = criteriaBuilder.and(in);

        return predicate;
    });
    System.out.println(all);
}
```

<img src="https://img1.imgtp.com/2022/10/20/4zhZALLm.png" alt="1666250408091.png" title="1666250408091.png" />

##### 模糊匹配

```java
/**
     * 模糊匹配测试
     */
@Test
public void testVague(){
    List<Customer> all = specificationDao.findAll((Specification<Customer>) (root, query, criteriaBuilder) -> {
        Path<String> cusName = root.get("cusName");
        //            Path<Long> id = root.get("id");
        Expression<Long> id = root.get("id").as(Long.class);

        Predicate like = criteriaBuilder.like(cusName, "%瓜");
        Order desc = criteriaBuilder.desc(id);

        Predicate predicate = query.where(like).orderBy(desc).getRestriction();

        return predicate;
    });
    System.out.println(all);
}
```

<img src="https://img1.imgtp.com/2022/10/20/xjuZaTsl.png" alt="1666248749381.png" title="1666248749381.png" />



##### 动态查询

> Specifications的动态查询相对麻烦，需要挨个判断，因此如果技术中业务复杂，不太建议使用 JPA。如果是字符串类型建议直接使用QueryByExample。

```java
@Test
public void testSpecification(){
    Customer customer = new Customer();
    customer.setCusAddress("SHENZHEN");
    customer.setCusName("冬瓜");
    //Specification是个函数式接口
    /**
         * root: 可以理解为根据目标对象生成的，可以获取对象的列属性
         * query: 组合条件 (order by, where ...)
         * criteriaBuilder: 可以用来设置各种条件(> < in ...)
         */
    List<Customer> list = specificationDao.findAll((root, query, criteriaBuilder) -> {
        //获取列属性
        Path<Long> id = root.get("id");
        Path<String> cusName = root.get("cusName");
        Path<String> cusAddress = root.get("cusAddress");

        /**
             * 参数一: 传入需要操作的属性，Expression类型对象。(Path 继承了 Expression接口)
             * 参数二: 需要匹配的值
             */
        List<Predicate> preList = new ArrayList<>();
        //挨个非空判断加入List
        if (!StringUtils.isEmpty(customer.getId())){
            Predicate preId = criteriaBuilder.equal(id, customer.getId());
            preList.add(preId);
        }

        if (!StringUtils.isEmpty(customer.getCusName())){
            Predicate preName = criteriaBuilder.equal(cusName, customer.getCusName());
            preList.add(preName);
        }

        if (!StringUtils.isEmpty(customer.getCusAddress())){
            Predicate preAddress = criteriaBuilder.equal(cusAddress, customer.getCusAddress());
            preList.add(preAddress);
        }

        Predicate predicate = criteriaBuilder.and(preList.toArray(new Predicate[preList.size()]));

        return predicate;
    });
    System.out.println(list);
}
```

<img src="https://img1.imgtp.com/2022/10/20/ETUksR3F.png" alt="1666233910418.png" title="1666233910418.png" />



## 多表关联

> JPA 中并没有对多表关联提供扩展，其底层采用的仍是实现框架的多表关联机制，可以理解为使用的是 Hibernate 的机制。关联关系可以说是 Hibernate 优于 Mybatis的机制，它在使用上更方便、舒适。对于关联表的插入，查询，更新，删除都是一步到位，只要配置好各表关联关系即可。

### OneToOne

> OneToOne意味着单表与单表关联，可以是单向，也可以是双向。使用时需要在目标属性上进行配置即可（通过注解@OneToOne 和 @JoinColumn）

#### 实体类

##### Account

```java
@Data
@Table(name = "tb_account")
@Entity
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String password;

}
```

##### Customer

```java
@Entity //声明为Hibernate的实体类
@Table(name = "tb_customer")    //所映射的表
@Data
public class Customer {

    /**
     * @Id: 声明为主键
     * @GeneratedValue: 设置主键的生成策略
     *   strategy = GenerationType.IDENTITY : 自增，mysql，前提是底层数据库支持自动增长
     *              GenerationType.SEQUENCE : 序列，oracle，前提是底层数据库必须支持序列
     *              GenerationType.TABLE : JPA 提供的一种机制，通过一张数据库表的形式帮助我们完成主键自增
     *              GenerationType.AUTO : 由程序自动的帮助我们选择主键生成策略
     * @Column: 配置属性与字段的映射关系
     *   name: 数据库中对应的表字段
     */
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id; //客户主键

    @Column(name = "cus_name")
    private String cusName;

    @Column(name = "cus_address")
    private String cusAddress;

    //配置OneToOne，cascade设置在做什么操作时进行关联
    // PERSIST: 插入  MERGE: 修改  REMOVE: 删除   ALL: 所有持久化操作
    @OneToOne(cascade = CascadeType.PERSIST)
    //    外键配置
    @JoinColumn(name = "account_id")
    private Account account;

}
```

#### 测试

##### 通用代码

```java
@ContextConfiguration(classes = SpringJPAConfig.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class OTOTest {

    @Autowired
    CustomerDao customerDao;
    
}
```

##### 新增测试

> 在Customer属性上配置了插入关联，Hibernate 会自动关联插入。

```java
@Test
public void testOTO(){
    Account account = new Account();
    account.setName("张三");

    Customer customer = new Customer();
    customer.setCusName("张三");
    customer.setAccount(account);

    customerDao.save(customer);
}
```

<img src="https://img1.imgtp.com/2022/10/20/Ght7puv8.png" alt="1666236595691.png" title="1666236595691.png" />



##### 查询测试

> 查询不需要关联关系的设置，Hibernate 会自动查出来。

```java
@Test
public void testOTO(){
    Optional<Customer> byId = customerDao.findById(26L);
    System.out.println(byId.get());
}
```

<img src="https://img1.imgtp.com/2022/10/20/azYV2UtQ.png" alt="1666243967523.png" title="1666243967523.png" />



# SpringBoot Data JPA

> 前面的配置也告诉了我们，Spring Data JPA在配置上并不繁琐，只是使用上步骤稍多。所以使用SpringBoot 进行整合时步骤很简单，执行需要引入 starter 依赖即可（使用mysql存储则再引入驱动）。

## 样例

### 依赖引入

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>     
```

### 配置文件

```properties
# 应用名称
spring.application.name=03-spring-boot-jpa
# 数据库驱动：
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# 数据源名称
spring.datasource.name=dataSource
# 数据库连接地址
spring.datasource.url=jdbc:mysql://localhost:3306/springdata_jpa?serverTimezone=UTC
# 数据库用户名&密码：
spring.datasource.username=root
spring.datasource.password=2002
# 应用服务 WEB 访问端口
server.port=8080

#是否显示sql在控制台
spring.jpa.show-sql=true
```

### 实体类

```java
@Entity //声明为Hibernate的实体类
@Table(name = "tb_customer")    //所映射的表
@Data
public class Customer {

    /**
     * @Id: 声明为主键
     * @GeneratedValue: 设置主键的生成策略
     *   strategy = GenerationType.IDENTITY : 自增，mysql，前提是底层数据库支持自动增长
     *              GenerationType.SEQUENCE : 序列，oracle，前提是底层数据库必须支持序列
     *              GenerationType.TABLE : JPA 提供的一种机制，通过一张数据库表的形式帮助我们完成主键自增
     *              GenerationType.AUTO : 由程序自动的帮助我们选择主键生成策略
     * @Column: 配置属性与字段的映射关系
     *   name: 数据库中对应的表字段
     */
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long id; //客户主键

    @Column(name = "cus_name")
    private String cusName;

    @Column(name = "cus_address")
    private String cusAddress;
}
```

### 控制层

```java
@RestController
public class CustomerController {

    //这里就不进行service dao分层了
    @Autowired
    CustomerDao customerDao;

    @GetMapping("/test")
    public Iterable<Customer> list(){
        return customerDao.findAll();
    }

}
```

### 执行效果

<img src="https://img1.imgtp.com/2022/10/20/BQs7907Q.png" alt="1666246347782.png" title="1666246347782.png" />

## 自动配置原理

> SpringBoot JPA 的自动配置主要通过两个类完成的：JpaBaseConfiguration 和 JpaRepositoriesAutoConfiguration两个自动配置类完成的。

