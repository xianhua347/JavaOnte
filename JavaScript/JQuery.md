# 1.简介

*jQuery*是一个快速、简洁的JavaScript框架

JQuery的对象是以伪数组的方式存储

# 2.API

## 1.选择器

| 名称       | 用法          | 描述                 |
| ---------- | ------------- | -------------------- |
| ID选择器   | $(”#id“)      | 获取指定的ID元素     |
| 全选选择器 | $("*")        | 获取所有元素         |
| 类选择器   | $(".div")     | 获取同一类Class元素  |
| 并集选择器 | $(div,p,li)   | 获取多个元素         |
| 交集选择器 | $(li.current) | 交集元素             |
| 子代选择器 | $("ul>li")    | 只获取ul里面的li元素 |
| 后代选择器 | $("ul li")    | 获取ul下的所有元素 |

## 2.隐式迭代

**给匹配到的元素进行循环，执行相对的方法**

## 3.筛选方法

| 语法               | 用法                           | 说明                   |
| ------------------ | ------------------------------ | ---------------------- |
| parent()           | $("li").paren();               | 查找父级               |
| children(selector) | $("ul").children("li")         | 查找下一级             |
| find(se;ector)     | $("ul").find("li");            | 后代选择器             |
| siblings(selector) | $(".first").siblings(selector) | 查找兄弟节点           |
| eq(index)          | $("li").eq(2)                  | 查找下下标为2的li      |
| hasClass(class)    | $('div').hasClass("protected") | 检测元素是否有特定的类 |
| nextAll[expr]      | $(".first").nextAll()          | 查找元素之后的同级元素 |
| prevtAll[expr]     | $(".last").prevtAll            | 查找元素之前的同级元素 |

## 4.属性操作

| 语法   | 说明                         |
| ------ | ---------------------------- |
| prop(“属性名”,"值") | 获得原始属性 类似于prototype |
| arrt() | 获得自定义属性 类似于getattribute |
| data()|获取属性，不会修改Dom，存储在缓存中|

## 5.遍历

| $.each(object, function (*index*, *element*) {}) | 遍历任何对象，数据处理 |
| ------------------------------------------------ | ---------------------- |
| $("div").each(function (*index*, *domEle*) {})   | 遍历dom对象，操作Dom   |

## 6 元素操作

| $(selector).html() | 获取匹配元素集中第一个元素的HTML内容 |
| ------------------ | ------------------------------------ |
| $(selector).val()  | 获取匹配input表单的值                |
| $(selector).text() | 获取元素内的**==文本内容==**         |

## 7.深拷贝和浅拷贝

### 浅拷贝
浅拷贝只是把==复杂数据类型的内存地址指向了==要继承的他属性的对象

```js
let obj = {
            id:10,
            esx:"男",
            ming:{
                age:10
            }
        }
        let target_obj={
            ming:{
                Address:"dddd"
            }
        }
        $.extend(target_obj, obj)  /* 将 object2 (相同覆盖)合并到 object1中 */
        console.log(target_obj);
```

<img src="C:\Users\Administrator\OneDrive\文档\markdown_img\浅拷贝.PNG" alt="浅拷贝" style="zoom:150%;" />

### 深拷贝
深拷贝是把==复杂数据和基本类型数据==完全拷贝一份，会开辟一块新的内存空间，而且不会覆盖复杂类型的数据
```js
let obj = {
            id:10,
            esx:"男",
            ming:{
                age:10
            }
        }
        let target_obj={
            ming:{
                Address:"dddd"
            }
        }
        $.extend(true,target_obj, obj)  /* 将 object2 (相同覆盖)合并到 object1中 */
        console.log(target_obj);
```

![浅拷贝](C:\Users\Administrator\OneDrive\文档\markdown_img\浅拷贝.PNG)

## 8.懒加载技术

### 懒加载是根据用户所在的页面来显示图片，从而减轻服务器压力

## 9.插件

## 1.fullpage

