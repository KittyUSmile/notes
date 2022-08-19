#  一、MyBatis简介

## 1、Mybatis特性

1. Mybatis是一个支持定制化SQL、存储过程以及高级映射的优秀的持久层框架。
2. Mybatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。
3. Mybatis可以使用简单的XML或注解将接口和Java中的POJO对象映射成数据库中的记录。
4. Mybatis是一个半自动的ORM（Object Relation Mapping）框架。



#  二、Mybatis的基本使用



## 1、使用步骤

1. 首先引入依赖，然后配置Mybatis的核心配置文件（配置连接信息、引入mapper映射文件），名字可以随意取。

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/><!-- 单独使用时配置成MANAGED没有事务 -->
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/user?serverTimezone=UTC"/>
                   <property name="username" value="root"/>
                   <property name="password" value="2002"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
           <mapper resource="mapper/UserInfoDaoMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

   如果觉得使用value直接定义数据源配置不方便，可以通过绑定properties的方式进行绑定：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <properties resource="jdbc.properties"/>
   
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/><!-- 单独使用时配置成MANAGED没有事务 -->
               <dataSource type="POOLED">
                   <property name="driver" value="${jdbc.driver}"/>
                   <property name="url" value="${jdbc.url}"/>
                   <property name="username" value="${jdbc.username}"/>
                   <property name="password" value="${jdbc.password}"/>
               </dataSource>
           </environment>
       </environments>
   
       <mappers>
           <mapper resource="mapper/UserInfoDaoMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

   

2. 创建出dao接口（Mybatis通过cglib动态代理实现该接口，并调用对应的mapper文件中sql语句）。

   ```java
   public interface UserInfoDao {
   
       int addUser(@Param("userInfo") UserInfo userInfo);
   
   }
   ```

   

3. 再创建对应表的实体类（Mybatis有一套默认字段和属性的映射规则，因此实体类中属性要与数据库表属性相对应）

   ```java
   @Data
   public class UserInfo {
   
       private String name;
   
       private String password;
   
   }
   ```

4. 创建出对应dao接口的mapper.xml文件（一个dao接口对应一个mapper，因此mapper可以创建在resource目录下，并且名字要和dao相同），在mapper.xml文件中配置其对应的dao接口（通过namespace全类名设置）：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.test1.dao.UserInfoDao">
   
       <insert id="addUser">
           insert into user_info(name, money) values(#{userInfo.name}, #{userInfo.password})
       </insert>
   
   </mapper>
   ```

   然后mapper.xml中编写的sql语句中的 id 要与其对应的 dao 接口中的方法名一致，Mybatis底层才能找到接口方法对应的sql语句。

   

   测试（在Mybatis核心配置文件中设置为JDBC事务，因此SqlSession不会自动提交，需要进行设置提交）：

   ```java
   @Test
   public void test1() throws IOException {
       //获取配置类的输入流
       InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
       //获取SqlSessionFactoryBuilder来构建SqlSessionFactory
       SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
       //获取SqlSessionFactory对象(传入获取的配置类的流对象)
       SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
       //获取SqlSession对象，SqlSession会话对象即一次与数据库连接的会话，类似于Servlet中的Session。参数中true表示自动提交
       SqlSession sqlSession = sqlSessionFactory.openSession(true);
       //通过SqlSession对象获取dao接口的代理实现类
       UserInfoDao userInfoDao = sqlSession.getMapper(UserInfoDao.class);
       UserInfo userInfo = new UserInfo();
       userInfo.setName("小龙");
       userInfo.setPassword("123");
       userInfoDao.addUser(userInfo);
   }
   ```

## 2、注意事项

1. Mybatis的配置文件名字可以随意写，获取配置文件可以通过名字获取。
2. dao接口名字要与mapper.xml文件名一致，因为Mybatis底层是通过名字来获取的。
3. mapper.xml中 sql 的 id 名要与 dao 接口中的方法名相对应。



## 3、查询功能

这里只讨论查询功能，因为增删改的返回值都只有执行是否成功的结果，而查询功能则需要获取和封装返回值。

首先查询结果需要与一个实体类进行映射，映射规则可以使用默认映射规则（resultType）或自定义映射规则（resultMap）。resultType一般用来映射字段名与实体类属性名一致的情况，而resultMap则一般用来处理返回字段名与实体类属性名不一致或者需要处理一对多多对一的关系时的情况。

默认映射规则使用如：

```xml
<select id="selectUserById" resultType="com.test1.pojo.UserInfo">
	select id, name, money from user_info where id = #{id};
</select>
```

即数据库查询信息的字段名与设置的实体类属性名进行对应赋值，名字相同的进行赋值。



##  4、类型别名设置

类型别名即进行查询功能时，设置的返回值类型都是全类名，又长使用又不方便，因此 mybatis 提供了一种标签进行简化：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>

<!--    设置类型别名-->
    <typeAliases>
<!--        <typeAlias type="com.test1.pojo.UserInfo" alias="UserInfo"/>-->
<!--        如果不设置别名，则默认为去掉全类名的类名-->
<!--        <typeAlias type="com.test1.pojo.UserInfo"/>-->
<!--        package是最常用的方式，以包为单位，为包下所有类设置默认类名为别名，且不区分大小写-->
        <package name="com.test1.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/><!-- 单独使用时配置成MANAGED没有事务 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="mapper/UserInfoDaoMapper.xml"/>
    </mappers>

</configuration>
```

顺便提一下，在mybatis配置文件中，配置需要按照某些顺序前后设置，这是规则，前后顺序如下：

```
properties
settings
typeAliases
typeHandlers
objectFactory
objectWrapperFactory
reflectorFactory
plugins
environments
databaseIdProvider
mappers
```

## 5、统一引入mapper

之前写的与 dao 绑定的 mapper配置为：

```xml
<mappers>
	<mapper resource="mapper/UserInfoDaoMapper.xml"/>
</mappers>
```

使用这种方式的话一次只能引入一个xml文件，很不方便，因此mybatis还提供了引入一个包下所有xml文件的方式：

```xml
<mappers>
	<package name="com.test1.dao"/>
</mappers>
```

但是使用这种方式一定要满意一下两个条件：

1. mapper接口所在的包必须和映射文件所在的包一致（在resources资源目录基础上创建）
2. mapper接口必须与映射文件名字相同

在resources目录下创建目录可以单级创建或者多级创建，多级创建以/分隔，不能以.分隔，如com/run/dao。



## 6、${}和#{}

这两个符号是mybatis中最重要的知识点之一。

${}本质上是字符串拼接。类似手写sql语句时的 + 拼接。

#{}本质上是占位符赋值。类似预编译sql的 ? 占位。



## 7、获取参数值

当将dao接口中的方法与mapper中方法绑定好后，如何处理传递的参数也是一个问题。

### 1、参数只有一个

当传递的参数只有一个时，使用${}和#{}都可以，并且Mybatis有相关的处理，mapper文件对应的 sql 语句中使用的参数名可以为任意。

```java
public interface UserInfoDao {
    UserInfo selectUserByName(String name);
}
```

```xml
<mapper namespace="com.test1.dao.UserInfoDao">
    <select id="selectUserByName" resultType="com.test1.pojo.UserInfo">
        select * from user_info where name = #{name}
    </select>
</mapper>
```

但是使用${}与#{}还是有区别的。

${}本质上是字符串拼接，当拼接字符串类型参数时，需要使用‘ ’括起来，而#{}则是占位符，MySQL底层会预先进行编译操作，再将#{}中的内存作为参数传入。



### 2、有多个参数

当有多个参数时，如果不加以区分，Mybatis底层就无法确定参数该怎么赋值，因此这里有多种处理方式：

1、**通过#{参数名}，‘${param1}‘…进行获取**

​	mybatis底层会将参数存储在一个 map 中，并按照参数传入顺序存储为参数名1、参数名2、参数名3…和param1、param2、param3…。这两种存储方式的KV存储的都是参数，不同的只是按照自己需求进行获取，例如参数名1的与 param1获取的是同一个参数。使用时可以通过${}拼接或者#{}占位进行获取，只不过${}遇到字符串类型时需要用‘ ’括起。

```java
int addUser(String name, String money);
```

```xml
<insert id="addUser">
    insert into user_info(name, money) values(#{name}, #{param2})
</insert>
```

2、**手动传入map集合，自定义键值**

​	使用方式与第一种方式一致，唯一不同点是我们可以自定义参数名字。

```java
UserInfo selectUserByMap(Map<String, Object> map);
```

```xml
<select id="selectUserByMap" resultType="com.test1.pojo.UserInfo">
    select * from user_info where id = #{id} and name = #{name}
</select>
```

3、**通过#{实体类属性}直接获取**

​	当传入的传输为一个实体类时，mapper中的 sql 语句可以直接通过 #{属性名} 或者 ${属性名} 进行获取值。这里仍要注意${}的拼接问题。

```java
int addUser(UserInfo userInfo);
```

```xml
<insert id="addUser">
    insert into user_info(name, money) values(#{name}, #{money})
</insert>
```

4、**@Param命名参数**

其实这种方式是第一种方式的升级版，其存储进 map 时使用的不是默认传入值名字或者param1这种命名方式，而是我们通过@Param(“”)设置的属性名进行设置的。

```java
UserInfo selectUserById(@Param("id") Integer id);
```

```xml
<select id="selectUserById" resultType="UserInfo">
    select id, name, money from user_info where id = #{id};
</select>
```

# 三、Mybatis功能使用

这里只讨论通过Mybatis对数据的操作功能和返回值接收情况。

## 1、返回一条或多条记录

当返回一条记录时，可以直接通过实体类进行接收，也可以用 List或者 Map 进行接收。如果使用 Map 进行接收，那么对应的 key 就是字段名，value 就是字段属性，在查询没有对应实体类型的数据时很常用。

```java
Map<String, Object> getUserByIdToMap(Integer id);
```

```xml
<select id="getUserByIdToMap" resultType="java.util.Map">
    select * from user_info where id = #{id}
</select>
```

如果返回多条记录，就只能通过 List 或 Map进行接收。

使用List接收：

```java
List<UserInfo> selectAllUser();
```

```xml
<select id="selectAllUser" resultType="userInfo">
    select * from user_info
</select>
```

使用Map接收：

使用 Map 接收多条数据时，需要指定按什么值进行分类存，如果不指定，就会直接将查出来的一条数据的字段名作为key，字段属性作为 value。而使用 Map 存储相同的多条数据时，可以使用主键作为 key，查出来的属性作为 value。

因此需要使用到一个注解 @MapKey(“”)，该注解的用法是指定查出来的数据中的哪一个字段作为 key。该注解可以说是Map作容器接收多条数据时的御用注解。

```java
@MapKey("id")
Map<String, Object> getAllUserToMap();
```

```xml
<select id="getAllUserToMap" resultType="java.util.Map">
    select * from user_info
</select>
```

## 2、返回各种数据类型

mybatis底层使用了多种别名对 Java 提供的类型进行处理，如若返回 int 类型，在返回值类型上可以填入：全类名、int、Integer、Int、integer、_int等多种名称。

```java
int countUser();
```

```xml
<select id="countUser" resultType="int">
	select count(1) from user_info
</select>
```



## 3、处理模糊查询

模糊查询的处理就可以了解到为什么 ${} 不如 #{} 安全却仍有使用价值。在使用模糊查询时，模糊匹配的规则是‘%模糊匹配对象%’，如果仍使用 #{} 进行占位预编译，结果会是‘%?%’，这个?会被看作字符串的一部分，不会对其继续赋值，因此 #{} 无法生效。

因此处理模糊查询的方式有两种：

1、使用 ${} 进行字符串拼接完成模糊查询。

```java
@MapKey("id")
Map<String, Object> selectByLikeName(String name);
```

```xml
<select id="selectByLikeName" resultType="map">
	select * from user_info where name like '%${name}%'
</select>
```

2、使用 concat 函数和 #{} 进行字符串拼接。

```java
@MapKey("id")
Map<String, Object> selectByLikeName(String name);
```

```xml
<select id="selectByLikeName" resultType="map">
    select * from user_info where name like concat('%',#{name},'%')
</select>
```

3、直接通过 #{} 进行拼接（最常用）

```java
@MapKey("id")
Map<String, Object> selectByLikeName(String name);
```

```xml
<select id="selectByLikeName" resultType="map">
	select * from user_info where name like "%"#{name}"%"
</select>
```

## 4、批量删除

通过mybatis批量删除数据库中的记录。此处不能使用 #{} 。因为 #{} 在进行编译时会自动加上 ‘ ’ 单引号 。

```java
int deleteByBatch(String ids);
```

```xml
<delete id="deleteByBatch">
	delete from user_info where id in (${ids})
</delete>
```



## 5、动态设置表名

动态设置表名的应用在分表后的场景，即需要动态查询不同的表。此处也只能使用 ${} 拼接字符串，而不能使用 #{}。

```java
List<UserInfo> getTableList(String tableName);
```

```xml
<select id="getTableList" resultType="com.test1.pojo.UserInfo">
	select * from ${tableName}
</select>
```



## 6、获取add数据的主键

通过Mybatis获取add数据时的主键数据。即获取插入数据的主键信息。

```java
void insertAndGetId(UserInfo userInfo);
```

```xml
<!--    
       useGeneratedKeys：表示使用了自增的主键
       keyProperty：将自增的主键的值赋给实体类中的某个属性
-->
<insert id="insertAndGetId" useGeneratedKeys="true" keyProperty="id">
    insert into user_info values(null, #{name}, #{money});
</insert>
```

sql 语句执行完后，该userInfo中的 id 属性会被设置为插入数据的主键值。



## 7、解决字段名与属性名不一致问题

一般而言，数据库中的字段名都是以下划线分隔，而实体类属性都是驼峰命名法，因此在进行查询赋值时会出现无法赋上值的问题，这里提供几种解决这个问题的方法。

### 1、修改查询字段名

通过在查询时修改查询的字段名来对应实体类属性名。

```java
List<UserInfo> selectAllUser();
```

```xml
<select id="selectAllUser" resultType="com.test1.pojo.UserInfo">
    select id, name, money, d_id dId from user_info
</select>
```

### 2、通过全局配置设置

通过mybatis提供的配置进行设置：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <!--        开启下划线转驼峰配置，默认时false关闭-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

</configuration>
```

然后就可以直接进行查询了，mybatis底层会默认进行映射了：

```xml
<select id="selectAllUser" resultType="com.test1.pojo.UserInfo">
	select * from user_info
</select>
```

### 3、自定义映射规则

通过自定义映射规则进行映射，即通过resultMap进行映射。建议使用resultMap时最好将所有属性与字段进行对应，即使它们是相等的。

```java
List<UserInfo> selectAllUser();
```

```xml
<resultMap id="userInfoResultMap" type="UserInfo">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="money" column="money"/>
    <result property="dId" column="d_id"/>
</resultMap>

<select id="selectAllUser" resultMap="userInfoResultMap">
    select * from user_info
</select>
```



## 8、解决属性赋值中多对一的映射

在实体类属性中，常有其它实体类作为属性而存在，而在数据库表中，这又是两张表，如何能通过映射将查询出来的数据嵌套映射到实体类属性中呢？这也有多种方式进行映射。

### 1、通过属性.属性进行映射

```java
UserInfo selectById(Integer id);
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test1.dao.UserInfoDao">

    <resultMap id="userInfoResultMap" type="UserInfo">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="money" column="money"/>
        <result property="dept.deptId" column="dept_id"/>
        <result property="dept.deptName" column="dept_name"/>
    </resultMap>

    <select id="selectById" resultMap="userInfoResultMap">
        select *
        from user_info
        left join dept_info
        on user_info.dept_id = dept_info.dept_id
        where user_info.id = #{id}
    </select>
</mapper>
```

### 2、通过association标签映射

```xml
<resultMap id="userInfoResultMap" type="UserInfo">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="money" column="money"/>
    <!--
        association：专门用来处理多对一的映射关系
        javaType：需要映射的属性类型
-->
    <association property="dept" javaType="Dept">
        <id property="deptId" column="dept_id"/>
        <id property="deptName" column="dept_name"/>
    </association>
</resultMap>

<select id="selectById" resultMap="userInfoResultMap">
    select *
    from user_info
    left join dept_info
    on user_info.dept_id = dept_info.dept_id
    where user_info.id = #{id}
</select>
```

### 3、通过association分布查询

也可以通过association进行分布查询，即将查询分为多步进行，其中次步在association标签中定义。这里可以解释一下为何要将本来简单的association分成两步查询进行，这样可以解耦合，分布的sql语句其它方法也能调用。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test1.dao.UserInfoDao">

    <resultMap id="userInfoResultMap" type="UserInfo">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="money" column="money"/>
<!--
        select：表示分布查询使用的 sql 语句
        column：传入 sql 语句的条件
 -->
        <association property="dept"
                     select="com.test1.dao.UserInfoDao.selectDeptById"
                     column="dept_id">
        </association>
    </resultMap>

    <select id="selectById" resultMap="userInfoResultMap">
        select * from user_info where id = #{id}
    </select>

    <select id="selectDeptById" resultType="Dept">
        select * from dept_info where dept_id = #{dept_id}
    </select>
</mapper>
```

#### 1、延迟加载

使用association可以实现延迟加载，延迟加载即进行具有分布查询的sql在执行时，若调用者只调用了分布查询中第一步就能查出来的数据时，就不会进行第二步查询。只有对分布查询的数据有需要时，才会进行查询。

然后如果需要使用延迟加载功能，就需要在mybatis的<setter>标签中进行设置：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <settings>
        <!--        开启下划线转驼峰配置，默认时false关闭-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
<!--        延迟加载的全局开关，当开启时，所有关联对象都会延迟加载，默认是false关闭-->
        <setting name="lazyLoadingEnabled" value="true"/>
<!--        与延迟加载正好相反，当开启时，只要有方法调用，都会直接加载该对象的所有属性
            如果需要使用延迟加载功能，就要将这个属性关闭-->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>

</configuration>
```

通过mybatis配置文件配置了延迟加载的话，即所有分布查询都是延迟加载了，如果想要某个分布查询直接一步全部查完而不是延迟加载，就可以在assiciation中设置属性fetchType：

这种设置方式只有当延迟加载全局配置功能开启后才生效，否则无论是否设置都是直接加载。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test1.dao.UserInfoDao">

    <resultMap id="userInfoResultMap" type="UserInfo">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="money" column="money"/>
<!--
        select：表示分布查询使用的 sql 语句
        column：传入 sql 语句的条件
 -->
        <association property="dept"
                     select="com.test1.dao.UserInfoDao.selectDeptById"
                     column="dept_id"
                     fetchType="eager">
<!--            设置加载方式为立即加载。共有两种加载方式：lazy（延迟加载），eager（立即加载）-->
        </association>
    </resultMap>

    <select id="selectById" resultMap="userInfoResultMap">
        select * from user_info where id = #{id}
    </select>

    <select id="selectDeptById" resultType="Dept">
        select * from dept_info where dept_id = #{dept_id}
    </select>
</mapper>
```

## 9、解决属性赋值中一对多的映射

一般而言一对多都是使用List<>进行包含的，因此就可以通过collection属性进行映射：

```java
Dept selectById(Integer id);
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test1.dao.UserInfoDao">

    <resultMap id="userInfoResultMap" type="Dept">
        <id property="deptId" column="dept_id"/>
        <result property="deptName" column="dept_name"/>
<!--        collection：专门处理一对多关系的标签
			property：属性名
            ofType：集合中的属性类型
-->
        <collection property="users" ofType="UserInfo">
            <id property="id" column="id"/>
            <result property="name" column="name"/>
            <result property="money" column="money"/>
        </collection>
    </resultMap>

    <select id="selectById" resultMap="userInfoResultMap">
        SELECT * FROM `dept_info` left join user_info on user_info.dept_id = dept_info.dept_id where dept_info.dept_id = #{id}
    </select>

</mapper>
```

以上方式也是直接进行的一步查询，同样也可以进行分布查询。

#### 1、分布查询

当然分布查询也可以进行延迟加载，只要开启了即可。

```java
Dept selectById(Integer id);
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.test1.dao.UserInfoDao">

    <resultMap id="userInfoResultMap" type="Dept">
        <id property="deptId" column="dept_id"/>
        <result property="deptName" column="dept_name"/>
        <collection property="users"
                    select="selectUserInfoById"
                    column="dept_id">
        </collection>
    </resultMap>

    <select id="selectById" resultMap="userInfoResultMap">
        select * from dept_info where dept_id = #{dept_id};
    </select>

    <select id="selectUserInfoById" resultType="UserInfo">
        select * from user_info where dept_id = #{dept_id}
    </select>

</mapper>
```

## 10、动态SQL标签

动态SQL的存在是为了解决当传递参数不确定时，能够按照参数动态操作数据库。动态SQL标签有<if>、<where>、<trim>、<choose><when><otherwise>、<foreach>等。

### 1、if 标签

<if>标签可以通过test属性的表达式进行判断，若表达结果为true，则执行<if>标签里的内容，反之标签中的内存不会执行。

```java
List<UserInfo> selectUserInfo(UserInfo userInfo);
```

```xml
<select id="selectUserInfo" resultType="UserInfo">
    select * from user_info where 1 = 1
    <if test="id != '' and id != null">
        and id = #{id}
    </if>
    <if test="name != '' and name != null">
        and name = #{name}
    </if>
    <if test="money != '' and money != null">
        and money = #{money}
    </if>
</select>
```



### 2、where 标签

<where>标签的作用就是提供mysql中where条件过滤作用。只不过mybatis对其进行了优化。使用<where>标签会对其中的sql语句进行优化，会动态删减sql语句前and和or连接符并为我们智能添加and，当然只有语句前，语句后的and，where标签不会处理。

且当where标签中没有内容时，where标签不会生效。

```java
List<UserInfo> selectUserInfo(UserInfo userInfo);
```

```xml
<select id="selectUserInfo" resultType="UserInfo">
    select * from user_info
    <where>
        <if test="id != '' and id != null">
            id = #{id}
        </if>
        <if test="name != '' and name != null">
            name = #{name}
        </if>
        <if test="money != '' and money != null">
            money = #{money}
        </if>
    </where>
</select>
```



### 3、trim 标签

trim标签的作用是对标签内整体sql的前后添加或删除标签，对局部sql的前后添加或删除标签。同样当标签中没有内容时，trim标签也没有任何效果。

标签内共有四个可设置属性：

1. prefix：在trim标签内容前面添加指定内容
2. suffix：在trim标签内容后面添加指定内容
3. prefixOverrides：在trim标签中内容前面去掉指定内容
4. suffixOverrides：在trim标签中内容后面去掉指定内容

```java
List<UserInfo> selectUserInfo(UserInfo userInfo);
```

```xml
<select id="selectUserInfo" resultType="UserInfo">
    select * from user_info
    <trim prefix="where" suffixOverrides="and|or">
        <if test="id != '' and id != null">
            id = #{id} and
        </if>
        <if test="name != '' and name != null">
            name = #{name} or
        </if>
        <if test="money != '' and money != null">
            money = #{money}
        </if>
    </trim>
</select>
```



### 4、choose、when、otherwise标签

这三个标签是用一起的，相等于在<choose>中进行选择<when>，当选择进入一个<when>后就不再执行其它<when>或者<otherwise>，当没有<when>满足条件时，最终会进入<otherwise>执行。所以一个<choose>中至少要存在一个<when>，而可以不存在<otherwise>。

```java
List<UserInfo> selectUserInfo(UserInfo userInfo);
```

```xml
<select id="selectUserInfo" resultType="UserInfo">
    select * from user_info
    <where>
        <choose>
            <when test="id != '' and id != null">
                id = #{id}
            </when>
            <when test="name != '' and name != null">
                name = #{name}
            </when>
            <when test="money != '' and money != null">
                money = #{money}
            </when>
            <otherwise>

            </otherwise>
        </choose>
    </where>
</select>
```



### 5、foreach 标签

foeach标签用来执行需要批量进行的操作。foreach标签中常用属性有：

1. collection：表示用来循环的数组或集合
2. item：数组或集合中的单个元素
3. separator：拼接时元素之间的分隔符
4. open：开启拼接前的内容以什么符号开始
5. close：结束拼接后的内容以什么符号结尾

如循环拼接删除 id 进行 in 方式批量删除：

```java
int deleteBatch(@Param("ids") Integer[] ids);
```

```xml
<delete id="deleteBatch">
    delete from user_info where id in
    <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
</delete>
```

或者循环拼接删除 id 进行 or 方式批量删除：

```java
int deleteBatch(@Param("ids") Integer[] ids);
```

```xml
<delete id="deleteBatch">
    delete from user_info where
    <foreach collection="ids" item="id" separator="or">
        id = #{id}
    </foreach>
</delete>
```



### 6、sql 标签

作为可重用的 sql 片段被其它语句引入使用。其它sql通过include进行引入。

```java
List<UserInfo> selectAllUser();
```

```xml
<select id="selectAllUser" resultType="com.test1.pojo.UserInfo">
    select <include refid="allArgs"/> from user_info
</select>

<sql id="allArgs">id, name, money, dept_id</sql>
```



# 四、Mybatis缓存

mybatis引入了缓存的机制，能够缓存查询过的sql语句，减少响应时间。缓存只能减少我们重复查询的时间，而不会影响我们查询的结果。

mybatis共有两级缓存，第一级缓存是默认开启的，是SqlSession级别的，即同一个SqlSession查询出来的数据会被缓存起来，下次遇到相同的查询就直接从一级缓存中取。

第二级缓存默认是不开启的，需要手动开启，是SqlSessionFactory级别的，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存起来，此后再执行相同的sql语句就会直接从缓存中取。

## 1、一级缓存

### 1、一级缓存失效的情况

1. 使用不同的SqlSession。
2. 使用同一个SqlSession，但是查询不同。
3. 同一个SqlSession两次相同查询之间执行了任何一次增删改操作（执行了任意增删改操作后mybatis会主动清空所有缓存）。
4. 同一个SqlSession两次相同查询之间清空了缓存（mybatis自动执行或者手动执行了SqlSession的clearCache()方法）。



## 2、二级缓存

### 1、开启二级缓存的条件（同时满足）：

1. 在mybatis的核心配置文件中，设置全局配置属性cacheEnabled=“true”，当然，mybatis默认该属性为true。

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <settings>
           <setting name="cacheEnabled" value="true"/>
       </settings>
   </configuration>
   ```

2. 在映射文件mapper中设置标签<cache/>

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.test1.dao.UserInfoDao">
   <!--开启缓存标签-->
       <cache/>
   </mapper>
   ```

3. 二级缓存必须在SqlSession关闭或提交后才有效（只有当SqlSession关闭或手动提交后才会将缓存数据保存在二级缓存中）。

4. 查询的数据所转换的实体类类型必须要实现序列化的接口。



### 2、二级缓存失效情况

当两次查询之间执行了任意的增删改，就会使一级和二级缓存同时失效。而一级缓存手动清空缓存只会清空SqlSession中的缓存，而不会清空二级缓存。



### 3、二级缓存相关配置

即在开启二级缓存的第二步中<cache/>标签中进行配置，即在进行二级缓存时配置的一些缓存策略。

该标签中可以设置的属性有：

1. **eviction属性**：缓存回收策略

   1. LRU（Least Recently Used）- 最近最少使用：排除最长时间内不被使用的缓存数据对象。

   2. FIFO（First in First out）- 先进先出：按照缓存数据对象进入缓存的顺序来移除它们。

      …

      默认采用的是LRU回收策略。

2. flushInterval属性：刷新间隔，单位为毫秒。刷新即清空缓存。

   默认情况是无刷新间隔，缓存仅仅在调用增删改语句时刷新。

3. **size属性**：引用数目，只能为正整数。

   即设置缓存中最多可以存储多少个缓存数据对象，不能设置太大，否则容易内存溢出。

4. **readOnly属性**：只读属性，true/false。

   该属性可以设置为true/false，当设置为true是时，通过缓存获取的缓存对象就是存储在二级缓存中的缓存对象的引用，不能被修改，否则缓存数据就会不准确。

   当设置为false时，表示可修改，即通过缓存获取的缓存对象是二级缓存中的缓存对象的拷贝（通过序列化拷贝），允许对其进行操作，速度相对较慢，但安全，因此Mybatis默认设置该属性为false。



## 3、缓存查询顺序

思考这个问题前，可以先思考查询缓存是先通过大范围查询更快，还是小范围更快。答案是大范围，因为大范围中可能有其他小范围已查出来的数据，可以直接拿来使用，因此mybatis中的缓存查询机制也是如此。

缓存查询顺序：

1. 先查询二级缓存，因为二级缓存中可能存在一级缓存已经查询出来的数据，可以拿来直接使用。
2. 如果二级缓存没有命中，再去查询一级缓存。
3. 如果一级缓存没有命中，再去数据库查询，并将查询结果放入一级缓存。
4. SqlSession关闭或提交后，一级缓存中的数据会写入二级缓存中。



## 4、第三方缓存

mybatis还可以引入第三方缓存插件，因为mybatis自觉做缓存相对不够专业，因此提供出二级缓存的接口，允许其它第三方技术来完善二级缓存机制，而一级缓存则不开放第三方。



# 五、Mybatis的逆向工程

先了解下正向和逆向工程：

- 正向工程：先创建出Java实体类，再由框架或其它根据实体类生成数据库表。Hibernate支持正向工程。

- 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成以下资源等：

  - Java实体类

  - Mapper接口

  - Mapper映射文件

    …



# 六、分页插件

使用分页前要了解下分页的参数和公式：

分页参数有：index（当前页的起始索引，即位于数据库中的位置）、pageSize（每页显示的条数）、pageNum（当前页的页码）。

公式：index = (pageNum - 1) * pageSize。

我们使用分页功能时，通常是通过 **页码 + 每页条数** 来进行选择，而后台物理分页只能通过 **数据索引 + 条数** 进行limit。因此需要记住分页公式。

分页插件有很多，可以使用pageHelper插件或者其它分页插件进行分页。

使用pageHelper插件时，需要先添加依赖：

```xml
<dependency>
	<groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>xxxxx</version>
</dependency>
```

然后在mybatis的核心配置文件中配置分页插件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <plugins>
<!--        设置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
</configuration>
```

然后就可以使用该插件实现分页功能了。使用步骤为：

1. 在查询功能之前开启分页功能

   PageHelper.startPage(int pageNum, int pageSize);

2. 在查询功能之后获取分页相关信息

   PageInfo<Emp> page = new PageInfo<>(list, 5); //limit表示分页数据，5表示导航分页的数量