# ajax(Asynchronous Javascript And XML And HTML)

## 1.简介

使用Ajax技术网页应用能够快速地将增量更新呈现在[用户界面](https://baike.baidu.com/item/用户界面/6582461)上，而不需要重载（刷新）整个页面，这使得程序能够更快地回应用户的操作。

## 2.原生ajax
#### 步骤
1. 创建XMLHttpRequest对象
2. 初始化请求
3. 发送请求
4. 处理返回数据
#### 代码
```js
//创建异步对象
var xhr = new XMLHttpRequest();
//设置请求行open()
xhr.open("get", " http://localhost:8000?a=100");
//发送请求
xhr.send();
//处理服务器传来的结果
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    //0: 请求未初始化
    // 1: 服务器连接已建立
    // 2: 请求已接收 接收到了响应头
    // 3: 请求处理中 正在下载响应体
    // 4: 请求已完成，且响应已就绪
    if (xhr.status >= 200 && xhr.status < 300) {
      
    }
  }
```

#### readyState状态码

0: 请求未初始化
1: 服务器连接已建立
2: 请求已接收 接收到了响应头
3: 请求处理中 正在下载响应体
4: 请求已完成，且响应已就绪

#### 取消请求的方法

```javascript
xhr.abort();
```
#### 设置请求超时时间和回调函数
```javascript
xhr.timeout =2000;
xhr.onoutitme=function(){}
```

## 3.JQuery ajax
#### 请求方法
1. $.get()方法
2. $.post()方法
3. $.ajax()方法
#### 代码
##### $get()
```js
$.get("请求地址", {"实体"}, function (data) {
    //回调函数
},'数据类型');
```

##### $.ajax()

```javascript
$.ajax({
 type: "GET",  //类型
 url: "url",  //请求地址
 data: "data",  //格式{key:value}
 dataType: "json",
 beforeSend: function () {}, //请求发送前回调,常用验证
 success: function (response) {  //请求成功回调
   
 },
 error: function (e) {  //请求超时回调
   if(e.statusText == "timeout"){
     alert("请求超时")
   }
 },
 complete: function () {}, //无论请求是成功还是失败都会执行的回调，常用全局成员的释放，或者页面状态的重置
)
```



### 4.axios

##### Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中

[axios中文网|axios API 中文文档 | axios (axios-js.com)](http://www.axios-js.com/zh-cn/)

##### 请求方法

1. axios.get

2. axios.post

3. axois API

#### 代码
axios.get and axios.post
```js
axios.get("http://localhost:8000/axios", {
  params: {
    id: 123456,
      //url
  },
})
.then((response) => {})
.catch((error) => {});
```

axios API

```javascript
axios({
 method:'post',
 url:'http://localhost:8000/axios',
 params:{
     v:10,
     lev:30
 },
 Headers:{
     a:100,
     b:300,
 },
 data:{
     name:'admin',
     pw:"123"
     //实体
 },
 responseType:'json'
}).then(response=>{})
.catch((error) => {});
```

### 5.feach

https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API#fetch_%E6%8E%A5%E5%8F%A3
#### 特点

1. 基于异步函数 es7的方法
2. 返回来的数据是Obj要进行处理

```js
fetch('url', {
 method: 'post',
 body: JSON.stringify({uname: '张三',pwd: '789'}),
 headers: {'Content-Type': 'application/json'}
 }).then(function(data){
   return data.text()
    //处理数据
}).then(function(data){
 console.log(data)
})
```

### 6.跨域问题



跨域是网景公司制定的一个浏览器访问安全规则

#### 同域规则：协议端口主机必须一致

#### 跨域的两种方法

1. 通过jsonp跨域

   1. 前端动态创建script标签

      ```js
      const input = document.querySelector("input");
          const p = document.querySelector("p");
          function handle(data) {
              //插入数据方法
            input.style.border = "1px solid red";
            p.innerHTML = data.msg;
          }
          input.onblur = function () {
              //鼠标移出事件
            const script = document.createElement("script");
              //创建script
            script.src = "http://localhost:8000/check";
              //crc指向请求地址
            document.body.appendChild(script);
              //插入到页面
          };
      ```

      

   2. 后端返回 JavaScript全局函数

      ```js
      const data = {
          msg: "用户名已经存在",
        };
      const str = JSON.stringify(data);
      response.send(`handle(${str})`);
      //返回的是调用方法
      ```

      

##### 跨域资源共享（CORS）

###### 只服务端设置Access-Control-Allow-Origin即可

#### 详细跨域解决方法

https://segmentfault.com/a/1190000011145364
