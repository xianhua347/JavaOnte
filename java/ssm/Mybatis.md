# Mybatis

## 1.简介

#### MyBatis 是一款优秀的`持久层框架`，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Ordinary Java Object,普通的 Java对象)映射成数据库中的记录。



## 2.环境配置

#### 在pom.xml中引入Mybatis mysql 仓库

```xml

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.3.1</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>
```

#### 配置mybatis-config.xml和config.properties

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="config.properties"/>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property value="${driver}" name="driver"/>
                <property value="${url}" name="url"/>
                <property value="${username}" name="username"/>
                <property value="${password}" name="password"/>
            </dataSource>
        </environment>

    </environments>
    <mappers>
        <mapper resource=""/>
    </mappers>
</configuration>
```



```properties
driver=com.mysql.cj.jdbc.Driver
url = jdbc:mysql://localhost:3306/school?serverTimezone=GMT%2B8&useSSL=false&&useUnicode=true&&CharacterEncoding=utf-8
username=root
password=1

```

### 配置MyBatisUtils工具类

```java
package com.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybitisUtils {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            String resource = "mybitis-config.xml";
            InputStream resourceAsStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory= new SqlSessionFactoryBuilder().build(resourceAsStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}

```

#### 配置mapper接口和mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">
</mapper>
```



```java
public interface Mapper {
 }
```

## 3. CRUD

#### 编写Mapper接口

```java
package com.dao;

import com.pojo.Websites;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

public interface WebsitesMapper {
    List<Websites> getWebsitesMapper();
    //查询数据库
    Websites selectById(int id);
    //添加
    void addWebsites(Websites website);
    //更新
    void updateWebsites(Websites website);
    //删除
    void deleteWebsites(int id);
    List<Websites>getWebsitesByLimit(Map<String,Integer>map);
 }

```

#### 编写Mapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.WebsitesMapper">
    <!--    <resultMap id="mapper" type="Websites">-->
    <!--        <result property="id" column="id"/>-->
    <!--        <result property="w_url" column="url"/>-->
    <!--    </resultMap>-->
    <select id="getWebsitesMapper" resultType="list">
        select *
        from school.websites
    </select>
    <select id="selectById" resultType="com.pojo.Websites" parameterType="int">
        select *
        from school.websites
        where id = #{id}
    </select>
    <insert id="addWebsites" parameterType="com.pojo.Websites">
        insert into school.websites(id, name, url, alexa, country) VALUE (#{id}, #{w_name}, #{url}, #{alexa}, #{country});
    </insert>
    <update id="updateWebsites" parameterType="com.pojo.Websites">
        update school.websites
        set id=#{id},
            name=#{w_name},
            url=#{url},
            alexa=#{alexa},
            country=#{country}
        where id = #{id}
    </update>
    <delete id="deleteWebsites" parameterType="_int">
        delete
        from school.websites
        where id = #{id};
    </delete>

    <select id="getWebsitesByLimit" parameterType="map" resultType="com.pojo.Websites">
        select *
        from school.websites
        limit #{strartPage},#{endPage};
    </select>
</mapper>
```

- resultType 是返回值类型 也就是Mapper接口返回值类型
- parameterType 是参数类型 也就是Mapper参数类型
- namespace 映射到Mapper接口

#### 编写测试类

```java
package com;

import com.dao.WebsitesMapper;
import com.pojo.Websites;
import com.utils.MybitisUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.HashMap;
import java.util.List;

public class Test {
    @org.junit.jupiter.api.Test
    public void getId(){
        SqlSession sqlSession = MybitisUtils.getSqlSession();
        //使用工具类方法 得到sqlsession
        WebsitesMapper mapper = sqlSession.getMapper(WebsitesMapper.class);
        Websites idInfo = mapper.selectById(2);
        System.out.println(idInfo);
    }   
}
```



## 4. 模糊查询

#### 1.java代码传入通配符% %

``` java
List<User> userList=mapper.getUserList("%类%")
```

#### 2.sql拼接通配符 (有注入风险)

```sql
select * from mybatis.user where name like "%"#{value} "%"
```

## 5.分页
1. ### 使用limit语句
编写Mapper接口
```java
 List<User> findByPager(Map<String, Object> params);

```
编写Mapper.xml
```xml
<select id="findByPager" resultType="com.xxx.mybatis.domain.User">
	select * from xx_user limit #{strartPage},#{endPage}
</select>
```
实现方法

```java
SqlSession sqlSession = MybitisUtils.getSqlSession();
        WebsitesMapper mapper = sqlSession.getMapper(WebsitesMapper.class);
        HashMap<String, Integer> stringIntegerHashMap = new HashMap<>();
        stringIntegerHashMap.put("strartPage", 0);
        stringIntegerHashMap.put("endPage", 2);
        List<Websites> websitesByLimit = mapper.getWebsitesByLimit(stringIntegerHashMap);
        for (Websites websites : websitesByLimit) {
            System.out.println(websites);
        }
        sqlSession.close();

```
2. ### 使用pagehelper实现

   **https://pagehelper.github.io/docs/howtouse/**


## 6.config.xml 常用配置

#### 1.proerties(外部数据库配置文件)

```xml
<properties resource="org/mybatis/example/config.properties">
    //proerties文件位置
  <property name="username" value="dev_user"/>
    //属性名和值
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```



```xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

#### 2.settings(设置文件)

- jdbcTypeForNull 把数据库格式转换成驼峰命名
- logImpl 日志工厂

| Setting                          | Description                                                  | Valid Values                                                 | Default                                               |
| :------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :---------------------------------------------------- |
| cacheEnabled                     | Globally enables or disables any caches configured in any mapper under this configuration. | true \| false                                                | true                                                  |
| lazyLoadingEnabled               | Globally enables or disables lazy loading. When enabled, all relations will be lazily loaded. This value can be superseded for a specific relation by using the `fetchType` attribute on it. | true \| false                                                | false                                                 |
| aggressiveLazyLoading            | When enabled, any method call will load all the lazy properties of the object. Otherwise, each property is loaded on demand (see also `lazyLoadTriggerMethods`). | true \| false                                                | false (true in ≤3.4.1)                                |
| multipleResultSetsEnabled        | Allows or disallows multiple ResultSets to be returned from a single statement (compatible driver required). | true \| false                                                | true                                                  |
| useColumnLabel                   | Uses the column label instead of the column name. Different drivers behave differently in this respect. Refer to the driver documentation, or test out both modes to determine how your driver behaves. | true \| false                                                | true                                                  |
| useGeneratedKeys                 | Allows JDBC support for generated keys. A compatible driver is required. This setting forces generated keys to be used if set to true, as some drivers deny compatibility but still work (e.g. Derby). | true \| false                                                | False                                                 |
| autoMappingBehavior              | Specifies if and how MyBatis should automatically map columns to fields/properties. NONE disables auto-mapping. PARTIAL will only auto-map results with no nested result mappings defined inside. FULL will auto-map result mappings of any complexity (containing nested or otherwise). | NONE, PARTIAL, FULL                                          | PARTIAL                                               |
| autoMappingUnknownColumnBehavior | Specify the behavior when detects an unknown column (or unknown property type) of automatic mapping target.`NONE`: Do nothing`WARNING`: Output warning log (The log level of `'org.apache.ibatis.session.AutoMappingUnknownColumnBehavior'` must be set to `WARN`)`FAILING`: Fail mapping (Throw `SqlSessionException`) | NONE, WARNING, FAILING                                       | NONE                                                  |
| defaultExecutorType              | Configures the default executor. SIMPLE executor does nothing special. REUSE executor reuses prepared statements. BATCH executor reuses statements and batches updates. | SIMPLE REUSE BATCH                                           | SIMPLE                                                |
| defaultStatementTimeout          | Sets the number of seconds the driver will wait for a response from the database. | Any positive integer                                         | Not Set (null)                                        |
| defaultFetchSize                 | Sets the driver a hint as to control fetching size for return results. This parameter value can be override by a query setting. | Any positive integer                                         | Not Set (null)                                        |
| defaultResultSetType             | Specifies a scroll strategy when omit it per statement settings. (Since: 3.5.2) | FORWARD_ONLY \| SCROLL_SENSITIVE \| SCROLL_INSENSITIVE \| DEFAULT(same behavior with 'Not Set') | Not Set (null)                                        |
| safeRowBoundsEnabled             | Allows using RowBounds on nested statements. If allow, set the false. | true \| false                                                | False                                                 |
| safeResultHandlerEnabled         | Allows using ResultHandler on nested statements. If allow, set the false. | true \| false                                                | True                                                  |
| mapUnderscoreToCamelCase         | Enables automatic mapping from classic database column names A_COLUMN to camel case classic Java property names aColumn. | true \| false                                                | False                                                 |
| localCacheScope                  | MyBatis uses local cache to prevent circular references and speed up repeated nested queries. By default (SESSION) all queries executed during a session are cached. If localCacheScope=STATEMENT local session will be used just for statement execution, no data will be shared between two different calls to the same SqlSession. | SESSION \| STATEMENT                                         | SESSION                                               |
| jdbcTypeForNull                  | Specifies the JDBC type for null values when no specific JDBC type was provided for the parameter. Some drivers require specifying the column JDBC type but others work with generic values like NULL, VARCHAR or OTHER. | JdbcType enumeration. Most common are: NULL, VARCHAR and OTHER | OTHER                                                 |
| lazyLoadTriggerMethods           | Specifies which Object's methods trigger a lazy load         | A method name list separated by commas                       | equals,clone,hashCode,toString                        |
| defaultScriptingLanguage         | Specifies the language used by default for dynamic SQL generation. | A type alias or fully qualified class name.                  | org.apache.ibatis.scripting.xmltags.XMLLanguageDriver |
| defaultEnumTypeHandler           | Specifies the `TypeHandler` used by default for Enum. (Since: 3.4.5) | A type alias or fully qualified class name.                  | org.apache.ibatis.type.EnumTypeHandler                |
| callSettersOnNulls               | Specifies if setters or map's put method will be called when a retrieved value is null. It is useful when you rely on Map.keySet() or null value initialization. Note primitives such as (int,boolean,etc.) will not be set to null. | true \| false                                                | false                                                 |
| returnInstanceForEmptyRow        | MyBatis, by default, returns `null` when all the columns of a returned row are NULL. When this setting is enabled, MyBatis returns an empty instance instead. Note that it is also applied to nested results (i.e. collectioin and association). Since: 3.4.2 | true \| false                                                | false                                                 |
| logPrefix                        | Specifies the prefix string that MyBatis will add to the logger names. | Any String                                                   | Not set                                               |
| logImpl                          | Specifies which logging implementation MyBatis should use. If this setting is not present logging implementation will be autodiscovered. | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | Not set                                               |
| proxyFactory                     | Specifies the proxy tool that MyBatis will use for creating lazy loading capable objects. | CGLIB \| JAVASSIST                                           | JAVASSIST (MyBatis 3.3 or above)                      |
| vfsImpl                          | Specifies VFS implementations                                | Fully qualified class names of custom VFS implementation separated by commas. | Not set                                               |
| useActualParamName               | Allow referencing statement parameters by their actual names declared in the method signature. To use this feature, your project must be compiled in Java 8 with `-parameters` option. (Since: 3.4.1) | true \| false                                                | true                                                  |
| configurationFactory             | Specifies the class that provides an instance of `Configuration`. The returned Configuration instance is used to load lazy properties of deserialized objects. This class must have a method with a signature `static Configuration getConfiguration()`. (Since: 3.2.3) | A type alias or fully qualified class name.                  | Not set                                               |
| shrinkWhitespacesInSql           | Removes extra whitespace characters from the SQL. Note that this also affects literal strings in SQL. (Since 3.5.5) | true \| false                                                | false                                                 |
| defaultSqlProviderType           | Specifies an sql provider class that holds provider method (Since 3.5.6). This class apply to the `type`(or `value`) attribute on sql provider annotation(e.g. `@SelectProvider`), when these attribute was omitted. | A type alias or fully qualified class name                   | Not set                                               |


### 3.typeAliases(别名)

#### 1.设置实体类

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

#### 2.设置包名

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

#### 3.注解

```java
@Alias("author")
public class Author {
    ...
}
```

#### 4.Mybatis 定义的java类型别名

| Alias      | Mapped Type |
| :--------- | :---------- |
| _byte      | byte        |
| _long      | long        |
| _short     | short       |
| _int       | int         |
| _integer   | int         |
| _double    | double      |
| _float     | float       |
| _boolean   | boolean     |
| string     | String      |
| byte       | Byte        |
| long       | Long        |
| short      | Short       |
| int        | Integer     |
| integer    | Integer     |
| double     | Double      |
| float      | Float       |
| boolean    | Boolean     |
| date       | Date        |
| decimal    | BigDecimal  |
| bigdecimal | BigDecimal  |
| object     | Object      |
| map        | Map         |
| hashmap    | HashMap     |
| list       | List        |
| arraylist  | ArrayList   |
| collection | Collection  |
| iterator   | Iterator    |

### 4.environments(环境配置)

environments 元素定义了如何配置环境。

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

注意一些关键点:

- 默认使用的环境 ID（比如：default="development"）
  - JDBC – 这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。
  - MANAGED – 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接。然而一些容器并不希望连接被关闭，因此需要将 closeConnection 属性设置为 false 来阻止默认的关闭行为。例如:
- 每个 environment 元素定义的环境 ID（比如：id="development"）。
- 事务管理器的配置（比如：type="JDBC"）。
- 数据源的配置（比如：type="POOLED"）
  - **UNPOOLED**– 这个数据源的实现会每次请求时打开和关闭连接
  - **POOLED**– 这种数据源的实现利用“池”的概念将 JDBC 连接对象 不用每次打开关闭连接

### 5.mappers(映射器)

1. 使用相对于类路径的资源引用
```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>

```

2. 使用映射器接口
```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

3. 使用包映射器
```java
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

## 7.多对一映射和一对多映射

### 1. 结果集映射(写好sql然后用resultMap映射就行)

```xml
<select id="getTeacherById" resultMap="studentTeacher1">
        select s.id sid, s.name sname, t.name tname, t.id tid
        from mybatis.student s,
             mybatis.teacher t
        where s.tid = t.id
          and t.id = #{tid};
    </select>

    <resultMap id="studentTeacher1" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>
```

### 2.嵌套查询(连表查询)

```xml
<!--查询嵌套(连表查询)-->
    <select id="getTeacherById3" resultMap="studentTeacher3">
        select *
        from mybatis.teacher
        where id = #{tid};
    </select>
    <resultMap id="studentTeacher3" type="Teacher">
        <collection property="students" javaType="ArrayList" ofType="Student" select="getTeacherById" column="id"/>
        <!-- selcet对应的是下面的查询-->
    </resultMap>
    <select id="getTeacherById" resultType="Student">
        select *
        from mybatis.student
        where id = #{tid};
    </select>
```




## 7.使用注解开发

| **注解** | **说明**                               |
| -------- | -------------------------------------- |
| @Insert  | 实现新增                               |
| @Delete  | 实现删除                               |
| @Update  | 实现更新                               |
| @Select  | 实现查询                               |
| @Result  | 实现结果集封装                         |
| @Results | 可以与@Result 一起使用，封装多个结果集   |

本质：反射机制实现

底层：动态代理

## 8.动态Sql

### 1. 动态Sql就是根据不同的情况拼接sql语句

### 2.动态sql标签

- if(选择语句)
- choose (when, otherwise) = (switch语句)
- trim (where, set) ( *where* 元素只会在子元素为true情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除 set自动去除分号)
- foreach (对集合进行遍历)

#### IF

```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
</select>
```

#### choose 

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

#### where and set

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

#### foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

## 9.缓存

缓存是一般的ORM 框架都会提供的功能，目的就是**提升查询的效率和减少数据库的压力**。跟Hibernate 一样，MyBatis 也有一级缓存和二级缓存，并且预留了集成第三方缓存的接口。

### 1.一级缓存(本地缓存)

一级缓存也叫本地缓存，MyBatis 的一级缓存是在会话（SqlSession）层面进行缓存的。MyBatis 的一级缓存是默认开启的，不需要任何的配置。SqlSession级别

![](https://img2018.cnblogs.com/blog/1383365/201906/1383365-20190628172851422-987384747.png)

### 2. 生命周期(缓存失效)

1. MyBatis在开启一个数据库会话时，会 创建一个新的SqlSession对象，SqlSession对象中会有一个新的Executor对象，Executor对象中持有一个新的PerpetualCache对象；当会话结束时，SqlSession对象及其内部的Executor对象还有PerpetualCache对象也一并释放掉。
2. 如果SqlSession**调用了close()方法**，会释放掉一级缓存PerpetualCache对象，一级缓存将不可用；
3. 如果SqlSession**调用了clearCache()**，会清空PerpetualCache对象中的数据，但是该对象仍可使用；
4. SqlSession中执行了任何一个**update操作**(update()、delete()、insert()) ，都会清空PerpetualCache对象的数据，但是该对象可以继续使用

### 3.执行过程

1. 对于某个查询，根据statementId,params,rowBounds来构建一个key值，根据这个key值去缓存Cache中取出对应的key值存储的缓存结果
2. 判断从Cache中根据特定的key值取的数据数据是否为空，即是否命中；
3. 如果命中，则直接将缓存结果返回；
4. 如果没命中：

1. 1. 去数据库中查询数据，得到查询结果；
   2. 将key和查询到的结果分别作为key,value对存储到Cache中；
   3. 将查询结果返回；

## 2.二级缓存

级缓存是用来解决一级缓存不能跨会话共享的问题的，范围是namespace 级别的，可以被多个SqlSession 共享（只要是同一个接口里面的相同方法，都可以共享），生命周期和应用同步

![](https://img2018.cnblogs.com/blog/1383365/201906/1383365-20190628180149776-546074458.png)

### 1.工作流程

如果启用了二级缓存，MyBatis 在创建Executor 对象的时候会对Executor 进行装饰。CachingExecutor 对于查询请求，会判断二级缓存是否有缓存结果，如果有就直接返回，如果没有委派交给真正的查询器Executor 实现类，比如SimpleExecutor 来执行查询，再走到一级缓存的流程。最后会把结果缓存起来，并且返回给用户。

### 2.使用二级缓存

第一步：配置 mybatis.configuration.cache-enabled=true，只要没有显式地设置cacheEnabled=false，都会用CachingExecutor 装饰基本的执行器。

第二步：在Mapper.xml 中配置<cache/>标签：

```xml
<cache type="org.apache.ibatis.cache.impl.PerpetualCache"
    size="1024"
eviction="LRU"
flushInterval="120000"
readOnly="false"/>
```

### 3.第三方缓存框架 echahce

1. 导入echahce
```xml
<dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.1.0</version>
        </dependency>

```
2. 编写ehcache.xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
 <!-- 磁盘保存路径 -->
 <diskStore path="D:\atguigu\ehcache" />
 
 <defaultCache 
   maxElementsInMemory="1000" 
   maxElementsOnDisk="10000000"
   eternal="false" 
   overflowToDisk="true" 
   timeToIdleSeconds="120"
   timeToLiveSeconds="120" 
   diskExpiryThreadIntervalSeconds="120"
   memoryStoreEvictionPolicy="LRU">
 </defaultCache>
</ehcache>
 
<!-- 
属性说明：
l diskStore：指定数据在磁盘中的存储位置。
l defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略
 
以下属性是必须的：
l maxElementsInMemory - 在内存中缓存的element的最大数目 
l maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
l eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
l overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上
 
以下属性是可选的：
l timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大
l timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大
 diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区.
l diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。
l diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
l memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）
 -->

```

3. 配置cache标签
```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
```

## 10.Mybatis执行流程

![](https://img2020.cnblogs.com/blog/2084540/202009/2084540-20200916132427508-886466614.png)

[Mybatis缓存文章]: https://www.cnblogs.com/wuzhenzhao/p/11103043.html
[狂神说Mybatis]: https://img2020.cnblogs.com/blog/2084540/202009/2084540-20200916132427508-886466614.png
