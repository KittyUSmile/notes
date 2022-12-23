# 一、原生 AJAX

> 通过原生 AJAX 向服务器发送请求，并处理返回数据。

## 1、Ajax简介

> AJAX 全称为 Asychronous JavaScript And XML，意思是异步的 JS 和 XML。通过 AJAX 可以在浏览器中向服务器发送异步请求，最大的优势就是可以无刷新获取数据。因此 AJAX 不是一种新的编程语言，而是一种将现有的标准组合在一起使用的新方式。                                                                                                                                                                                                                   



## 2、NodeJS的安装和介绍

> NodeJS 其实就是运行 JS 代码的环境，直接在官网安装即可。并通过在命令框输入 node -v 测试是否安装成功。



## 3、express框架安装与介绍

> Express 其实就是用来模拟服务端后台。

**安装**

```apl
// 打开 vscode 中的项目终端，先初始化项目
npm init -yes

// 安装 express 框架
npm i express
```

**创建 js 文件**

```js
// 先通过 npm init -yes 命令初始化，再通过 npm i express 下载express
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

// 3、创建路由规则
app.get('/',(request, response) => {
    //设置响应
    response.send('HELLO EXPRESS');
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})

// 5、在终端输入 node express_use.js 启动
```



## 4、nodemon工具安装

> nodemon工具的作用是自动重启修改过的 js 脚本，不用每次修改 js 手动重启。nodemon 的安装很简单，直接在vscode终端输入一行命令即可，但前提是电脑或服务器已安装 nodejs。

**安装命令**

```apl
npm install -g nodemon
```

- 安装成功后，使用命令 **nodemon xx.js** 重启 js 服务即可。



## 5、Ajax请求

### 1、入门案例

> Ajax中最重要的一个对象：XMLHttpRequest 对象，一切请求发送操作都是由它发送。

#### GET

> 通过 AJAX 发送 GET 请求。

**server.js**

```js
// 先通过 npm init -yes 命令初始化，再通过 npm i express 下载express
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

// 3、创建路由规则
app.get('/server',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体 只能发送字符串和buffer
    response.send('HELLO AJAX');
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})

// 5、在终端输入 node express_use.js 启动

```

**GET.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #result{
            width: 200px;
            height: 100px;
            border: solid 1px pink;
        }
    </style>
</head>
<body>
    <button>发送请求</button>
    <div id="result"></div>
</body>
<script>
    const btn = document.getElementsByTagName('button')[0];
    btn.onclick = function(){
        // 开始 Ajax 操作
        //1、创建对象
        const xhr = new XMLHttpRequest();
        //2、初始化，设置请求方法和url
        xhr.open('GET','http://localhost:8000/server?a=100&b=100&c=12');
        //3、发送
        xhr.send();
        //4、事件绑定 处理服务端返回的结果
        /**
         * on: when 当...时候  readystate: xhr中的属性，表示状态 0,1,2,3,4  change: readystate发生改变
         *   0: 未初始化
         *   1: open()方法调用完毕
         *   2: send()方法调用完毕
         *   3: 服务端返回部分结果
         *   4: 服务端返回全部结果
         */
        xhr.onreadystatechange = function(){
            //在状态为 4 时处理返回结果
            if(xhr.readyState == 4){
                // 判断响应状态码 200 404 403 401 500
                // 2xx 都表示成功
                if(xhr.status >= 200 && xhr.status < 300){
                    //处理结果
                    //1、响应行
                    console.log(xhr.status); // 状态码
                    console.log(xhr.statusText); // 状态字符串
                    console.log(xhr.getAllResponseHeaders()); // 所有响应头
                    console.log(xhr.response); // 响应体
                    // 设置 result 的文本
                    document.getElementById('result').innerHTML = xhr.response;
                }else{

                }
            }
        }
    }
</script>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/27/Snipaste_2022-11-27_16-07-45.png">

#### POST

**server.js**

```js
// 先通过 npm init -yes 命令初始化，再通过 npm i express 下载express
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

app.all('/server',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置允许所有响应头
    response.setHeader('Access-Control-Allow-Headers','*');
    //设置响应体 只能发送字符串和buffer
    response.send('HELLO AJAX POST');
});


// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})

// 5、在终端输入 node express_use.js 启动

```

**POST.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #result{
            width: 200px;
            height: 200px;
            border: solid 1px pink;
        }
    </style>
</head>
<body>
    <div id="result"></div>
    <script>
        const result = document.getElementById('result');
        result.addEventListener("mouseover", ()=>{
            const xhr = new XMLHttpRequest();
            xhr.open("POST", 'http://localhost:8000/server');
            //设置请求头
            xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
            // 自定义请求头 需要后端设置允许所有请求头
            xhr.setRequestHeader('run','fu');
            // POST 设置参数方式 1
            // xhr.send('a=100&b=200&c=10');
            // POST 设置参数方式 2
            xhr.send('a:100&b:200&c:10');
            xhr.onreadystatechange = () => {
                if(xhr.readyState == 4){
                    if(xhr.status >= 200 && xhr.status < 300){
                        result.innerHTML = xhr.response;
                    }
                }
            }

        })
        result.addEventListener("mouseleave", () => {
            result.innerHTML = '';
        })
    </script>
</body>
</html>
```

### 2、设置请求参数

#### GET

> 直接写在请求后面即可。

```js
xhr.open('GET','http://localhost:8000/server?a=100&b=100&c=12');
```

#### POST

> POST 设置请求参数在 send 时设置。

```js
// POST 设置参数方式 1
// xhr.send('a=100&b=200&c=10');
// POST 设置参数方式 2
xhr.send('a:100&b:200&c:10');
```



### 3、设置请求头参数

> 直接通过 XMLHttpRequest 对象设置。

```js
//设置请求头
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
// 自定义请求头 需要后端设置允许所有请求头
xhr.setRequestHeader('run','fu');
```

### 4、JSON数据响应

> 对后端返回的 JSON 数据进行快速响应。有两种方式，手动转化和自动转化

**server.js**

```js
app.all('/json-server',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置允许所有响应头
    response.setHeader('Access-Control-Allow-Headers','*');
    //响应数据
    const data = {
        name: 'Jack'
    };
    //设置响应体
    response.send(JSON.stringify(data));
});
```

**JSON.html**

```js
<script>
    const result = document.getElementById('div1');
    document.getElementsByTagName('button')[0].addEventListener("click",()=>{
        const xhr = new XMLHttpRequest();
        //2、自动转化，先设置响应体数据的类型
        xhr.responseType = 'json';
        xhr.open('GET', 'http://localhost:8000/json-server?t=' + Date.now());
        xhr.send();
        xhr.onreadystatechange = () => {
            if(xhr.readyState == 4){
                if(xhr.status >= 200 && xhr.status < 300){
                    //1、手动转化
                    let data = JSON.parse(xhr.response);
                    console.log(data)	// {name:'Jack'};
                    //2、自动转化
                    result.innerHTML = xhr.response;
                    console.log(data)	// {name:'Jack'};
                }
            }
        }
    })
</script>
```



### 5、手动取消Ajax请求

> XMLHttpRequest 提供了一个方法取消请求发送：abort（）

```html
<body>
    <button>发送请求</button>
    <button>取消发送</button>
    <script>
        let xhr = new XMLHttpRequest();
        const btns = document.querySelectorAll('button');
        btns[0].onclick = () => {
            xhr.open('GET', 'http://localhost:8000/delay');
            // 发送
            xhr.send();
        }
        btns[1].onclick = () => {
            // 取消发送
            xhr.abort();
        }
    </script>
</body>
```



## 6、Ajax问题解决

### 1、IE 浏览器缓存问题

> IE 浏览器针对多次同一请求，会利用缓存返回相同结果，因此服务器端更新的数据不能及时获取，这种问题可以通过加上时间戳解决。一般而言，这个问题不需要考虑，一般以后使用的工具都会解决这个问题。

**server.js**

```js
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

// 3、创建路由规则
app.get('/ie',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体 只能发送字符串和buffer
    response.send('HELLO AJAX');
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})
```

**IEGet.html**

```js
// 在参数传递时加上一个时间戳参数，让IE识别每次请求都不同
xhr.open('GET', 'http://localhost:8000/ie?t=' + Date.now());
```



### 2、请求超时与网络异常处理

> 如果出现请求超时或者网络异常问题，一般会给用户一些提示信息。

**delay.js**

```js
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

// 3、创建路由规则
app.all('/delay',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体 [延迟三秒]
    setTimeout(() => {
        response.send('延时响应');
    }, 3000);
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})
```

**delay.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #ie{
            width: 200px;
            height: 200px;
            border: 1px solid pink;
        }
    </style>
</head>
<body>
    <button>按钮</button>
    <div id="ie"></div>
    <script>
        const result = document.getElementById('ie');
        document.getElementsByTagName('button')[0].addEventListener("click",()=>{
            const xhr = new XMLHttpRequest();
            // 设置两秒超时 [超时后取消请求]
            xhr.timeout = 2000;
            xhr.ontimeout = () => {
                alert('网络异常,请稍后重试');
            }
            //网络异常回调
            xhr.onerror = () => {
                alert('你的网络似乎出了一些问题')
            }
            xhr.open('GET', 'http://localhost:8000/delay');
            xhr.send();
            xhr.onreadystatechange = () => {
                if(xhr.readyState == 4){
                    if(xhr.status >= 200 && xhr.status < 300){
                        result.innerHTML = xhr.response;
                    }
                }
            }
        })
    </script>
</body>
</html>
```



### 3、请求多次发送问题

> 有时候用户可能会多次点击请求发送按钮，短时间内反复给服务器发送相同请求，这可能会增大服务器压力，所以可以做出一些限制，让同一时间只有一个请求在发送。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button>发送请求</button>
    <script>
        let xhr = new XMLHttpRequest();
        const btns = document.querySelectorAll('button');
        let isSending = false;
        btns[0].onclick = () => {
            // 正在发送请求, 取消同一请求
            if(isSending) xhr.abort();
            isSending = true;
            xhr.open('GET', 'http://localhost:8000/delay');
            xhr.send();
            xhr.onreadystatechange = () => {
                if(xhr.readyState == 4){
                    //成功接收到响应数据后, 修改为未发送状态
                    isSending = false;
                }
            }
        }
    </script>
</body>
</html>
```



# 二、JQuery 中的 AJAX

> JQuery 可以直接使用远程库，在 BootCDN 中搜索 JQuery 并引入即可。在 jQuery 中直接通过 $.get 和 $.post 进行发送请求

**server.js**

```js
// 先通过 npm init -yes 命令初始化，再通过 npm i express 下载express
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

// 3、创建路由规则
app.all('/json-server',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体
    const data = {name: 'aaa'};
    response.send(JSON.stringify(data));
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})

// 5、在终端输入 node express_use.js 启动
```

## get

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button></button>
    <script>
        $('button').eq(0).click(function () {
            $.get('http://localhost:8000/json-server',
                { a: 100, b: 200 },
                function (data) {
                    console.log(data);
                },'json');
        })
    </script>
</body>
```

## post

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button></button>
    <script>
        $('button').eq(1).click(() => {
            $.post('http://localhost:8000/json-server',
                { a: 100, b: 200 },
                function (data) {
                    console.log(data);
                },'json');
        })
    </script>
</body>
```

## 通用请求

> 通用请求发送，使用频繁。

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button></button>
    <script>
        $('button').eq(2).click(function(){
            $.ajax({
                //url
                url: 'http://localhost:8000/json-server',
                //参数
                data: {a:100, b:200},
                //请求类型
                type: 'GET',
                //响应体结果设置 [json自动转对象]
                dataType: 'json',
                //成功回调
                success: function(data){
                    console.log(data);
                },
                //超时时间 [ms]
                timeout: 2000,
                //失败回调
                error: function(data){
                    console.log('出错了');
                },
                //请求头信息设置 [自定义请求头需要后端支持]
                headers: {
                    a:100,
                    b:200
                }
            })
        })
    </script>
</body>
```



# 三、axios 发送 AJAX 请求

> axios 是 AJAX 的工具库，它专注于数据请求。

**server.js**

```js
// 先通过 npm init -yes 命令初始化，再通过 npm i express 下载express
// 1、引入express
const express = require('express');

// 2、创建应用对象
const app = express();

app.all('/axios-server',(request, response) => {
    //设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置允许所有响应头
    response.setHeader('Access-Control-Allow-Headers','*');
    //设置响应体
    const data = {name: 'aaa'};
    response.send(JSON.stringify(data));
});

// 4、监听端口启动服务
app.listen(8000, () => {
    console.log("服务已启动, 8000端口监听中");
})

// 5、在终端输入 node express_use.js 启动

```

## get

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>
    <script>
        //配置baseURL
        axios.defaults.baseURL = 'http://localhost:8000';
        // GET
        $('button').eq(0).click(function () {
            axios.get('/axios-server', {
                // url 参数
                params: {
                    id: 100,
                    name: '张三'
                },
                // 请求头信息
                headers: {
                    name: 'zs',
                    age: 18
                }
            }).then(value => {
                // 完整的对象，包括返回数据、请求XMLHttpRequest对象、请求头、状态码等信息
                console.log(value);
            })
        })
    </script>
</body>
```

## post

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>
    <script>
        //配置baseURL
        axios.defaults.baseURL = 'http://localhost:8000';
        // POST
        $('button').eq(1).click(function(){
            axios.post('/axios-server',
                {   //url 参数
                    id: 200,
                    name: '李四'
                } ,{
                // 请求头信息
                headers: {
                    name: 'lisi',
                    age: 18
                },
                // 请求体
                data: {
                    username: 'lisi',
                    password: '123'
                }
            })
        })
    </script>
</body>
```

## 通用方式

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>
    <script>
        //配置baseURL
        axios.defaults.baseURL = 'http://localhost:8000';
        // 通用方式
        $('button').eq(2).click(function(){
            axios({
                // 请求方式
                method: 'POST',
                // url
                url: '/axios-server',
                // url参数
                params:{
                    token: 'abab'
                },
                // 头信息
                headers:{
                    a:100,
                    b:200
                },
                //请求体参数
                data:{
                    username:'admin',
                    password:'admin'
                }
            })
        }).then(response=>{
            console.log(response);
        })
    </script>
</body>
```

# 四、解决跨域

## 1、JSONP

> JSONP借助了一些标签本身具有的跨域功能完成跨域，比如 \<script>\</script>标签，在 js 文件中对页面元素进行操作，再通过引入这个 js 文件完成跨域。



