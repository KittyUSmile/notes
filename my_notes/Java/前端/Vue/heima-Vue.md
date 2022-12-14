# 一、webpack

> webpack是前端工程化的一个解决方案。

**当前的前端开发规范**

- 模块化（ js 的模块化、css 的模块化、资源的模块化）
- 组件化（复用现有的 UI 结构、样式、行为）
- 规范化（目录结构的划分、编码规范化、接口规范化、文档规范化、Git 分支管理）
- 自动化（自动化构建、自动部署、自动化测试）

**webpack的主要功能**：提供了友好的前端模块化的开发支持，以及处理代码压缩混淆、处理浏览器端 JavaScript 的兼容性、性能优化等强大功能。并且目前 Vue、React 等前端项目中，基本都是通过 webpack 进行工程化开发的。



## 1、webpack 基本运用

> 先通过一个《创建列表隔行变色项目》案例感受 webpack 的使用，感受 webpack 如何为我们解决兼容性问题。

### 步骤

#### 1、初始化项目

新建项目空白目录运行 **npm init -y** 命令，初始化包管理配置文件 **package.json** [ package.json的作用相 当于pom.xml ]

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_17-31-52.png">

新建 src 源代码目录

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_17-33-25.png">

新建 src -> **index.html** 首页和 src -> **index.js** 脚本文件

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_17-34-16.png" style="zoom: 50%;" >

初始化列表页面基本的结构 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./index.js"></script>
</head>
<body>
    <ul>
        <li>这是第 1 个 li</li>
        <li>这是第 2 个 li</li>
        <li>这是第 3 个 li</li>
        <li>这是第 4 个 li</li>
        <li>这是第 5 个 li</li>
        <li>这是第 6 个 li</li>
        <li>这是第 7 个 li</li>
        <li>这是第 8 个 li</li>
        <li>这是第 9 个 li</li>
    </ul>
</body>
</html>
```

#### 2、安装 jQuery 和 webpack

运行 **npm install jquery -S** 命令，安装 jQuery **[ -S 意思是将下载的依赖记录在package.json中的dependencies节点中,-S是--save的简写,这两个都可以用,参数和安装包的顺序无所谓,当然这个参数可以被省略 ]**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_17-36-27.png">

通过 ES6 模块化的方式导入 jQuery，实现列表隔行变色，到这里如果不安装配置webpack，运行时会**提示语法问题**

```js
import $ from 'jquery'  //ES6语法

$(function () {
    $('li:odd').css('background', 'pink')
    $('li:even').css('background', 'skyblue')
})
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_18-09-36.png" style="zoom: 33%;" >

运行 **npm install webpack@5.42.1 webpack-cli@4.7.2 -D** 下载 webpack 到项目中 **[-D 意思是将下载的依赖记录在package.json中的devDependencies节点中,-D是--save-dev的简写]**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_17-38-32.png">

[^注]: package.json 文件是用来记录项目中使用到的依赖的，其中 dependencies 节点中存放的是开发和上线阶段都需要使用的包，而 devDependencies 节点中存放的是只在开发阶段需要使用到的包。

#### 3、配置 webpack

配置webpack，在项目根目录下创建名字为 **webpack.config.js** 的 webpack 配置文件，初始化为如下配置:

```js
// 使用 Node.js 中的导出语法,向外导出一个webpack的配置对象,给webpack用
module.exports = {
    mode: 'development' //mode用来指定构建模式,可选值有 development 和 production, 这两个的意思是当前项目的状态，开发模式用development,发布上线用production
}
```

然后在 package.json 的 scripts 节点下,新增 **dev 脚本**：

```js
"scripts":{
     // 设置script节点下的脚本,可以通过npm run执行指定的脚本,例如npm run dev运行对应的"webpack"脚本,其中"dev"是命名,只要命名合法可以更改,"webpack"则是脚本不能乱写
    "dev": "webpack"
}
```

#### 4、运行脚本

脚本新增完后可以在终端处执行脚本启动打包命令 **npm run dev**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_18-02-38.png">

打包成功后在目录下可以发现一个 **dist** 目录，目录下有处理过的 **main.js** 文件，替换我们原先引入的 index.js，项目报错就被解决了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="../dist/main.js"></script>
</head>
<body>
    <ul>
        <li>这是第 1 个 li</li>
        <li>这是第 2 个 li</li>
        <li>这是第 3 个 li</li>
        <li>这是第 4 个 li</li>
        <li>这是第 5 个 li</li>
        <li>这是第 6 个 li</li>
        <li>这是第 7 个 li</li>
        <li>这是第 8 个 li</li>
        <li>这是第 9 个 li</li>
    </ul>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/13/Snipaste_2022-12-13_18-08-23.png" style="zoom:33%;" >





