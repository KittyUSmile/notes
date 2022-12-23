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

### 1、初始化项目

新建项目空白目录运行 **npm init -y** 命令，初始化包管理配置文件 **package.json** [ package.json的作用相 当于pom.xml ]

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_17-31-52.png">

新建 src 源代码目录

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_17-33-25.png">

新建 src -> **index.html** 首页和 src -> **index.js** 脚本文件

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_17-34-16.png">

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



### 2、安装 webpack

运行 **npm install jquery -S** 命令，安装 jQuery **[ -S 意思是将下载的依赖记录在package.json中的dependencies节点中,-S是--save的简写,这两个都可以用,参数和安装包的顺序无所谓,当然这个参数可以被省略 ]**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_17-36-27.png">

通过 ES6 模块化的方式导入 jQuery，实现列表隔行变色，到这里如果不安装配置webpack，运行时会**提示语法问题（ES6新语法导致）**

```js
import $ from 'jquery'  //ES6语法

$(function () {
    $('li:odd').css('background', 'pink')
    $('li:even').css('background', 'skyblue')
})
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_18-09-36.png">

运行 **npm install webpack@5.42.1 webpack-cli@4.7.2 -D** 下载 webpack 到项目中 **[-D 意思是将下载的依赖记录在package.json中的devDependencies节点中,-D是--save-dev的简写]**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_17-38-32.png">

[^注]: package.json 文件用来记录项目中使用到的依赖，而 dependencies 节点中存放的是开发和上线阶段都需要使用的包， devDependencies 节点中存放的是开发阶段需要使用到的包。



### 3、配置 webpack

配置webpack，在项目根目录下创建名字为 **webpack.config.js** 的 webpack 配置文件，初始化为如下配置:

```js
// 使用 Node.js 中的导出语法,向外导出一个webpack的配置对象,给webpack用
module.exports = {
    //mode用来指定构建模式,可选值有 development 和 production, 这两个的意思是当前项目的状态，开发模式用development,发布上线用production,使用production时webpack会自动压缩代码,打包时间也会略长
    mode: 'development' 
}
```

然后在 package.json 的 scripts 节点下,新增 **dev 脚本**：

```js
"scripts":{
     // 设置script节点下的脚本,可以通过npm run执行指定的脚本,例如npm run dev运行对应的"webpack"脚本,其中"dev"是命名,只要命名合法可以更改,"webpack"则是脚本不能乱写
    "dev": "webpack"
}
```



### 4、运行脚本

脚本新增完后可以在终端处执行脚本启动打包命令 **npm run dev**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-13_18-02-38.png">

打包成功后在目录下可以发现一个 **dist** 目录，目录下有处理过的 **main.js** 文件，替换我们原先引入的 index.js，项目报错就被webpack解决了

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

[^注]: 运行 npm run dev 命令时，先会去 package.json 文件中找到脚本 “dev”，发现命令是 webpack，然后会再去项目根目录下找到 webpack 配置文件：webpack.config.js，查询 webpack 的构建模式。



## 2、webpack 约定/配置

> webpack的一些默认约定或者配置。

#### 默认打包约定

webpack默认找 src -> index.js 文件进行打包，然后放到 dist -> main.js 下。 如果需要修改这个默认规则，可以在 webpack 的配置文件 webpack.config.js 中进行配置，修改如下：

```js
const path = require('path') //导入 node.js 中专门操作路径的模块

module.exports = {
    entry: path.join(__dirname, './src/index.js'),	// 打包入口文件的路径
    output: {
        path: path.join(__dirname, './dist'),	// 输出文件的存放路径
        filename: 'main.js'	//输出文件的名称
    }
}
```



#### 自启动配置

> 打包完毕后，自动启动服务器，访问指定端口。

在 **webpack.config.js** 配置文件中，通过 devServer 节点配置自启动和端口：

```js
module.exports = {
    mode: 'development',
    devServer:{
        open: true,   //初次打包完成后,自动打开浏览器
        host: '127.0.0.1',  //主机地址
        port: 80    //端口号
    }
}
```



#### 源码错误行数定位配置

> 因为浏览器运行的是我们打包过后的 js 文件，如果出现报错，那么提示的是打包后的 js 文件的行数，而不是我们开发时的 js 文件行数，这对我们进行错误定位不利，所以需要进行一下配置，让浏览器报错的行数和我们开发的 js 文件的行数一致。

在 **webpack.config.js** 中进行配置：

```js
module.exports = {
    // 在开发调试阶段,建议把 devtool 的值设置为 eval-source-map,但是不建议在生产模式下使用
    devtool: 'eval-source-map',
    ...
}
```

这里有一个问题需要注意：如果在生产模式下仍然使用 **eval-source-map**，用户也可以发现错误行数和查阅报错的代码，这样做很不安全，因此在生产环境下，最好使用 **nosource-source-map**，这样既会提示出错误行数，又不会暴露报错代码，推荐**在生产环境下使用**：

```js
module.exports = {
    // 在生产模式阶段,建议把 devtool 的值设置为 nosource-source-map
    devtool: 'nosource-source-map',
    ...
}
```



#### 别名配置

> 用别名表示想要表示的部分，比如用 ’@‘ 符号表示根目录，需要在 webpack.config.js 中进行配置。

在 **webpack.config.js** 下的 **resolve** 模块中配置：

```js
const path = require('path') //导入 node.js 中专门操作路径的模块

module.exports = {
    ...
    resolve:{
        alias:{
            // 告诉 webpack, '@'符号表示 src 根目录
            '@': path.join(__dirname, './src/')
        }
    },
}
```



## 3、webpack 插件



#### 自动重新打包插件

> 不安装插件时，每次修改了自己的 js 文件，就需要重新运行插件更新 js 文件，比较麻烦，可以通过插件自动帮我们完成这部分操作。

运行如下命令安装插件：

```js
npm install webpack-dev-server -D
```

安装完成后进行**配置**：

1. 更改 **package.json -> script** 中的 dev 命令如下：

   ```js
   "scripts":{
       "dev": "webpack serve"
   }
   ```

2. 再次运行 **npm run dev** 命令，打包启动，以后只要修改源代码，保存后就会自动重新打包（**如果出现unable 问题，去掉下载的版本中的 @ 版本号**），需要关闭则连按两次 ctrl + c 即可。

   <img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-14_15-52-40.png">

3. 自动打包没问题了，但是会发现无论如何修改 index.js，index.html 样式都不会改变，这是因为首先，webpack 打包插件的热更采用了 http 协议，不是 file 协议，默认访问 **localhost:8080**，访问文件即可。其次，webpack 的打包插件每次新生成的 main.js 并不存在于磁盘中，而是存在于**项目根目录的内存中（不是磁盘，因此看不到）**，需要修改 index.html 的引用文件为内存中的 main.js 文件，修改完成后就能生效了。

   ```html
   <script src="/main.js"></script>
   ```



#### HTML自动复制插件

> HTML 复制插件可以自动把 html 文件复制到其它目录的内存中，一般用于对 src -> index.html 的复制，可以将其复制到项目根目录的内存中，这样和 **自动打包插件** 一起使用时，直接访问 **localhost:8080** 即可访问 index.html 文件。

运行如下命令安装插件（**可以去掉@版本号**）：

```js
npm install html-webpack-plugin -D
```

安装完成后在 webpack.config.js 中进行**配置**：

```js
// 1、导入 HTML 插件，得到一个构造函数
const HtmlPlugin = require('html-webpack-plugin')

// 2、创建 HTML 插件的实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',    //指定源文件的存放路径
    filename: './index.html',        //指定生成的文件的存放路径
})

module.exports = {
    mode: 'development',
    // 通过plugins节点, 将插件加入webpack的配置对象, 使之生效
    plugins: [htmlPlugin]
}
```

配置完成后，启动命令 **npm run dev**，直接访问 **localhost:8080** 即可访问 **index.html** 文件。这里有个注意点：**如果在 index.html 不引入 main.js 文件，复制插件会从内存中找出来并自动引入**。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-14_17-17-31.png">



#### 自动删除 dist 目录插件

> 每次打包生成新 dist 目录前自动删除旧 dist 目录，省去程序员自己删除的功夫。

通过以下命令下载插件：

```js
npm install clean-webpack-plugin -D
```

在 **webpack.config.js** 中配置插件：

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    ...
    plugins: [...,new CleanWebpackPlugin()],
    ...
}
```



## 4、webpack loader

> webpack 默认只能处理 js 文件，如果 webpack 处理的 js 文件中引入了其它类型的文件，比如 .css，就需要提供相应 loader 加载器进行处理，否则就会报错。

#### CSS loader

以 css 文件为例，**安装和配置**对应的 loader 进行处理，首先通过以下命令**安装 css 的加载器**：

```js
npm i style-loader css-loader -D
```

安装成功后在 **webpack.config.js** 的 **module -> rules** 数组中中进行配置：

```js
module.exports = {
    module:{    // 所有第三方文件模块的匹配规则
        rules:[  // 文件后缀名的匹配规则
            // /xxx/表示正则,$表示结尾,\转义
            {test: /\.css$/, use:['style-loader', 'css-loader']} 
        ]
    }
}
```



#### less loader

如果要加载 less 文件，则需要**安装 less 的加载器**，可以通过以下命令**安装 less 加载器**：

```js
npm i less-loader less -D
```

然后在 **webpack.config.js** 的 **module -> rules** 数组中中配置：

```js
module.exports = {
    module:{    // 所有第三方文件模块的匹配规则
        rules:[  // 文件后缀名的匹配规则
            // /xxx/表示正则,$表示结尾,\转义
            {test: /\.less$/, use:['style-loader', 'css-loader', 'less-loader']} 
        ]
    }
}
```



#### file loader

> 第三方文件加载器，可以帮助 webpack 打包第三方文件，如图片（.jpg、.png、.gif）

先通过以下命令**安装文件加载器**：

```js
npm i url-loader file-loader -D
```

然后在 **webpack.config.js** 的 **module -> rules** 数组中，添加 loader 匹配规则：

```js
module.exports = {
    module:{    // 所有第三方文件模块的匹配规则
        rules:[  // 文件后缀名的匹配规则
            // /xxx/表示正则,$表示结尾,\转义,|表示或者,limit用来指定图片的大小,单位是字节,小于该限制的图片才会被转为 base64 图片
            {test: /\.jpg|png|gif$/, use:'url-loader?limit=22229'} 
        ]
    }
}
```



#### babel loader

> 用来加载 webpack 无法处理的 js 高级语法（webpack只能打包处理一部分高级的 js 语法）。

先通过以下命令安装对应的依赖包：

```js
npm i babel-loader @babel/core @babel/plugin-proposal-decorators -D
```

然后在 **webpack.config.js** 的 **module -> rules** 数组中，添加 loader 匹配规则：

```js
module.exports = {
    module:{    // 所有第三方文件模块的匹配规则
        rules:[  // 文件后缀名的匹配规则
            // /xxx/表示正则,$表示结尾,\转义, exclude表示需要排除加载的包,/node_modules/一般都是第三方包,已经做好了兼容性,不需要我们管
            {test: /\.js$/, use:'babel-loader', exclude: /node_modules/ } 
        ]
    }
}
```

当然，如果 js 中使用了 装饰器语法（@），需要配置一下插件，配置过程如下：

在根目录下创建名为 **babel.config.js** 的配置文件，定义 Babel 的配置项为：

```js
module.exports = {
    // 声明 babel 可用的插件
    plugins:[['@babel/plugin-proposal-decorators',{legacy: true}]]
}
```



[^注]: webpack 加载流程为：当 webpack 遇到自己不能加载的文件时，先去 module -> rules 中找有没有这个文件后缀名的加载器，如果没有，则报错，有则从配置的 use 中找到加载器从后往前传递加载，以以上为例，先交由 css-loader 加载，css-loader 加载完毕后，交由 style-loader 加载，style-loader 加载完毕后，把加载结果交还给 webpack，webpack 再将结果整合进 main.js 中。



## 5、webpack 打包

> 前端代码写完后，需要将具体的文件打包存储到硬盘中，将具体的文件返回给后端，让后端进行发布上线，这里有个问题，使用 webpack 提供的插件时，如自动打包、HTML复制插件时，相关的 main.js 等文件存在于内存中，因此需要打包将其生成在硬盘上。

打包的步骤很简单，在 **package.json** 中的 **scripts** 节点下新增一个 脚本就可以了，需要打包时通过命令 **npm run build** 运行该脚本：

```js
"scripts": {
    // "build"是脚本的名字,可以随便取,打包命令是"webpack", "--mode production"是打包的参数,作用是将打包的模式改为production,上线模式,该模式打包时会进行压缩优化
   "build": "webpack --mode production"
}
```

打包成功后，会在根目录下生成一个 **dist** 目录，里面有压缩过的前端文件。这里有一个问题，如果打包后的文件都挤在 **dist** 目录下，就不符合规范，可以通过配置让指定的文件放到指定文件夹下分好类。以下进行文件的指定打包分类：



#### js 文件

通过 **webpack.config.js** 的 **output** 节点指定存放位置（**需要导入 node.js 中的 path 模块**）：

```js
const path = require('path') //导入 node.js 中专门操作路径的模块

module.exports = {
    ...
    output: {
        path: path.join(__dirname, './dist'),	// 输出文件的存放路径
        filename: 'js/main.js'	//将 main.js 文件输出到js目录下
    },
    ...
}
```



#### image 文件

通过 **webpack.config.js** 的 **module** 节点进行配置：

```js
module.exports = {
    module:{    // 所有第三方文件模块的匹配规则
        rules:[  // 文件后缀名的匹配规则
            // /xxx/表示正则,$表示结尾,\转义,|表示或者,limit用来指定图片的大小,单位是字节,小于该限制的图片才会被转为 base64 图片
            // 同时指定把打包生成的图片文件存储到 dist 目录下的 image 文件夹中
            {test: /\.jpg|png|gif$/, use:'url-loader?limit=22229&outputPath=images'} 
        ]
    }
}

```



# 二、Vue



## 1、简介

> vue，一套用于构建用户界面的前端框架。它可以很方便的往 HTML 页面中填充数据，只要我们遵守它的规范（不遵守的话后期维护很难）

### 核心知识点

指令、组件（对 UI 结构的复用）、路由、Vuex、vue组件库。



### 两个特性

1. **数据驱动视图**
   - Vue 会作为页面结构和页面数据之间的桥梁，页面数据不再由我们通过 DOM 手动操作，而是交给 Vue 自动渲染，只要页面数据发生变化，Vue 就会为我们自动渲染数据到页面中。需要注意的是，**数据驱动视图是单向数据绑定，即数据的变化会导致页面的变化。**
   - 数据驱动视图的好处是我们只管把数据维护好，页面结构会被 Vue 自动渲染出来。
2. **双向数据绑定**
   - 双向数据绑定主要体现在表单上，传统作法是用一个数据源来对应一个 DOM 元素，DOM 元素被修改时通过 DOM 操作获取值赋给数据源，数据源被修改再通过 DOM 操作赋给 DOM 元素，这样做重复且耗时，而Vue 的双向数据绑定为我们把这部分工作做了，可以让开发者在不操作 DOM 元素的前提下，**自动把 DOM 元素的内容同步到数据源，也可以同时把数据源的内容同步到 DOM 元素。**



### MVVM

> MVVM 是 Vue 的工作模式，也是一种代理操作的编程思想。

**M（Model：数据源）V（View：视图）VM（ViewModel：Vue 实例）**，其中 VM 是 Vue 的核心，它介于 Model 和 View 之间，监听DOM、监听数据源、自动更新视图、自动同步数据源，这些操作都是 VM 提供的。我们学习 Vue 其实主要就是学习 VM 提供的功能和它是怎么工作的。



## 2、基本使用

### 安装调试工具

使用 Vue 可以在 google 浏览器中先安装 **vue.js devtools** 插件，并设置允许访问文件网址，这个插件可以提高我们 Vue 的开发效率。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-16_16-06-13.png">



<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-16_16-07-12.png">





### 入门案例

> 一个使用 Vue 的简单入门案例。

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
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <!-- {{}}: Vue中的固定语法,绑定Vue实例中的data中的哪个数据-->
        {{username}}
    </div>
</body>

<script>
    const vm = new Vue({
        // el: vue绑定DOM元素的固定写法, #表示id选择器
        el: '#app',
        // data: 要渲染到绑定的 DOM 元素上的数据
        data: {
            username: '我是张三'
        }
    })
</script>

</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_15-56-09.png">

## 3、指令

> 指令（Directives）是 vue 为开发者提供的模板语法，用于辅助开发者渲染页面的基本结构。

vue中的指令按照不同的用途可以分为如下**六大类**：

1. **内容渲染**指令
2. **属性绑定**指令
3. **事件绑定**指令
4. **双向绑定**指令
5. **条件渲染**指令
6. **列表渲染**指令



### 内容渲染指令

> 内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有以下三个。

#### v-text

> v-text: 用于标签内容和 Vue 对象的 data 中的属性的单向绑定，常用作给标签内容赋值或者覆盖标签内容。
>
> 语法格式：v-text=“绑定属性”

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

    <div id="app">
        <!-- 把 username 对应的值，渲染到 p 标签内容中-->
        <p v-text="username"></p>

        <!-- 把 gender 对应的值，渲染到 p 标签内容中，如果该标签内容有值，则会被覆盖掉 -->
        <p v-text="gender">女</p>
    </div>
</body>

<script>
    const vm = new Vue({
        // el: vue绑定DOM元素的固定写法, #表示id选择器
        el: '#app',
        // data: 要渲染到绑定的 DOM 元素上的数据
        data: {
            username: '我是张三',
            gender: '男'
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_16-29-03.png">



#### {{}}

> 插值表达式，用于不覆盖情况下的数据填充，起占位符作用，可以替代 v-text。但是要注意，插值只能用在元素内容上，不能用在元素属性上。

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

    <div id="app">
        <p>姓名: {{ username }}</p>
        <p>性别: {{ gender }}</p>
    </div>
</body>

<script>
    const vm = new Vue({
        // el: vue绑定DOM元素的固定写法, #表示id选择器
        el: '#app',
        // data: 要渲染到绑定的 DOM 元素上的数据
        data: {
            username: '我是张三',
            gender: '男'
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_16-30-05.png">

 {{}} 除了支持插值表示，还支持 **JavaScript 表达式**的运算，例如：

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        {{number + 1}}
        <hr>
        [{{tips}}] 的反转: [{{tips.split('').reverse().join('')}}]
        <hr>
        {{ok ? false : true}}
    </div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            number: 1,
            tips: 'switch',
            ok: true
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_17-41-41.png">



#### v-html

> 将绑定的数据源内容解析渲染到标签内容中。
>
> 语法格式：v-html=“绑定属性”

**使用示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

    <div id="app">
        <p>姓名: {{ username }}</p>
        <p>性别: {{ gender }}</p>
        <p v-html="star"></p>
    </div>
</body>

<script>
    const vm = new Vue({
        // el: vue绑定DOM元素的固定写法, #表示id选择器
        el: '#app',
        // data: 要渲染到绑定的 DOM 元素上的数据
        data: {
            username: '我是张三',
            gender: '男',
            star: '<p style="color:red">白羊座</p>',
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_16-33-16.png">

### 属性绑定指令

> 属性绑定指令用来单向绑定数据源和元素属性。指令是 v-bind: ，在使用中，v-bind: 可以直接简写为冒号。

#### v-bind: / :

> v-bind: 用于标签属性和 Vue 对象的 data 中的属性的单向绑定，:属性=”“ 的 “” 中可以写一些简单的 JS 表达式。
>
> 语法格式为：v-bind:标签属性=“绑定属性”。

**使用示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <input type="text" v-bind:placeholder="tips">
        <hr>
        <img :src="photo" style="width: 100px;">
    </div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            tips: "请输入用户名",
            photo: "https://v2.cn.vuejs.org/images/logo.svg",
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_17-21-22.png">

v-bind 也支持 JavaScript 表达式的运算，例如：

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <!-- :title 里面的非绑定值要记得加''，否则也会去data里找 -->
        <div :title="'list-' + id">div</div>
    </div>
</body>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            id: 2
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_17-46-23.png">

### 事件绑定指令

> vue 提供了 v-on 事件绑定指令，用来辅助程序员为 DOM 元素绑定事件监听。

#### v-on:	/ @

> v-on: 为标签绑定 Vue 对象的 methods 中的某个方法，语法格式为：v-on:事件名称=“事件处理函数名称”，v-on: 可以被简写为 @

**使用示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <p>count : {{count}}</p>
        <button v-on:click="add()">+1</button>
        <button @click="sub()">-1</button>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            count: 0,
            base: 2,
        },
        // vue 固定格式, 定义事件的处理函数
        methods: {
            add(){
                // this 指的就是 Vue 对象
                this.count += 1;
            },
            sub(){
                this.count -= 1;
            }
        },
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_18-18-26.png">

[^注]: 如果要绑定输入、键盘等事件，可以参考原生 DOM 对象的事件，如 onclick、oninput、onkeyup，这些事件替换为 vue 事件则为：v-on:click（@click）、v-on:input（@input）、v-on:keyup（@keyup）



#### $event

> vue 提供的原生事件对象（每个事件都有自己的对象），使用场景这里需要进行一下解释：首先，通过如 @click=“add” 这样不带括号调用方法时，可以在方法参数中接收事件对象，但是如果通过如 @click=“add()” 这样带括号调用方法时，方法就没法接收到事件对象了，因此为了接收到事件对象，vue 提供了一个指令 $event，这个指令表示的就是原生事件对象。

**使用场景**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <p>count : {{count}}</p>
        <button v-on:click="add">+1</button>
        <button @click="sub(1, $event)">-1</button>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            count: 0,
            base: 2,
        },
        // vue 固定格式, 定义事件的处理函数
        methods: {
            add(e){
                console.log(e);
                this.count += 1;
            },
            sub(n, e){
                console.log(n, e);
                this.count -= 1;
            }
        },
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/20/Snipaste_2022-12-20_18-47-54.png">



#### 事件修饰符

> 事件修饰符是 Vue 提供的一种对事件触发进行控制的指令，比如 a 标签的 href 跳转控制，比如阻止冒泡事件的发生（子元素事件发生逐级导致父级事件发生）。事件修饰符一般加在指令后面就可生效。
>
> 语法格式为：@事件.事件修饰符=“方法”

**常见的事件修饰符**

| 事件修饰符 |                             说明                             |
| :--------: | :----------------------------------------------------------: |
|  .prevent  | 阻止默认行为（例如：阻止 a 连接的跳转、阻止表单的提交@submit.prevent等） |
|   .stop    |                         阻止事件冒泡                         |
|  .capture  |               以捕获模式触发当前的事件处理函数               |
|   .once    |                     绑定的事件只触发一次                     |
|   .self    |    只有在 event.target 是当前元素自身时才触发事件处理函数    |

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <!-- @click.prevent 阻止原a标签的跳转行为 -->
        <a href="http://www.baidu.com" @click.prevent="click">百度</a>
        <hr>
        <!-- @click.stop 阻止事件冒泡到父级元素 -->
        <div @click="divHandler">
            <button @click.stop="btnHandler">按钮</button>
        </div>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        methods: {
            click(){
                console.log('百度');
            },
            divHandler(){
                console.log('divHandler');
            },
            btnHandler(){
                console.log('btnHandler');
            }
        },
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_10-06-16.png">

#### 按键修饰符

> Vue 提供的用于监听按键触发事件的指令，按键修饰符同样是事件绑定指令。
>
> 语法格式为：@键盘事件.键盘按键=“方法”

**常见按键监听及按键事件**

|      指令      |        作用         |
| :------------: | :-----------------: |
|   @keyup.esc   |  监听esc按键的弹起  |
|  @keydown.esc  |  监听esc按键的按下  |
|  @keyup.enter  | 监听enter按键的弹起 |
| @keydown.enter | 监听enter按键的按下 |

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <input type="text" @keyup.esc="close" @keyup.enter="submit">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data:{},
        methods: {
            close(e) {
                e.target.value = ""
            },
            submit(e){
                console.log("表单提交");
                e.target.value = "";
            }
        },
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_14-34-43.png">

### 双向绑定指令

> vue 提供了 v-model 双向数据绑定指令，用来辅助开发者在不操作 DOM 的前提下，快速获取表单的数据。

#### v-model

> 语法格式：v-model=“属性”

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <p>用户名字是: {{username}}</p>
        <input type="text" v-model="username">
        <select v-model="city">
            <option value="">请选择城市</option>
            <option value="1">广州</option>
            <option value="2">上海</option>
            <option value="3">北京</option>
        </select>
    </div>
</body>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            username: "张三",
            city: '',
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_14-59-38.png">

[^注]: v-model 指令一般和具有用户输入属性的标签一起使用，比如 input 输入框、textarea、select

##### 修饰符

v-model 指令有一些独特的**修饰符**，可以帮助 v-model 指令在不同场景下使用，常用的指令有：

| 修饰符  |                     作用                     |          示例          |
| :-----: | :------------------------------------------: | :--------------------: |
| .number |        自动将用户的输入值转为数值类型        | v-model.number=“count” |
|  .trim  |        自动过滤用户输入的首位空白字符        |   v-model.trim=“msg”   |
|  .lazy  | 双向同步数据时只同步最终状态，而不是随时更新 |   v-model.lazy=“msg”   |

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <!-- v-model.number 将输入值转为 number -->
        <input type="text" v-model.number="n1"> + 
        <input type="text" v-model.number="n2"> = {{ n1 + n2}}
        <hr>
        <!-- v-model.trim 去除首尾空格 -->
        <input type="text" v-model.trim="msg">
        <button @click="getMsg">获取用户名</button>
        <hr>
        <!-- v-model.lazy 让双向绑定数据作最终状态同步,不实时更新,提高效率 -->
        <input type="text" v-model.lazy="msg">
    </div>
</body>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            n1: 1,
            n2: 2,
            msg: ""
        },
        methods: {
            getMsg(){
                console.log(`用户名:${this.msg}`);
            }
        },
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_15-23-07.png">

### 条件渲染指令

> 条件渲染指令可以用来对 DOM 元素的显示和隐藏进行操作，常见的条件渲染指令有两个：v-if 和 v-show

#### v-if

> 当目标属性为 true 时才展示元素，否则动态移除元素。当初始状态属性为 false 时，并且以后也很可能不需要展示时，使用 v-if 的性能相对 v-model 更好
>
> 语法格式：v-if=“isTrue”

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <p v-if="flag">v-if 指令</p>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            flag: true, 
        }
    })
</script>
```



#### v-show

> 当目标属性为 true 时才展示元素，否则隐藏元素（为元素添加 display: none 样式），如果显示隐藏比较频繁的话，v-show 的性能好于 v-if 
>
> 目标语法：v-show=“isTrue”

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <p v-show="flag">v-show 指令</p>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            flag: true, 
        }
    })
</script>
```



#### v-else / v-else-if

> v-else 和 v-else-if 都是 v-if 的配套指令，这两个标签都需要和 v-if 配合使用，逻辑上就等于 else 和 else if。

**用法示例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <div v-if="type === 'A'">优秀</div>
        <div v-else-if="type === 'B'">良好</div>
        <div v-else-if="type === 'C'">一般</div>
        <div v-else>差</div>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            type: 'A'
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_15-57-41.png">

### 列表渲染指令

> vue 提供了 v-for 指令用于渲染结构相同的一组列表数据，语法格式为：v-for=“item in items”。

#### v-for

> v-for 可以用于获取数组中的每个数据，它还支持一个可选择的第二个参数，即当前的数据的索引，语法格式为：v-for=“(item, index) in items”

**使用举例**

```html
<body>
    <!-- CSS only -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet"
        crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <ul>
            <table class="table table-bordered table-hover table-striped">
                <thead>
                    <th>索引</th>
                    <th>ID</th>
                    <th>姓名</th>
                </thead>
                <tbody>
                    <!-- v-for 使用到 index 索引, v-for 的元素本级也可以使用 -->
                    <!-- 官方建议: 只要用到了 v-for 指令，那么一定要绑定一个 :key 属性(如果不绑定,在.vue文件中会报错),并且尽量把当前循环数据的id作为key的值,其他值也可以,如字符串或者数字类型,但是必须唯一,同时不建议用索引作key,因为索引并不和数据牢牢绑定 -->
                    <tr v-for="(item,index) in items" :title="item.name" :key="item.id">
                        <td>{{ index }}</td>
                        <td>{{ item.id }}</td>
                        <td>{{ item.name }}</td>
                    </tr>
                </tbody>
            </table>
        </ul>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            items:[
                {id: 1, name: "张三"},
                {id: 2, name: "李四"},
                {id: 3, name: "王五"},
            ]
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/Snipaste_2022-12-22_16-17-55.png">

## 4、过滤器（vue2）

> 过滤器（Filters）是 vue 为开发者提供的功能，常用于文本的格式化，过滤器可以用在两个地方：**插值表达式**和 **v-bind 属性绑定**。过滤器的用法是添加在 JavaScript 表达式的尾部，由 “**管道符**” 进行调用。需要注意的是，过滤器功能在 vue3 被完全废弃了，该功能只能在 vue2 中使用。

### 私有过滤器

> 作用域为本 vue 对象，写法为直接写在 vue 对象内部的 filters 节点下。

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        <!-- | 表示管道符, 后面的参数 capitalize 表示过滤函数,将 message 作为参数传入 capitalize 函数中,并将该函数的返回值作为最终内容 -->
        <p>{{ message | capitalize }}</p>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            message: 'abc',
        },
        // 固定语法, 过滤器函数必须定义在 filters 节点下, val 表示管道符前面的值,过滤函数必须要有返回值
        filters:{
            // 第一个参数规定为传递过来的需要过滤的参数
            capitalize(val){
                return val.toUpperCase();
            }
        }
    })
</script>
```

[^注]: 写在 vue 对象内部的过滤器被称为私有过滤器，作用域为本 vue 对象。需要注意的是，过滤器可以连续的调用，在前一个过滤器返回值的基础上再继续过滤，格式是在后面追加（ 管道符 过滤函数），如 {{ message | capitalize | merge}}，过滤器函数是一个 JS 函数，所以可以传递参数，当然接受时第一个参数仍为需要过滤的参数，后面可以再加传递过来的参数。



### 全局过滤器

> 过滤器基本的作用域是一个 vue 对象，跨 vue 对象无法共享过滤器，如果想在多个 vue 对象中共享过滤器，则可以考虑使用全局过滤器。全局过滤器定义在 vue 对象之外，一般在使用过滤器时都是使用全局过滤器，很少使用私有过滤器。

**使用举例**

```html
<script>
    const vm1 = new Vue({...});
	// 全局过滤器 - 独立于每个 Vue 对象之外
    // Vue.filter()方法接收两个参数,第一个参数是全局过滤器的名字,第二个参数是全局过滤器的处理函数
    Vue.filter('capitalize', (str) => {
        return str.toUpperCase();
    })
</script>
```

[^注]: 当同时存在私有过滤器和全局过滤器时，vue 对象会优先使用自己的私有过滤器。并且全局过滤器最好写在最前面，以免出错。



## 5、侦听器

> 侦听器是 vue 提供的一项功能，通过它可以监视 data 中数据的变化，再根据数据的变化执行特定的操作。侦听器有两种格式写法：方法格式，对象格式。

### 方法格式

> 方法格式的侦听器写法。好处是方便，缺点是无法在刚进入页面时就自动触发侦听函数。

侦听器的应用场景有：**实时处理用户的输入数据**。

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.2/jquery.min.js"></script>
    <div id="app">
        <input type="text" v-model="message">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            message: '',
        },
        // watch 节点,用于创建监听属性的方法,所有侦听器都应该被定义在 watch 节点下
        watch: {
            // 需要监听哪个数据,就把数据名作为方法名即可
            // 方法中可传参也可不传,传入参数时第一个是变化后的新值,第二个是变化前的旧值
            message(newVal, oldVal){
                if(newVal == '')return;
                $.get('https://www.escook.cn/api/finduser/' + newVal, (result)=>{
                    console.log(result);
                })
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_10-04-47.png">



### 对象格式

> 对象格式的侦听器写法。好处是可以通过设置 immediate 让每次刚进入页面时自动触发侦听器，并且可以通过设置 deep 监听到对象内部的属性。

**使用举例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.2/jquery.min.js"></script>
    <div id="app">
        <input type="text" v-model="user.name">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            user: {
                name: '张三'
            },
        },
        // watch 节点,用于创建监听属性的方法,所有侦听器都应该被定义在 watch 节点下
        watch: {
            // 需要监听哪个数据,就把数据名作为对象名即可 [对象侦听器写法]
            user: {
                // 每次数据发生变化就会调用 handler 方法
                handler(newVal) {
                    console.log(newVal);
                    if (newVal == '') {
                        console.log("用户名不能为空！");
                        return;
                    };
                    $.get('https://www.escook.cn/api/finduser/' + newVal, (result) => {
                        console.log(result);
                    })
                },
                // immediate 属性, 默认是false, 当需要页面一加载就执行监听函数时可以设置为 true, 否则第一次加载页面时,页面数据没变化,不会调用监听函数
                immediate: true,
                // deep 属性, 默认是false,当需要监听的属性是对象时，并且要监听对象内部属性的变化时，设置deep属性为true
                deep: true,
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_10-36-02.png">



#### immediate 属性

> 在对象格式的侦听器中使用，作用是在首次加载页面时就执行属性绑定的侦听器方法，默认值是 false。

**使用案例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.2/jquery.min.js"></script>
    <div id="app">
        <input type="text" v-model="name">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            name: '张三'
        },
        // watch 节点,用于创建监听属性的方法,所有侦听器都应该被定义在 watch 节点下
        watch: {
            // 需要监听哪个数据,就把数据名作为方法名即可 [对象侦听器写法]
            name: {
                // 每次数据发生变化就会调用 handler 方法
                handler(newVal, oldVal) {
                    if (newVal == '') {
                        console.log("用户名不能为空！");
                        return;
                    };
                    $.get('https://www.escook.cn/api/finduser/' + newVal, (result) => {
                        console.log(result);
                    })
                },
                // immediate 属性, 默认是false, 当需要页面一加载就执行监听函数时可以设置为 true, 否则第一次加载页面时,页面数据没变化,不会调用监听函数
                immediate: true,
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_10-47-30.png">

[^注]: 侦听器默认只在监听的数据发生变化时，才调用监听函数，第一次进入页面或者刷新时数据为初始化状态，没有变化，不会调用监听函数，如果需要在第一次进入页面时就执行监听函数，可以将使用对象格式，将 immediate 的属性设置为 true（默认是 false）。



#### deep 属性

> 在对象格式的侦听器中使用，作用是允许侦听对象中的属性变化（默认情况下监听对象，不会监听对象中属性变化），

**使用案例**

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.2/jquery.min.js"></script>
    <div id="app">
        <input type="text" v-model="user.name">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            user: {
                name: '张三'
            },
        },
        // watch 节点,用于创建监听属性的方法,所有侦听器都应该被定义在 watch 节点下
        watch: {
            // 需要监听哪个数据,就把数据名作为方法名即可 [对象侦听器写法]
            user: {
                // 每次数据发生变化就会调用 handler 方法
                handler(newVal) {
                    console.log(newVal);
                },
                // deep 属性, 默认是false,当需要监听的属性是对象时，并且要监听对象内部属性的变化时，设置deep属性为true
                deep: true,
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_10-51-54.png">

使用以上方式获取到的是一个 Object 对象，里面有监听到的对象中变化的属性，如果想**直接获取到变化的属性**，而不是对象，可以采用以下写法：

```html
<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.2/jquery.min.js"></script>
    <div id="app">
        <input type="text" v-model="user.name">
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            user: {
                name: '张三'
            },
        },
        // watch 节点,用于创建监听属性的方法,所有侦听器都应该被定义在 watch 节点下
        watch: {
            // 用 '' 把要监听的对象中的属性包住,传入的值就是变化后的属性值,当然还可以再传入一个oldVal
            'user.name'(newVal){
                console.log(newVal);
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_10-55-22.png">



## 6、计算属性

> 计算属性是 Vue 提供的一种特殊属性，它定义在 vue 对象的 computed 节点下，可以用来做一些公共的操作，返回的值会作为 Vue 的属性。计算属性可以做到很好的代码复用。

**使用案例**

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 200px;
            height: 200px;
        }
    </style>
</head>

<body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
    <div id="app">
        R: <input type="text" v-model="r">
        G: <input type="text" v-model="g">
        B: <input type="text" v-model="b">
        <hr>
        <div class="box" :style="{background: rgb}">
            <!-- 直接调用 -->
            {{ rgb }}
        </div>
    </div>
</body>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            r: '0',
            g: '0',
            b: '0'
        },
        // Vue 提供的用来做公共操作的方法节点,里面的方法名字和返回值会被作为 Vue 的属性
        computed:{
            rgb(){
                // 返回一个 rgb 字符串
                return `rgb(${this.r},${this.g},${this.b})`;
            }
        }
    })
</script>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_13-37-34.png">



## 7、vue-cli

> vue-cli 是 Vue.js 开发的标准工具，它简化了程序员基于 webpack 创建工程化的 Vue 项目的过程，它可以让程序员更专注在应用上，而不是 webpack 配置。

### 1、安装和使用

vue-cli 是 npm 上的一个全局包，使用 npm 命令下载： 

```
npm install -g @vue/cli 
```

基于 vue-cli 快速生成工程化的 Vue 项目：

```
vue create 项目名称
```

之后的选择按照如下进行选择：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-50-05.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-53-08.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-53-58.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-54-59.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-55-59.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_14-56-53.png">

### 2、目录构成

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_15-18-48.png" style="zoom:50%;" >

### 3、vue运行流程

> 在工程化的项目中，vue 要做的事情就是通过 **main.js** 把 **App.vue** 渲染到 **index.html** 的指定区域中。

**拿main.js举例**

```js
// 导入 Vue 构造函数
import Vue from 'vue'
// 导入 App.vue 文件
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  // el: '#app',
  // 调用render函数指定App.vue文件中的内容替换掉index.html中id为app的组件内容
  render: h => h(App),
  // .$mount('#app) 作用和 el:'#app'一样, 都是指定关联的容器id
}).$mount('#app')

```



## 8、组件

> vue 中规定，组件的后缀名是 .vue。每个 .vue 组件都由三部分构成，分别是：**template**（组件的模板结构）、**script**（组件的 JavaScript 行为）、**style**（组件的样式）。

**使用举例**

```vue
<template>
  <div class="box-style">
    <h3>用户名: {{ username }}</h3>
  </div>
</template>

<script>
// 默认导出, 这是vue规定的固定写法
export default {
  // data数据源
  // 需要注意的是: vue 规定了 data 数据必须是一个函数, 否则会报错
  data() {
    // return 中的 {} 里面可以定义vue的数据
    return {
      username: "李四",
    };
  },
};
</script>

<style>
.box-style {
  background-color: rgb(222, 253, 255);
}
</style>
```

