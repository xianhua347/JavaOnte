# 1.javaWeb

### 1.1 基本概念

#### javaweb是B/S模式，用来开发动态网页的技术栈

## 2.web服务器

#### 提供web服务的软件或者计算机，叫做web服务器，常见的web服务器 IIS apache nginx

## 3.Tomcat

### 3.1简介

#### tomcat是apache开源组织和sun公司开发的web服务器,是中小型web服务器

### 3.2 tomcat中文乱码问题 

#### 修改Tomcat的conf的server.xml文件加上 URIEncoding="UTF-8"

### 3.3Idea中tomcat配置

##### 1.打开configure

##### 2.点击添加服务

##### 3.选择tomcat local

##### 4.选择端口（默认8080）

##### 5.点击deployment选择项目war包

##### 6.设置上下文可以是空

![image-20211026215801353](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211026215801353.png)

![image-20211026215927878](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211026215927878.png)

### 3.4 tomcat所有应用都在webapps目录下

## 4.HTTP协议

### 4.1 简介

#### Http协议是tcp/ip协议簇应用层用于web传输的协议 

#### Http是c/s模式 http是无连接协议	

### 4.2工作流程
#### 客户端发起请求
#### 服务器接收请求
#### 服务器处理客户端发送的请求并给予回应
#### 断开连接

### 4.3 http报文结构
#### （1）请求报文
##### 请求行 －通用信息头 -  请求头 － 实体头 － 报文主体

#### （2）响应报文
##### 状态行 － 通用信息头 － 响应头 － 实体头 － 报文主体

### 4.4 常见的状态消息

#### 200系列 成功

#### 300系列 重定向

#### 400系列客户端 错误

#### 500系列服务器 错误

## 5.Maven 

### 5.1简介

#### Maven约定大于配置

#### Maven是java包管理工具 负责加载第三方jar包和打包程序

#### maven通过pom.xlm来管理项目和加载依赖

### 5.2 修改maven镜像仓库
#### 找到maven-config文件夹下的 settings.xml
#### 使用任意编辑器打开文件
#### 找到mirrors 标签
#### 添加如下代码
```xml
<mirror>
      <id>nexus-aliyun</id>
      <name>Nexus aliyun</name>
      <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    </mirror>
  </mirrors>
```

### 5.3 在idea使用maven

#### 新建项目 找到maven选项
#### 选择位置
#### 选择自己安装maven的路径 并且修改settings.xml位置和 repository的位置（是自己的本地的repository和settings）

## 6.Servlet

### 6.1简介

###### Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

### 6.2 Servlet声明周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 初始化后调用 **init ()** 方法。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 销毁前调用 **destroy()** 方法。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package javax.servlet;
import java.io.IOException;

public interface Servlet {
    void init(ServletConfig var1) throws ServletException;
    ServletConfig getServletConfig();
    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
    String getServletInfo();
    void destroy();
}

```

### 6.3 servlet实例化

如果想使用servlet必须有servlet包和继承HttpServlet抽象类 然后重写方法doget()和dopost()方法

### 6.4 Request和Response

Request是web端发来的请求的报文 可以获得客户端的内容

Response是给客户端发送响应报文 可以设置给客户端的内容

### (5)ServletContext

#### 5.1简介

##### ServletContext官方叫servlet上下文。**服务器会为每一个工程创建一个对象**，这个对象就是ServletContext对象。这个对象全局唯一，**而且工程内部的所有servlet都共享这个对象**。所以叫全局应用程序共享对象。

##### 5.2作用

1. 是一个域对象

2. 可以读取全局配置参数

3. 可以搜索当前工程目录下面的资源文件

4. 可以获取当前工程名字（了解）

### (6)请求转发和重定向
#### 6.1请求转发
##### 客户端发起请求服务器响应然后把另外一个页面的内容复制一份呈现给用户，不改变url链接
##### 实现方法
```java
//1.使用上下文来转发
ServletContext servletContext = this.getServletContext();
servletContext.getRequestDispatcher("/ServletDemo01").forward(req,resp);
//使用req来转发
req.getRequestDispatcher("/ServletDemo01").forward(req,resp);
```

#### 6.2重定向

##### 客户端发起请求服务器响应然后A页面会跳转到B页面，改变url链接

##### 实现原理

```java
reSponse.setStatus("302");
//设置状态码为302重新定向页面
reSponse.setHeader("location","servlet地址？参数名=参数值&参数名=参数值。。。");
//设置重新定向的地址
```

##### 实现方法

```java
resp.sendRedirect("https://limestart.cn/");
```

## 7.jsp

## (1)简介

#### **JSP**（全称**J**ava**S**erver **P**ages）是由[Sun Microsystems](https://baike.baidu.com/item/Sun Microsystems)公司主导创建的一种[动态网页技术](https://baike.baidu.com/item/动态网页技术/9415956)标准。

### (2).基本使用

#### 1.导入包jsp.jar包

```xml
<dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.2.1</version>
            <scope>provided</scope>
        </dependency>
```

#### 2.编写jsp页面

#### 3.编写servlet并在web.xml里注册

## (3).jsp执行过程

1. 客户端发起请求
2. web服务器接收请求 判断是不是jsp页面 如果是就给jsp引擎
3. jsp引擎会把jsp文件转化成sevlet 前端网页成为一个个的println语句
4. 服务器响应请求

## (4).jsp生命周期
1. 编译阶段：

servlet容器编译servlet源文件，生成servlet类

2. 初始化阶段：
加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法

3. 执行阶段：
调用与JSP对应的servlet实例的服务方法

4. 销毁阶段：
    调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

## (5).基本语法
### 代码块
```java
<% 代码片段 %>
```
### 表达式
```java
<%= 表达式 %>
```

###  九大隐式对象


| **对象**    | **描述**                                                     |
| :---------- | :----------------------------------------------------------- |
| request     | **HttpServletRequest** 接口的实例                            |
| response    | **HttpServletResponse** 接口的实例                           |
| out         | **JspWriter**类的实例，用于把结果输出至网页上                |
| session     | **HttpSession**类的实例                                      |
| application | **ServletContext**类的实例，与应用上下文有关                 |
| config      | **ServletConfig**类的实例                                    |
| pageContext | **PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问 |
| page        | 类似于Java类中的this关键字                                   |
| Exception   | **Exception**类的对象，代表发生错误的JSP页面中对应的异常对象 |



## 8.MVC

### M:Model模型 V: View视图 C:Controller控制器

![mvc](C:\Users\Administrator\OneDrive\文档\markdown_img\mvc.png)