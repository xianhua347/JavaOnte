# static
- static是跟随jvm一起启动
- static方法可以被其他的类和方法访问
- static只执行一次
- static直接可以用类型名访问 
- 只有内部类才能用static
- 静态方法无法访问非静态属性

```java
public static void go(){
        System.out.println("我可以跑");
    }
classType.go()
```



# final
final不可以被继承

不能被重新

# 内部类

1. 成员内部类

   类里面套类(不可以有静态成员)


2. 静态内部类

   有static关键字的内部类 

3. 局部内部类

   1. 方法(函数)里面建立class

4. 匿名内部类

   没有给变量名的内部类



## object
object是java里面的超类所有类都会继承他
# 异常(Exception)

### 异常的类型
- 检查类异常
		用户输入错误的数据类型，找不到文件
- 运行时异常
		下标越界等
- 错误（Error）
		虚拟机内存溢出等 

### Error和Exception的区别

1. Error通常是Java虚拟机的错误，程序直接会断开

2. Exception通常是程序错误，可以被检测到

   ## 处理机制

- 抛出

- 捕获

  五个关键字

  ```java
  try catch finally throw 
  ```
throw通常出现在方法中

装箱和拆箱

装箱：基本类型包装成引用类型

拆箱：把引用类型拆成基本类型

```java
Integer i1= 10;
        //装箱
int i2= 10;
        //拆箱
```
