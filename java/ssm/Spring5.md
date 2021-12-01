## 1.Spring

### 1.1简介

#### Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。

### 1.2特点

- 轻量
- 控制反转(IOC)
- 面向切面(AOP)
- 容器
- 框架

### 1.3架构组成

![](https://bkimg.cdn.bcebos.com/pic/2e2eb9389b504fc245d07093e5dde71191ef6d9d?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U5Mg==,g_7,xp_5,yp_5/format,f_auto)

## 2.IOC

### 2.1简介

#### **控制反转**（Inversion of Control，缩写为**IoC**），是[面向对象编程](https://baike.baidu.com/item/面向对象编程/254878)中的一种设计原则，可以用来减低计算机[代码](https://baike.baidu.com/item/代码/86048)之间的[耦合度](https://baike.baidu.com/item/耦合度/2603938)。其中最常见的方式叫做**[依赖注入](https://baike.baidu.com/item/依赖注入/5177233)**（Dependency Injection，简称**DI**） Spring使用set方法实现IOC 所谓的IOC可以用一句话概括 对象由Spring创建 管理 装配

#### 控制反转:  本来代码控制权在程序猿手里，使用Spring IOC可以把代码的控制权交给用户实现解耦， 从而不用再去new对象程序猿可以更好的专注于业务的实现
#### 依赖注入:  就是使用Set方法进行注入

### 3.HelloSpring

### 3.1导入Spring包

### 3.2.新建一个HelloWorld类

```java
package pojo;

public class HelloWorld {
    public void say() {
        System.out.println("Hello world!");
    }
}
```

### 3.3新建ApplicationContext.xml

### 3.4在ApplicationContext.xml注册bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="hello" class="pojo.Hello"/>
</beans>
```

### 3.5 新建测试类
```JAVA
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import pojo.Hello;
import pojo.NoneCons;
import pojo.Person;

public class MyTest {
    public static void main(String[] args) {
    	//获取applicationCOntext上下文
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        //找到对应的Bean 使用反射可以避免强转
        Hello contextBean = context.getBean("hello", Hello.class);
        //执行方法
        contextBean.say();
    }
}
```

## 4.控制反转IOC创建对象的方法

### 4.1Spring默认使用无参构造创建对象

### 4.2使用有参构造创建对象

```xml
<!--constructor-arg 会向有参构造函数赋值-->

<bean id="person02" class="pojo.NoneCons">
    	<!--注入值-->
        <constructor-arg name="age" value="31"/>
        <constructor-arg name="name" value="John"/>
    </bean>
```

### 4.3使用@ConstructorProperties注解

```java
@ConstructorProperties({"name1","age1"})
//参数名对应的是Bean的 constructor-arg name=""的名称
    public NoneCons(String name, String age) {
        this.name = name;
        this.age = age;
    }
```

```xml
<bean id="person02" class="pojo.NoneCons">
        <constructor-arg name="age1" value="31"/>
        <constructor-arg name="name1" value="John"/>
    </bean>
```

## 5.application.xml配置文件
-   5.1bean
	- 注册bean可以让application找到 	
-  alias
	-  别名
- description
	- 描述信息
- import 
	-  导入其他的bean文件 团队开发使用

## 6.依赖注入（Dependency Injection)

- 依赖 bean对象依赖于容器
- 注入 bean所以的属性 依赖于容器来注入

###  6.1. 使用构造器来注入

- Spring默认使用空构造器来注入
- 可以使用有参构造来注入
```xml
<!--constructor-arg 会向有参构造函数赋值-->

<bean id="person02" class="pojo.NoneCons">
    	<!--注入值-->
        <constructor-arg name="age" value="31"/>
        <constructor-arg name="name" value="John"/>
    </bean>
```

###   6.2 使用set方法注入

#### 1.编写Student类和Address类 用来测试各种值的注入
##### student类

```java
package com.pojo;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Properties;

public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbits;
    private Map<String,String> IdCard;
    private String wife;
    private Properties info;

    public Student() {
    }

    public Student(String name, Address address, String[] books, List<String> hobbits, Map<String, String> idCard, String wife, Properties properties) {
        this.name = name;
        this.address = address;
        this.books = books;
        this.hobbits = hobbits;
        IdCard = idCard;
        this.wife = wife;
        this.info = properties;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbits() {
        return hobbits;
    }

    public void setHobbits(List<String> hobbits) {
        this.hobbits = hobbits;
    }

    public Map<String, String> getIdCard() {
        return IdCard;
    }

    public void setIdCard(Map<String, String> idCard) {
        IdCard = idCard;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", address=" + address.toString() +
                ", books=" + Arrays.toString(books) +
                ", hobbits=" + hobbits +
                ", IdCard=" + IdCard +
                ", wife='" + wife + '\'' +
                ", info=" + info +
                '}';
    }
}
```

##### Address类
```java
package com.pojo;

public class Address {
    private String address;

    @Override
    public String toString() {
        return "com.pojo.Address{" +
                "address='" + address + '\'' +
                '}';
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Address(String address) {
        this.address = address;
    }
}
```
##### 2. 编写applicationContext.xml 注入值
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="student" class="com.pojo.Student">
            <!--            普通类型注入-->
            <property name="name" value="John"/>
<!--            复杂类型注入-->
            <property name="address" value="Linyi City"/>
<!--            数组注入-->
            <property name="books">
                <array>
                    <value>三体1</value>
                    <value>三体2</value>
                    <value>三体3</value>
                </array>
            </property>
<!--            list注入-->
            <property name="hobbits">
                <list>
                    <value>lol</value>
                    <value>com</value>
                    <value>dnf</value>
                </list>
            </property>
<!--            map类型注入-->
            <property name="idCard">
                <map>
                    <entry key="身份证" value="111111"/>
                </map>
            </property>
<!--            null值-->
            <property name="wife">
                <null/>
            </property>
<!--            特殊类型注入-->
            <property name="info">
                <props>
                    <prop key="身高">1.1cm</prop>
                    <prop key="体重">1kg</prop>
                </props>
            </property>
        </bean>
    <!-- more bean definitions go here -->

</beans>
```
##### 测试
```JAVA
import com.pojo.Student;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    @Test
    public void Test01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = context.getBean(Student.class);
        System.out.println(student.toString());
    }
}
```
### 6.3 命名空间注入

##### 1.编写实体类

```java
package pojo;

public class human {
    private int age;
    private String name;

    public human() {
    }

    public human(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "human{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
```

##### 2. 编写applicationcontext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    p = property-->
<!--    <bean id="human" class="pojo.human" p:age="11" p:name="Human"/>-->
<!--    c= constructor-args -->
    <bean id="human2" class="pojo.human" c:age="14" c:name="UnHuman"/>
    <!-- more bean definitions go here -->
</beans>
```
-  注意！ 必须导入命名空间的约束才可以用

##### 3.编写测试类

```JAVA
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import pojo.human;

public class Test {
    @org.junit.Test
    public void Test01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationcontext.xml");
        human bean = context.getBean(human.class);
        System.out.println(bean);
    }
}
```

## 7. Bean作用域

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

#### 7.1 [singleton](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes-singleton)单例模式(Spring默认机制)

```xml
<bean id="human2" class="pojo.human" c:age="14" c:name="UnHuman" scope="singleton"/>
```

#### 7.2 protype原型模式(每次容器get的时候，都会产生一个新对象)

```xml
<bean id="human2" class="pojo.human" c:age="14" c:name="UnHuman" scope="prototype"/>
```

#### 7.3.request session application websocket是SpringMVC的作用域

## 8.自动装配

- 就是将一个Bean注入到其它的Bean的Property中，默认情况下，容器不会自动装配，需要我们手动设定 使用autowired或者@Autowired来实现。Spring 可以通过向Bean Factory中注入的方式来搞定bean之间的依赖关系，达到自动装配的目的。

### 8.1 使用autowired选项

```xml
<bean id="dog" class="com.pojo.Dog" autowire="byName"/>
<bean id="dog" class="com.pojo.Dog" autowire="bytype"/>
```
- bynameSpring容器会在上下文里自动匹配get方法名字 实现自动装配
- bytype Spring容器会在上下文里自动匹配属性类型 来实现自动装配

### 8.2使用@Autowire和 @Qualifier实现自动装配
1. 注解可以使用在set方法和属性上面
2.  @Qualifier() 指定那个bean自动装配
#### 代码
##### 编写测试类 dog cat god
```java
package com.pojo;

public class Cat {
    public void shout(){
        System.out.println("I can't say miao miao");
    }
}
```


```java
package com.pojo;

public class Dog {
    public void shout(){
        System.out.println("I can't say wa wa");
    }
}

```

```java
package com.pojo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;


public class God {
    private String name;
    @Autowired
    @Qualifier("cat1")
    private Cat cat;
    @Autowired
    private Dog dog;

    public God() {
    }

    public God(String name, Cat cat, Dog dog) {
        this.name = name;
        this.cat = cat;
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    @Override
    public String toString() {
        return "God{" +
                "name='" + name + '\'' +
                ", cat=" + cat.toString() +
                ", dog=" + dog.toString() +
                '}';
    }
}
```

##### 编写applicationcontext

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-3.0.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
    <context:annotation-config/>
    <bean id="cat" class="com.pojo.Cat"/>
    <bean id="cat1" class="com.pojo.Cat"/>
    <bean id="dog" class="com.pojo.Dog" />
    <bean id="god" class="com.pojo.God"/>
</beans>
```
##### 编写test类
```java
import com.pojo.Cat;
import com.pojo.God;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    @Test
    public void Test01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        God bean = context.getBean(God.class);
        bean.getCat().shout();
        bean.getDog().shout();
    }
}
```

### 8.3  **@Resource**

  @Resource如有指定的name属性，先按该属性进行byName方式查找装配；其次再进行默认的byName方式进行装配；如果以上都不成功，则按byType的方式自动装配。都不成功，则报异常。

```java
  package com.hdu.autowire;
  
  import javax.annotation.Resource;
  
  public class User {
      @Resource
     private Cat cat;
      @Resource(name="dogXXX")
      private Dog dog;
    private String str;
     
     public Cat getCat() {
         return cat;
     }
     
     public Dog getDog() {
         return dog;
     }
     
     public String getStr() {
         return str;
     }
 }
```

配置文件1：

```xml
<bean id="catXXX" class="com.hdu.autowire.Cat"></bean>
<bean id="cat" class="com.hdu.autowire.Cat"></bean>
<bean id="dogXXX" class="com.hdu.autowire.Dog"></bean>
    
<bean id="user" class="com.hdu.autowire.User"></bean>
```

 先进行byName查找，成功。

 

配置文件2：

```xml
<context:annotation-config/> 
    
<bean id="catXXX" class="com.hdu.autowire.Cat"></bean>
<bean id="dogXXX" class="com.hdu.autowire.Dog"></bean>
    
<bean id="user" class="com.hdu.autowire.User"></bean>
```

  先进行byName查找，失败；再进行byType查找，成功。

 

配置文件3：

```xml
<bean id="catXXX" class="com.hdu.autowire.Cat"></bean>
<bean id="catYYY" class="com.hdu.autowire.Cat"></bean>
<bean id="dogXXX" class="com.hdu.autowire.Dog"></bean>
    
<bean id="user" class="com.hdu.autowire.User"></bean>
```

先进行byName查找，失败；再进行byType查找，有两个匹配，失败，报错。

## 8.4.小结

  @Autowired与@Resource异同：

  1° @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。

  2° @Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用。

  3° @Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。 当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。

## 9.注解

### 9.1常用注解

| 注解               | 解释                                                         |
| ------------------ | ------------------------------------------------------------ |
| @Configuration     | **标识一个该类是Spring MVC controller处理器，用来创建处理http请求的对象.** |
| @Configuration     | **标记一个该类是Spring的上下文 就等于applicationcontext.xml** |
| **@Controller**    | **标识一个该类是Spring MVC controller处理器，用来创建处理http请求的对象.** |
| **@Component**     | **把普通pojo实例化到spring容器中，相当于配置文件中的 <bean id="" class=""/>**<br/>**它泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。**<br/>**此外，被@controller 、@service、@repository 、@component 注解的类，都会把这些类纳入进spring容器中进行管理** |
| **@Service**       | **用于标注业务层组件，以注解的方式把这个类注入到spring配置中** |
| @Repository        | 用于标注dao(数据访问层)组件，以注解的方式把这个类注入到spring配置中 |
| ***\*@Scope\****   | **@Scope注解是springIoc容器中的一个作用域，在 Spring IoC 容器中具有以下几种作用域：基本作用域\**singleton（单例）\**、\**prototype(多例)\**，Web 作用域（reqeust、session、globalsession），自定义作用域。（@Scope注解默认的singleton单例模式）** |
| **@Bean**          | **产生一个bean的方法，并且交给Spring容器管理，\**相当于配置文件中的 <bean id="" class=""/>\**** |
| **@Aspect**        | **作用是标记一个切面类（spring不会将切面注册为Bean也不会增强，但是需要扫描）** |
| **@Pointcut**      | **义切点，切点表达式(execution(权限访问符 返回值类型 方法所属的类名包路径.方法名(形参类型) 异常类型))** |
| **@Aspect**        | **作用是标记一个切面类（spring不会将切面注册为Bean也不会增强，但是需要扫描）** |
| **@Transactional** | **事务管理一般有编程式和声明式两种，编程式是直接在代码中进行编写事物处理过程，而声名式则是通过注解方式或者是在xml文件中进行配置，相对编程式很方便。** |
### 9.2注意事项 

1.必须导入SpringAopjar包

2.如果不是纯注解开发必须在Applicationcontext.xml声明aop和context约束和添加<context:component-scan base-package=""/> <context:annotation-config/>这两句xml
```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-3.0.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">
       <!--context:component-scan = 扫描包以便spring可以找到/
       <context:annotation-config/> = 启动注解
       >>>-->
    <context:component-scan base-package="com.controller"/>
    <context:component-scan base-package="com.pojo"/>
    <context:component-scan base-package="com.dao"/>
    <context:component-scan base-package="com.Service"/>
<!--    <context:component-scan base-package=""/>-->
    <context:annotation-config/>

</beans>

```

## 9.代理模式

##### 好处

- 实现解耦
- 无需修改客户代码的前提下于已有类的对象上增加额外行为
- 增加权限

### 9.1. 静态代理

使用一个代理对象将对象包装起来，然后用该代理对象来取代该对象，任何对原始对象的调用都要通过代理，代理对象决定是否以及何时调用原始对象的方法

####  例子

![image-20211201151226254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211201151226254.png)

有个需求 我想租房我必须找中介租放让中介代理我去做这个事情 我是接触不到房东的 房东和中介都有把房子租出去的需求 

#### 代码

##### 租房接口

```java
package com.staicproxy;

public interface Tenement {
    void tenement();
}
```



##### 房东类

```java
package com.staicproxy;

public class Landlord implements Tenement {
    @Override
    public void tenement() {
        System.out.println("LandLord wanna rent a house");
    }
}

```

##### 中介类

```java
package com.staicproxy;

public class Proxy implements Tenement {
    private Landlord landlord;

    public Proxy() {
    }

    public Proxy(Landlord landlord) {
        this.landlord = landlord;
    }

    public Landlord getLandlord() {
        return landlord;
    }

    public void setLandlord(Landlord landlord) {
        this.landlord = landlord;
    }

    @Override
    public void tenement() {
        landlord.tenement();
        this.seeHouse();
        this.money();
    }
    public void seeHouse(){
        System.out.println("seeHouse");
    }
    public void money(){
        System.out.println("Well, who will collect them, then?");
    }
}
```

##### 测试类(我 类)

```java
package com.staicproxy;


import org.junit.jupiter.api.Test;

public class Me {
    @Test
    public void Test(){
        Landlord landlord = new Landlord();
        Tenement tenement = new Landlord();
        tenement.tenement();
        Proxy proxy = new Proxy(landlord);
        proxy.tenement();
    }
}
```

##### 实现思想

房东(Landlord)和中介(Proxy)都去实现租房(Tenement)这个接口 然后再中介类(Proxy)新建房东的属性设置set方法 这样就可以实现让中介(Proxy)代理房东去做租房这件事情了 

### 9.2动态代理

动态代理顾名思义 代理类在程序运行时创建的代理方式被成为动态代理 而不是写死再代码里面

使用Proxy和InvocationHandler实现 

proxy获取代理对象 

InvocationHandler处理代理对象并返回

#### 租房类

```java
package com.proxy;

public interface Tenement {
    void tenement();
}
```

#### 	房东类

```java
package com.proxy;

public class Landlord implements Tenement {
    @Override
    public void tenement() {
        System.out.println("LandLord wanna rent a house");
    }
}
```

#### 代理类

```java
package com.proxy;

import com.staicproxy.Landlord;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyInvocationHandler implements InvocationHandler {
    //被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
    }

    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        sayHello(method.getName());
        return method.invoke(target, args);
    }

    public void sayHello(String massage){
        System.out.println("say hello" + massage);
    }
}
```

#### 客户机(测试类)

```java
package com.proxy;


import com.staicproxy02.UserService;
import com.staicproxy02.UserServiceImpi;

public class Client {
    public static void main(String[] args) {
        Landlord landlord = new Landlord();
        ProxyInvocationHandler proxyInvocationHandler = new ProxyInvocationHandler();
        proxyInvocationHandler.setTarget(landlord);
        Tenement tenement = (Tenement) proxyInvocationHandler.getProxy();
        tenement.tenement();
    }
}
```

## 10.aop(Aspect Oriented Programming)

### 10.1什么是aop

#### AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。可以在不改变代码的情况下增加功能 底层实现原理就是动态代理！

### 10.2Spring aop

#### 提供声明式事务；允许用户自定义切面

####  aop名词

- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个**类**   。

- 通知（Advice）：切面必须要完成的工作。即，**它是类中的一个方法**。

- 目标（Target）：被通知对象。

- 代理（Proxy）：向目标对象应用通知之后创建的对象。

- 切入点（PointCut）：切面通知 执行的 “地点”的定义 要切入的对象。

- 连接点（JointPoint）：与切入点匹配的执行点。

  ![](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JAeTYOaaH6rZ6WmLLgwQLHVOZ1JpRb7ViaprZCRXsUbH0bZpibiaTjqib68LQHOWZicSvuU8Y1dquUVGw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 五种Spring切入通知

![](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JAeTYOaaH6rZ6WmLLgwQLHbAWH8haUQeJ0LVBxxX0icC5TZlBkEBGibibey7jFrCbibPzQcRhkNFcGAA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 10.3.使用Spring-aop

【重点】使用AOP织入，需要导入一个依赖包！

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

#### **第一种方式**

**通过 Spring API 实现**

##### 首先编写我们的业务接口和实现类

```java
public interface UserService {

   public void add();

   public void delete();

   public void update();

   public void search();

}
public class UserServiceImpl implements UserService{

   @Override
   public void add() {
       System.out.println("增加用户");
  }

   @Override
   public void delete() {
       System.out.println("删除用户");
  }

   @Override
   public void update() {
       System.out.println("更新用户");
  }

   @Override
   public void search() {
       System.out.println("查询用户");
  }
}
```

#### 增强类

```java
public class Log implements MethodBeforeAdvice {

   //method : 要执行的目标对象的方法
   //objects : 被调用的方法的参数
   //Object : 目标对象
   @Override
   public void before(Method method, Object[] objects, Object o) throws Throwable {
       System.out.println( o.getClass().getName() + "的" + method.getName() + "方法被执行了");
  }
}
public class AfterLog implements AfterReturningAdvice {
   //returnValue 返回值
   //method被调用的方法
   //args 被调用的方法的对象的参数
   //target 被调用的目标对象
   @Override
   public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
       System.out.println("执行了" + target.getClass().getName()
       +"的"+method.getName()+"方法,"
       +"返回值："+returnValue);
  }
}
```

##### 最后去spring的文件中注册 , 并实现aop切入实现 , 注意导入约束 .

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

   <!--注册bean-->
   <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
   <bean id="log" class="com.kuang.log.Log"/>
   <bean id="afterLog" class="com.kuang.log.AfterLog"/>

   <!--aop的配置-->
   <aop:config>
       <!--切入点 expression:表达式匹配要执行的方法-->
       <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
       <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
       <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
       <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
   </aop:config>
</beans>
```

#### **第二种方式**

**自定义类来实现Aop**

##### 代码不变 添加个userServiceImpl

```java
public class DiyPointcut {
   public void before(){
       System.out.println("---------方法执行前---------");
  }
   public void after(){
       System.out.println("---------方法执行后---------");
  }  
}
```

##### 注册再applcationContext 注册bean

```xml
<!--第二种方式自定义实现-->
<!--注册bean-->
<bean id="diy" class="com.kuang.config.DiyPointcut"/>

<!--aop的配置-->
<aop:config>
   <!--第二种方式：使用AOP的标签实现-->
   <aop:aspect ref="diy">
       <aop:pointcut id="diyPonitcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
       <aop:before pointcut-ref="diyPonitcut" method="before"/>
       <aop:after pointcut-ref="diyPonitcut" method="after"/>
   </aop:aspect>
</aop:config>
```

#### **第三种方式**

**使用注解实现**

##### 第一步：编写一个注解实现的增强类

```java
package com.kuang.config;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
// 是一个aop类
public class AnnotationPointcut {
   @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
   public void before(){
       System.out.println("---------方法执行前---------");
  }

   @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
   public void after(){
       System.out.println("---------方法执行后---------");
  }

   @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
   public void around(ProceedingJoinPoint jp) throws Throwable {
       System.out.println("环绕前");
       System.out.println("签名:"+jp.getSignature());
       //执行目标方法proceed
       Object proceed = jp.proceed();
       System.out.println("环绕后");
       System.out.println(proceed);
  }
}
```

第二步：在Spring配置文件中，注册bean，并增加支持注解的配置

```xml
<!--第三种方式:注解实现-->
<bean id="annotationPointcut" class="com.kuang.config.AnnotationPointcut"/>
<aop:aspectj-autoproxy/>
```

aop:aspectj-autoproxy：说明
通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了<aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-aut



## 11.事务管理

pring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

### 11.1**编程式事务管理**

- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

### 11.2**声明式事务管理**

- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**使用Spring管理事务，注意头文件的约束导入 : tx**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

- 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
- 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

**JDBC事务**

```xml
<!--配置事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
</bean>
```

**配置好事务管理器后我们需要去配置事务的通知**

```xml
<!--配置事务通知-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
   <tx:attributes>
       <!--配置哪些方法使用什么样的事务,配置事务的传播特性-->
       <tx:method name="add" propagation="REQUIRED"/>
       <tx:method name="delete" propagation="REQUIRED"/>
       <tx:method name="update" propagation="REQUIRED"/>
       <tx:method name="search*" propagation="REQUIRED"/>
       <tx:method name="get" read-only="true"/>
       <tx:method name="*" propagation="REQUIRED"/>
   </tx:attributes>
</tx:advice>
```

**spring事务传播特性：**

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！

**配置AOP**

导入aop的头文件！

```xml
<!--使用aop 添加事务-->
<aop:config>
   <aop:pointcut id="txPointcut" expression="execution(* com.kuang.dao.*.*(..))"/>
   <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>
```

**进行测试**

删掉刚才插入的数据，再次测试！

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserMapper mapper = (UserMapper) context.getBean("userDao");
   List<User> user = mapper.selectUser();
   System.out.println(user);
}
```

> 思考问题？

为什么需要配置事务？

- 如果不配置，就需要我们手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！






[Spring自动装配详解]: https://blog.csdn.net/zcywell/article/details/88397158
[Spring注解常用注解解析]: https://www.cnblogs.com/nhdlb/p/12451728.html

[狂神说Spring]: https://www.cnblogs.com/renxuw/p/12994080.html
[spring官网]: https://spring.io/
[spring整合mybatis]: https://www.jianshu.com/p/412051d41d73

