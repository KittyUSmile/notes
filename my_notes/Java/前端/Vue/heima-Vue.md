
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

#### 1、默认打包约定

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



#### 2、自启动配置

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

或者直接在 package.json 中设置 serve 脚本的执行参数：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/14/Snipaste_2023-01-14_13-56-57.png">

#### 3、源码错误行数定位配置

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



#### 4、别名配置

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


# 二、eslint

> eslint 是一套代码规范，如果项目中引入了 eslint，当代码不合规时就会报错。

当通过 vue-cli  生成的项目中引入了 eslint，可以先查看文件中的 eslintrc.js，这个文件是 eslint 的配置文件，其中的 rules 是 eslint 的规则：
<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/29/Snipaste_2022-12-29_10-49-00.png">

如果想要自定义更多的规则，可以去 [eslint 中文官网](http://eslint.cn/docs/rules/)进行查阅。


# 三、Vue

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

> v-bind: 用于标签属性和 Vue 对象的 data 中的属性的单向绑定，:属性="" 的 "" 中可以写一些简单的 JS 表达式。
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

> v-on: 为标签绑定 Vue 对象的 methods 中的某个方法，语法格式为：v-on:事件名称=“事件处理函数名称”，v-on: 可以被简写为 @。其中 @事件名称="" 的 "" 中可以直接写简单的 JS 表达式，不只能调用方法。

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

### 自定义指令
> vue 允许开发者自定义指令，自定义指令分为 **私有自定义指令** 和 **全局自定义指令**。

#### 私有自定义指令
> 私有自定义指令定义在 vue 对象的 directives 节点下，只能本组件使用。

使用举例：

```js
<template>
  <div id="app">
    <p v-color="color">红色</p>
    <button @click="color = 'green'">变绿</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      color: 'blue'
    };
  },
  directives: {
    // 自定义指令命名，不需要加v-
    color: {
      // 指令绑定操作, 第一次使用自定义指令时会自动调用bind函数, el是此指令绑定的DOM元素, 第二个参数binding是传递进来的变量
      bind(el, binding) {
        el.style.color = binding.value;
      },
      // 指令绑定的 DOM 元素更新时调用 update() 方法
      update(el, binding){
        el.style.color = binding.value;
      }
    },
  }
};
</script>

<style lang="less" scoped>
</style>

```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/29/Snipaste_2022-12-29_10-02-21.png">
如果bind和update的函数逻辑一样，则对象格式的自定义指令可以简写成函数格式：
```js
directives:{
	//在 bind 和 update 时，触发相同的业务逻辑
	color(el, binding){
		el.style.color = binding.value
	}
}
```


#### 全局自定义指令
> 全局自定义指令在 main.js 中定义，通过 Vue.directive() 进行声明，以后如果要使用都是使用全局自定义指令，一般不用私有自定义指令。

举例如下：
```js
import Vue from 'vue'
import App from './App.vue'

// Vue 启动在终端上的提示信息, 默认是 true
Vue.config.productionTip = false

Vue.directive('color', {
  bind(el, binding){
    el.style.color = binding.value
  },
  update(el, binding){
    el.style.color = binding.value
  },
})

new Vue({
  render: h => h(App),
}).$mount('#app')

```

当 bind 和 update 方法逻辑一致时，可以简化为函数格式：
```js
Vue.directive('color', (el, binding) => {
  el.style.color = binding.value
})
```

## 4、过滤器（vue2）

> 过滤器（Filters）是 vue 为开发者提供的功能，可以理解为在后面加个方法处理数据，常用于文本的格式化，过滤器可以用在两个地方：**插值表达式**和 **v-bind 属性绑定**。过滤器的用法是添加在 JavaScript 表达式的尾部，由 “**管道符**” 进行调用。需要注意的是，过滤器功能在 vue3 被完全废弃了，该功能只能在 vue2 中使用。

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

> 侦听器 watch 是 vue 提供的一项功能，通过它可以监视 data 中数据的变化，再根据数据的变化执行特定的操作。侦听器有两种格式写法：方法格式，对象格式。

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

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/26/Snipaste_2022-12-26_09-46-03.png">



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

> 计算属性 computed 是 Vue 提供的一种特殊属性，它定义在 vue 对象的 computed 节点下，可以用来做一些公共的操作，返回的值会作为 Vue 的属性。计算属性可以做到很好的代码复用。

**计算属性的语法格式为（HTML格式）**：

```vue
<script>
    const vm = new Vue({
        el: '#app',
        data: {},
        // Vue 提供的用来做公共操作的方法节点,里面的方法名字和返回值会被作为 Vue 的属性
        computed:{
            method1(){
                return 'result';
            }
        }
    })
</script>
```

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
  // 调用render函数指定App.vue文件中的内容替换掉index.html中id为app的组件内容,render函数中渲染哪个vue文件哪个就叫根组件。
  render: h => h(App),
  // .$mount('#app) 作用和 el:'#app'一样, 都是指定关联的容器id
}).$mount('#app')

```



## 8、组件

> vue 中规定，组件的后缀名是 .vue。组件除 App.vue 外一般都放在 components 文件夹下。

### 1、组件结构

> 每个 .vue 组件都由三部分构成，分别是：**template**（组件的模板结构）、**script**（组件的 JavaScript 行为）、**style**（组件的样式）。

**组件结构使用的格式为**

```vue
<template>
  
</template>

<script>
export default {
	data(){
        return{}
    }
}
</script>

<style>

</style>
```

**使用举例**

```vue
<template>
  <div class="box-style">
    <h3>用户名: {{ username }}</h3>
    <button @click="changeName">修改用户名</button>
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
  // 方法仍写在methods
  methods:{
    changeName(){
      this.username = '张三'
    }
  },
  // 侦听器
  watch:{},
  // 当前组件的计算属性
  computed:{},
  // vue2过滤器
  filters:{},
};
</script>

<!-- 样式设置为less方式 -->
<style lang="less">
.box-style {
  background-color: rgb(222, 253, 255);
}
</style>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_16-30-13.png">

### 2、组件引入

> 各组件之间默认没有关系，但是可以通过引入组件的方式构造出父组件和子组件。组件引入有两种方式，分别可以获得私有组件和全局组件

#### 私有组件

> 引入后只能在本组件中使用，其它组件无法使用。

**引入举例**

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- 第三步: 以标签形式使用刚才注册的组件 -->
    <HelloWorld></HelloWorld>
  </div>
</template>

<script>
// 引入组件第一步: 使用 import 语法导入需要的组件
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  // vue 提供的节点，用来声明引入的组件。第二步: 使用 import 语法导入需要的组件
  components: {
    //导入格式为: '命名': 组件名  如果命名和组件名一致可以直接写组件名即可
    HelloWorld
  }
}
</script>

<style lang="less">

</style>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_18-23-56.png">



引入组件时手写可能会写错，可以安装一个 vscode 插件：**Path Autocomplete**

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/23/Snipaste_2022-12-23_18-26-32.png">

安装后在 vscode 的 设置 -> 应用程序 -> 设置同步 中找到 settings.json，在最外层 {} 里面的头部加入以下配置：

```json
//导入文件时是否携带文件的扩展名
"path-autocomplete.extensionOnImport": true,
//配置 @ 的路径提示
"path-autocomplete.pathMappings": {
	"@":"${folder}/src"
},
```

[^注]: 如果插件没有生效，首先重启，然后查看 vscode 工作区是否只有我们目前正在使用的那个项目，如果不是，重新只打开那个项目即可。



#### 全局组件

> **所有组件中都能使用的全局组件**，可以通过 vue 项目的 **main.js** 入口文件，通过 **Vue.component()** 方法进行全局注册，示例代码如下：

```js
import Vue from 'vue'
import App from './App.vue'
// 导入组件
import Count from '@/components/Count.vue'

// 声明组件, 参数一为字符串格式,表示组件的注册名称, 参数二为需要被注册的组件
// 如果Count组件有声明name属性, 那么下面的注册名称可以直接写为 Count.name
Vue.component('MyCount', Count)

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')

```



### 3、生命周期

> 生命周期（Lift Cycle）是指一个组件从 **创建 -> 运行 -> 销毁** 的整个阶段，在组件不同生命周期阶段，都可以指定生命周期函数，生命周期函数会在组件到达某个生命周期阶段时自动执行。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/26/20200515103424403.png">

**各周期函数使用举例**
```js
<script>
export default {
  data() {
    return {};
  },
  
  // Vue 创建阶段的四个函数
  // 在刚初始化完vue对象后调用, 此时data或者其它数据或方法都还未加载, 该方法使用频率非常少
  beforeCreate(){},
  // data 和 methods 等都初始化完毕后调用, 可以用于数据的初始化, 但此时页面结构还未生成, 不能操作 DOM, 该方法使用频率最高
  created(){},
  // 基于数据和模板在内存中编译生成好模板后, 调用该方法, 此时模板还未渲染到浏览器中, 该方法使用频率非常少
  beforeMount(){},
  // 内存中编译生成的模板渲染到浏览器中后, 调用该方法, 此时浏览器中已包含当前组件的 DOM 结构了,可以开始操作DOM了, 该方法使用频率较高
  mounted(){},


  // Vue 运行阶段的两个函数
  // 将要重新渲染模板结构中的数据时（如data中数据变化）, 调用该函数, 需要注意, 此时页面上的数据是旧的, 而data中的数据是新的, 该方法使用频率较低
  beforeUpdate(){},
  // 已经将新数据渲染到页面上后, 调用该函数, 此时无论是页面还是 data, 数据都是最新的, 该方法使用频率较高
  updated(){},


  // Vue 销毁阶段的两个函数 [使用均较少]
  // 在组件即将被销毁前调用执行, 此时组件还未被销毁, 还处于正常工作状态
  beforeDestroy(){},
  // 组件已经被销毁后调用
  destroyed(){},
};
</script>
```

### 4、数据共享
> 谈组件之间的数据共享前先谈谈组件之间的关系，组件之间可以笼统地看作只有**父子关系**和**兄弟关系**。其中父向子数据共享可以通过**自定义属性**，即 props 进行数据共享，子向父共享数据可以使用**自定义事件**；兄弟组件之间的数据共享在 vue2 中使用的是 **EventBus 方案**

#### 父子关系
父向子数据共享举例 [ 通过 props ]：
```js
// 父组件
<Son :msg="message" :user='userInfo'></Son>

data(){
	return{
		message: 'hello vue',
		userInfo: { name: 'zs', age: 20},
	}
}
```

```js
// 子组件
<template>
	<div>
		<p>父组件传递的 msg 值: {{ msg }}</p>
		<p>父组件传递的 user 值: {{ user }}</p>
	</div>
</template>

props:['msg', 'user']
```

子向父数据共享举例 [ 通过自定义事件 ]：
```js
// 子组件
export default{
	data(){
		return { count: 0 }
	},
	methods:{
		add(){
			this.count += 1
			// this.$emit('自定义事件名称', 事件传递的参数) 表示触发一个自定义事件
			this.$emit('numchange', this.count)
		}
	}
}
```

```js
// 父组件
// 自定义事件被触发时调用getNewCount方法
<Son @numchange='getNewCount'></Son>

export default{
	data(){
		return { countFromSon: 0 }
	},
	methods:{
		getNewCount(val){
			this.countFromSon = val
		}
	}
}
```

#### 兄弟关系
> 兄弟组件之间通过 EventBus 共享数据，EventBus其实就是一个普通的Vue对象，作为传输数据的桥梁。

样例：

eventBus.js：
```js
import Vue from 'vue'

//向外共享 Vue 的实例对象
export default new Vue()

// eventBus.js
```

数据发送方：
```js
import bus from './eventBus.js'

export default {
	data(){
		return {
			msg: 'hello vue.js'
		}
	},
	methods:{
		sendMsg(){
			// 触发自定义事件 share, 并将msg作为参数进行传递
			bus.$emit('share', this.msg)
		}
	}
}
```

数据接收方：
```js
import bus from './eventBus.js'

export default {
	data() {
		return {
			msgFromLeft: ''
		}
	},
	created(){
		// 为 bus 绑定好事件 share 及触发函数
		bus.$on('share', val => {
			this.msgFromLeft = val
		})
	}
}
```

### 5、动态组件
> 动态组件其实就是指让组件能够动态显示或者隐藏。

Vue 内置提供了一个占位符组件\<component>，可以通过设置该组件的 is 属性，来绑定要渲染的组件，使用如：

```js
<template>
  <div id="app">
  
    <component :is="comName"></component>
  </div>
</template>

<script>
import Left from '@/components/Left.vue'
import Right from '@/components/Right.vue'

export default {
  name: 'App',
  components:{
    Left, Right
  },
  data(){
    return{
      comName:'Left',
    }
  }
}
</script>
```

#### 1、keep-alive
> 默认情况下使用动态组件隐藏组件时，组件会被销毁，如果想要组件一直活着，可以用 keep-alive 标签把 component 组件包起来

```js
<keep-alive>
	<component :is="comName"></component>
</keep-alive>
```

如果只希望部分组件被缓存，可以采用 keep-alive 标签的 include 属性和 exclude 属性进行设置，但是需要注意，这两个属性同时只能使用一个。

```js
<!-- 通过设置 include 属性指定哪些组件能被缓存 -->
<keep-alive include="Left,Right">
	<component :is="comName"></component>
</keep-alive>
```

keep-alive 同样有两个生命周期函数：activated（组件被激活时执行）和 deactivated（组件被缓存时执行）

```js
<script>

export default {
  data() {
    return {};
  },
  activated() {},
  deactivated() {},
};
</script>
```

## 9、props

> vue 提供的自定义属性配置，程序员可以在使用组件时通过自定义组件的某些属性来达到一些自定义的效果，很好的提高组件的复用性。

### 数组格式

> props 属性的两种定义方式之一，以数组格式定义 props 属性，好处是简洁，缺点是无法配置这些自定义属性的属性。

**数组格式 props 语法如下**：

```vue
<script>
export default {
  // 组件的自定义属性, props中的数据可以直接在模板结构中被使用。
  // 需要注意,props中的属性值不允许被更改,它是只读的,否则会在控制台报错,如果有操作props中属性的需求,可以将其赋给data中的变量再进行操作
  props: ['initA', 'initB','initC'],
  
  // 组件的私有属性
  data() {
    return {};
  },
};
</script>
```



### 对象格式

> props 属性的两种定义方式之一，以对象格式定义 props 属性，允许定义 props 属性的同时进行一些配置。

```vue
<script>
export default {
  props: {
    // 对象格式自定义属性的格式: 
    //    自定义属性A: { /* 配置选项 */},
    //    自定义属性B: { /* 配置选项 */},
    init:{
      //当外界使用到 init 属性, 且该init属性没有被赋予值时, default值生效
      default: 0,
      //定义该属性接收的类型,如果传递过来的值不符合此类型,则会在终端报错
      type: Number,
      //设置必传参为true,即只要使用到该组件,都必须传入该属性值,否则在终端报错
      required: true,
    }
  },
  data() {
    return {};
  },
};
</script>
```

**使用举例**

**子组件 Count.vue**
```html
<template>
  <div>
    <h5>{{ data }}</h5>
    <button @click="data += 1">+ 1</button>
  </div>
</template>

<script>
export default {
  props: ["init"],
  data() {
    return {
      // init自定义属性也属于Vue对象的属性,所以也能被this出来
      data: this.init,
    };
  },
};
</script>

<style lang="less">
</style>
```

**父组件 App.vue**

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- 第三步: 以标签形式使用刚才注册的组件, 可以设置在子组件中自定义的属性 --> 
    <!-- 设置 init 属性值为 6 -->
    <MyCount init="6"></MyCount>
  </div>
</template>

<script>
// 引入组件第一步: 使用 import 语法导入需要的组件
import Count from '@/components/Count.vue'


export default {
  name: 'App',
  // vue 提供的节点，用来声明引入的组件。第二步: 使用 import 语法导入需要的组件
  components: {
    //导入格式为: '命名': 组件名  如果命名和组件名一致可以直接写组件名即可
    Count
  }
}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/26/Snipaste_2022-12-26_09-37-40.png">

[^注]: 传入 props 属性的值默认是字符串类型，如果想传入数值类型的值，在设置的属性前加上冒号即可，表示 v-bind 绑定，v-bind 中写的是 JS 代码，写个数字表示就是数值类型，如 :init=“9”，表示传入的是数字9，而不是字符串 “9”。

## 10、ref
> ref 是 vue 提供的一种引用属性，可以给标签设置唯一的 ref 属性，然后就可以在组件中直接获取到该 DOM 标签元素，或者用它给子组件设置唯一的 ref 属性，也可以在本组件中获取到该子组件的引用。可以说 ref 可以满足我们对 DOM 操作的需要。

### 1、获取DOM
> 在 vue 项目里，不推荐使用 jQuery，如果需要获取页面的 DOM 元素，可以使用 vue 提供的 ref。想通过 ref 获取 DOM 元素，需要先给 DOM 元素标签设置唯一的 ref 属性，然后通过this.$refs.属性获取DOM元素。

使用举例：
```js
<template>
	<div>
		<h1 ref="myh1_1">h1</h1>
		<h2 ref="myh2_1">h2</h2>
		<button @click="show">按钮</button>
	</div>
</template>

<script>
export default{
	methods:{
		show(){
			this.$refs.myh1_1.style.color = 'red'
			this.$refs.myh2_1.style.color = 'green'
		}
	}
}
</script>
```

### 2、获取子组件
> ref 同样可以给子组件设置，并通过 this.$refs.属性 获取。

使用举例：
```js
<template>
  <div id="app">
    <Test2 ref="test2"></Test2>
    <button @click="getSonRef">test2</button>
  </div>
</template>

<script>


export default {
  data(){
    return{
      sonVal: '',
    }
  }
  methods:{
    getSonRef(){
      this.sonVal = this.$refs.test2.msg;
    }
  },
}
</script>
```

## 11、插槽
> 插槽（Slot）可以理解为一块预留区域，留给开发者插入合适的代码。

用两个组件展示一下插槽最基本的用法：

App.vue：
```js
<template>
  <div id="app">
	  <!-- 第三方 Left 组件 -->
      <Left>
        <!-- 在组件中添加的内容会自动加到插槽中 -->
        <p>我在插槽里插入一段诗句</p>
      </Left>
  </div>
</template>
```

Left.vue：
```js
<template>
  <div id="left">
    <!-- 设置一块插槽区域 -->
    <slot></slot>
  </div>
</template>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/28/Snipaste_2022-12-28_16-51-16.png">

### v-slot: 
需要注意的是，vue 官方规定了每一个 slot 插槽都要有一个 name 名称属性，如果省略了，那么就会有一个默认名称叫做 default，向组件中添加的内容都会被默认填充到名称为 default 的插槽中，如果要指定内容要渲染到某个插槽内，则需要用到 v-slot: ，v-slot: 可以简写成 #

```js
<template>
  <div id="app">
      <!-- 第三方 Left 组件 -->
      <Left>
        <!-- 
          1、把内容填充到指定名称的插槽中，需要使用 v-slot: 这个指令, 后面直接跟插槽的 name ,但是v-slot: 不能直接用在内容标签上, 它只能用在 template 标签上
         -->
        <template v-slot:zs>
          <!-- 在组件中的内容不知道加到哪里，就会加到插槽中 -->
          <p>我在插槽里插入一段内容</p>
        </template>
      </Left>
  </div>
</template>
```

[^注]: template 标签本质上是一个虚拟的标签，只起到包裹性质的作用，它不会被渲染为任何实质性的 html 元素。

### 默认内容
> 即官方所称的"后备内容"，当没有向插槽内指定内容时，可以设置插槽展示默认内容。

样例如下：

```js
<slot name="zs">
  <p>默认内容</p>
</slot>
```

### 具名插槽
> 具名插槽其实就是设置了 name 属性的插槽，具名只是一个叫法。

样例如下：

```js
<slot name="zs">

</slot>
```

### 作用域插槽
> 可以向调用者提供一些信息的插槽就是作用域插槽，调用者可以获取插槽属性定义的一些信息。

样例如下：

插槽：
```js
<!-- msg 命名可以随意 -->
<slot name="content" msg="hello"></slot>
```

调用者：
```js
<!-- scope是用来接收插槽传递上来数据的参数, 命名可以随意, 但一般都命名为 scope -->
<template #content="scope">
	<p>{{ scope.msg }}</p>
</template>
```

插槽传递数据时，可以传递对象、数组等数据，当然插槽传递数据时也可以动态绑定数据；调用者在接收时，会封装在接收参数中，如果只有一个接收参数，数据都会被封装进去，同时也可以使用解构赋值给多个参数。

## 12、路由
> 路由（router）其实就是一组哈希地址和组件之间的对应关系，不同的哈希地址展示不同的组件，哈希地址由 '#' 开头，如 '#/home'。

路由的工作方式为：首先由用户点击页面上的路由链接，让URL地址栏中的 Hash 地址发生变化，并由前端路由监听到 Hash 地址的变化，然后把当前的 Hash 地址对应的组件再渲染到浏览器中，这就是路由的工作方式。

### vue-router
> vue 项目中的路由管理包，它只能配置 vue 项目使用，可以轻松管理 SPA 项目（单页面项目）中组件的切换。

### 基本使用

如果要使用 vue-router 参与 vue 项目，首先需要安装 vue-router 包（注意版本兼容问题，vue2用以下的版本即可）：

```
npm i vue-router@3.5.2 -S
```

然后在 src 源代码目录下，新建 router/index.js 路由模块，初始化为如下代码：

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
// Vue.use()函数的作用就是用来安装插件的
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter()

// 4、向外共享路由的实例对象
export default router
```

把导出的路由挂载到 main.js 中：

```js
import Vue from 'vue'
import App from './App.vue'
// 在进行模块化导入时, 如果给定的是文件夹, 则默认导入这个文件夹下，名字叫做 index.js 的文件
import router from './router'

Vue.config.productionTip = false

new Vue({
  // 路由通过以下方式挂载
  // router: router 简写为 router
  router,
  render: h => h(App),
}).$mount('#app')

```

然后就可以通过定义路由规则和路由占位符来进行路由测试了：

App.vue：
```vue
<template>
  <div id="app">
    <h2>App.vue</h2>
    <!-- 当安装和配置好 vue-router后，就可以使用 router-link 来代替普通的 a 链接 -->
    <!-- <a href="#/home">首页</a> -->
    <router-link to="/home">首页</router-link>
    <router-link to="/movie">电影</router-link>
    <router-link to="/about">关于</router-link>
    <hr />
    <!-- vue-router提供的占位符组件 -->
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style lang="less">
</style>
```

router/index.js：

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

//导入需要的组件
import Home from '@/components/Home'
import Movie from '@/components/Movie'
import About from '@/components/About'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter({
  // routes 数组,定义 "hash地址" 与 "组件" 之间的对应关系 
  routes: [
    // path不需要加#号, 规定路由地址对应的组件
    { path: '/home', component: Home },
    { path: '/movie', component: Movie },
    { path: '/about', component: About },
  ]
})

// 4、向外共享路由的实例对象
export default router
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/29/Snipaste_2022-12-29_15-15-45.png">

#### 路由重定向
> 当用户访问某个地址时，程序员希望将其跳转到另一个地址，就需要使用到路由重定向 redirect。

使用举例：

router/index.js：

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

//导入需要的组件
import Home from '@/components/Home'
import Movie from '@/components/Movie'
import About from '@/components/About'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter({
  // routes 数组,定义 "hash地址" 与 "组件" 之间的对应关系 
  routes: [
    // 路由重定向，当访问到/地址时，跳转到'/home', '/home'又会跳转到 Home组件
    { path: '/', redirect: '/home' },
    { path: '/home', component: Home },
    { path: '/movie', component: Movie },
    { path: '/about', component: About },
  ]
})

// 4、向外共享路由的实例对象
export default router
```

#### 嵌套路由
> 嵌套路由是指某个组件是被路由渲染到浏览器中，这个组件里还有子级路由链接和路由组件，达成一种嵌套关系。

使用举例：

外层组件 About.vue：

```vue
<template>
  <div id="app-container">
    <router-link to="/about/tab1">tab1</router-link>
    <router-link to="/about/tab2">tab2</router-link>
    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
export default {};
</script>

<style lang="less" scoped>
#app-container {
  width: 1200px;
  height: 400px;
  background-color: rgb(255, 222, 246);
}
</style>
```

规则绑定 index.js：

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

//导入需要的组件
import Home from '@/components/Home'
import Movie from '@/components/Movie'
import About from '@/components/About'
import Tab1 from '@/components/tabs/Tab1.vue'
import Tab2 from '@/components/tabs/Tab2'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter({
  // routes 数组,定义 "hash地址" 与 "组件" 之间的对应关系 
  routes: [
    // 不需要加 # 号
    { path: '/', redirect: '/home' },
    { path: '/home', component: Home },
    { path: '/movie', component: Movie },
    {
      path: '/about',
      // 子级路由默认跳转方式1
      // redirect: '/about/tab1',
      component: About,
      // 嵌套路由通过 children 属性声明子级路由规则, 子路由不要加 / 
      children: [
         // 子级路由默认跳转方式2
	    { path: '', component: Tab1 },
        { path: 'tab1', component: Tab1 },
        { path: 'tab2', component: Tab2 },
      ]
    },
  ]
})

// 4、向外共享路由的实例对象
export default router
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/29/Snipaste_2022-12-29_15-45-26.png">

#### 动态路由
> 动态路由是指路由规则中的可以匹配动态传递过来的请求，并把它们对应到某个组件上，比如：{path: '/movie/:id', component: Movie}，其中 :id 表示动态接收请求，只要是路径是 /movie/xx，都可以匹配该规则。

使用举例：

```js
<template>
  <div id="app-container">
    <router-link to="/about/1">1</router-link>|
    <router-link to="/about/2">2</router-link>|
    <router-link to="/about/3">3</router-link>|
    <hr />
    <router-view></router-view>
  </div>
</template>
```

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

//导入需要的组件
import Home from '@/components/Home'
import Movie from '@/components/Movie'
import About from '@/components/About'
import Tab1 from '@/components/tabs/Tab1.vue'
import Tab2 from '@/components/tabs/Tab2'
import A1 from '@/components/numbers/1.vue'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About,
      // 嵌套路由通过 children 属性声明子级路由规则, 子路由不要加 / 
      children: [
        // :id 表示动态绑定路由, 凡是 /about/xx 都能匹配上
        { path: ':id', component: A1 },
      ]
    },
  ]
})

// 4、向外共享路由的实例对象
export default router
```

如果需要获取动态路由传递的的路径信息，可以通过 **this.$route.params.xx** 获取（xx是你在路由匹配规则中路由动态路径的那个名称，以上为例，为 id），在 template 中可以简写为 $route.params.xx。

还有一种获取方式是通过 props 来获取的，首先在路由配置种给动态路由加上 props: true，如：

```js
{ path: '/about/:id', component: A1, props: true },
```

然后在跳转组件中加上 props 属性接收即可：

```js
<template>
  <div id="container">
    <h3>{{ id }}</h3>
  </div>
</template>

<script>
export default {
  props: ["id"],
};
</script>
```

#### 编程式导航
> 谈编程式导航前，可以先了解一下声明式导航，声明式导航是指在浏览器中通过点击链接的方式实现的导航跳转，如a链接、路由链接都属于声明式导航。而编程导航是指通过调用 API 来实现导航的方式，如网页中调用 location.href 跳转到新页面是编程式导航。

vue-router 为我们提供了许多用来编程式导航的 API，常用的有以下三个：

```js
 // 跳转到指定的 hash 地址，并增加一条历史记录
 this.$router.push('hash地址')
```

```js
// 跳转到指定的 hash 地址，并替换掉当前的历史记录
this.$router.replace('hash地址')
```

```js
// 前进或者后退n层历史记录, 正数前进, 负数后退
// 如果只想后退一层页面，可以调用 $router.back()
// 如果只想前进一层页面，可以调用 $router.forward()
this.$router.go(数值n)
```

#### 导航守卫
> 导航守卫可以拦截路由跳转，并在跳转前做一些自定义操作。而常用的**全局前置守卫**其实就是 vue 提供的一个在路由之前执行的一个方法，在每次路由跳转之前都会调用该方法。

使用举例：

```js
// 1、导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2、调用 Vue.use() 函数, 把 VueRouter 安装为 Vue 的插件
Vue.use(VueRouter)

// 3、创建路由的实例对象
const router = new VueRouter({
  //...
})

// 全局前置守卫
router.beforeEach((to, from, next) => {
  // to 是将要访问的路由的信息对象
  // from 是将要离开的路由的信息对象
  // next 是一个函数, 调用 next() 表示放行, 允许这次路由导航, 如果不调用 next(), 表示无法放行, 什么都不展示
  // next() 有三种调用方式, 第一种next()表示直接放行, 第二种next('/login')表示跳转到'/login'页面, 第三种next(false)表示哪都不许去,强制停留在本页面
  if (to.fullPath.includes('/movie')) {
    next('/home')
  } else {
    next()
  }
})

// 4、向外共享路由的实例对象
export default router
```

### 路由特性

#### 路由元信息

> 路由元信息是 vue 提供的附加在某个路由上的属性信息，可以通过 **$route.meta.属性** 访问，它可以在路由地址或者导航首位上被访问到，并用于被获取。

route/index.js：

```js
const router = new VueRouter({
  //配置路由
  routes: [
    {
      path: '/home',
      component: Home,
      // 路由元信息，可以存放自定义KV的meta信息
      meta: {
        footerShow: true,
      }
    },
  ]
})
```

app.vue：

```vue
<template>
  <div>
    <Header></Header>
    <!-- 路由组件占位符 -->
    <router-view></router-view>
    <!-- 通过路由中的meta.footerShow属性决定是否展示Footer组件 -->
    <Footer v-show="$route.meta.footerShow"></Footer>
  </div>
</template>
...
```

#### 路由传参

> vue 提供了两种路由传参方式，可以将参数通过路由的方式传递给被路由组件。一种是通过字符串方式传，另一种是通过对象方式传参。

##### 字符串传递

> 字符串传递可以传递 params 和 query 两种，params 可以看成动态路由路径，query 可以看成是路由跳转的参数。vue 分别提供了 $route.params 和 $route.query 两种方式获取它们。

通过 **$route.params.属性** 方式获取过程如下：

配置动态路由：

```js
const router = new VueRouter({
  //配置路由
  routes: [
    ...
    {
      path: '/search/:keyword',
      component: Search,
      meta: {
        footerShow: true,
      }
    },
    ...
  ]
})
```

路由跳转传参：

```js
this.$router.push(`/search/${this.searchData}`).catch((e) => {});
```

在路由跳转组件中通过 **$route.params.参数名** 方式获取对应属性：

```js
{{ $route.params.keyword }}
```

通过 **$route.query.属性** 方式获取过程如下：

编程式导航时将参数通过 **? &** 方式加在路由后面：

```js
this.$router.push(`/search/${this.searchData}?a=b&b=c`).catch((e) => {});
```

在跳转组件中通过 **$route.query.参数名** 方式获取：

```js
{{ $route.query.a }}
```

##### 对象传递

> 对象传递是使用频率较高的，需要掌握。通过路由跳转传递对象参数时需要先给路由命名，才可以通过对象形式给路由设置参数。

配置路由：

```js
const router = new VueRouter({
  //配置路由
  routes: [
    ...
    {
      path: '/search/:keyword',
      component: Search,
      meta: {
        footerShow: true,
      },
      name: "search",
    },
    ...
  ]
})
```

路由跳转传参：

```js
goSearch() {
      this.$router
        .push({
          // 指定哪个路由
          name: "search",
          // params 参数
          params: {
            keyword: this.searchData,
          },
          // query 参数
          query: {
            k: this.searchData,
            j: "abc",
          },
        })
        .catch((e) => {});
    },
  },
```

获取参数：

```js
{{ $route.params.keyword }}, {{ $route.query }}
```


> [!NOTE] 技巧
> 路由参数传递时，如果 params 参数不保证会被传递，那么可以在动态路由的 :keyword 后面加上一个 ?，这样可以避免因为不传递 params 参数导致URL有误，如：'/search/:keyword?'


## 问题解决

> vue 中常见的问题及解决。

### 1、样式冲突

> 样式冲突是指在如 App.vue 文件中引入了其它多个组件，而引入组件的样式默认都是全局的，在某个组件中设置的元素样式可能会影响其它组件中的元素样式，这种现象被称作样式冲突。

**解决样式冲突的方式如下**

```vue
<!-- 在需要单独设置样式的组件的 style 标签中加上 scoped 属性-->
<style lang="less" scoped>
    h3{
        color: aqua;
    }
</style>
```

[^注]: 这种方式的底层原理为：vue 在编译时会为该组件的所有标签加上一个唯一的属性，然后通过改造我们的选择器为属性选择器（属性选择器格式：标签[属性]{ 样式 } ），然后为标签赋予样式。



### 2、修改第三方组件样式

> 第三方组件引入，如 ElementUI 组件的引入，都是作为子组件引入，如果要直接通过父组件修改子组件中的某些样式，可以采用如下方式

```vue
<style lang="less" scoped>
// /deep/ h4 构成了一个子类选择器, /deep/的值是 vue 加载时为标签加上的唯一的属性值 
/deep/ h4{
    color:rgb(255, 189, 210)
}
</style>
```

[^注]: 一般来说，组件样式都需要设置属性 scoped 来隔绝样式冲突，scoped 可以为父组件中所有标签加上唯一的属性值，但不会给子组件加，所以想修改子组件中的某些样式，直接设置无法成功，vue 会将我们的选择器改造为属性选择器，如h4 标签选择器会变为 h4[属性值] 属性选择器，而加进来的子组件中没有该属性，所以只能通过子类选择器来选择，/deep/ 属性选择器选择到父组件，h4 定位到子组件的标签，所以 /deep/ h4 就能选择到子组件的 h4 标签。

### 3、重复push/replace控制台报错

> vue2对于同一路由的重复 push / replace 会导致控制台报错，有两种解决办法。第一种在 push/replace 后添加 catch ，这种方法需要在每一个 push/replace 后加上 catch。另一种则重写 VueRouter原型对象中的 push/replace 方法。

第一种：
```js
this.$router
        .push({
          // 指定哪个路由
          name: "search",
          // params 参数
          params: {
            keyword: this.searchData,
          },
          // query 参数
          query: {
            k: this.searchData,
            j: "abc",
          },
        })
        .catch((e) => {});
```

第二种（在router/index.js 中找到 VueRouter 对象，重写其原型对象中的 push/replace 方法）：
```js
//保留原始的push方法
let originPush = VueRouter.prototype.push
//第一个参数: 跳转路由地址; 第二个参数: 成功回调; 第三个参数: 失败回调
VueRouter.prototype.push = function (location, resolve, reject) {
  if (resolve && reject) {
    // call方法/apply方法: 调用一次函数并修改函数调用上下文对象,call方法参数用逗号隔开,apply参数用数组传递
    // this表示当前的VueRouter对象
    originPush.call(this, location, resolve, reject);
  } else {
    originPush.call(this, location, () => { }, () => { })
  }
}

//保留原始的replace方法
let originReplace = VueRouter.prototype.replace
VueRouter.prototype.replace = function (location, resolve, reject) {
  if (resolve && reject) {
    originReplace.call(this, location, resolve, reject);
  } else {
    originReplace.call(this, location, () => { }, () => { })
  }
}
```

# 四、Vuex

> Vuex 是 Vue 共享数据的一种机制，它可以允许数据在组件之间共享，并实时响应和更新。

使用 Vuex 的好处：
1. 能够在 vuex 中集中管理共享的数据，易于开发和后期维护。
2. 能够高效地实现组件之间的数据共享，提高开发效率。
3. 存储在 vuex 中的数据都是响应式的，能够实时保持数据与页面的同步。

一般而言，需要在组件之间共享的数据，才有必要存储到 vuex 中，自己私有的数据还是存储在 data 中好。

## Vuex 的基本使用

如果需要在项目中加上 vuex 的话，需要先安装 vuex 依赖包：

```js
npm install vuex --save
```

安装好后在 main.js 中导入和使用 vuex ：

```js
import Vuex from 'vuex'

Vue.use(Vuex)
```

然后在 main.js 里创建一个 store 对象用来存放全局共享变量：

```js
const store = new Vuex.Store({
	// state 中存放的就是全局共享的数据
	state: { count: 0 }
})
```

最后再把创建好的 store 对象挂载到 vue 实例中：

```js
new Vue({
	el: '#app',
	render: h => h(app),
	router,
	// 将创建的共享数据对象，挂载到 Vue 实例中
	// 所有的组件，就可以直接从 store 中获取全局的数据了
	store
})
```

## state

> vuex 通过 state 节点提供组件间的共享数据，所有共享的数据都应放入 store 对象的 state 中。

```js
export default new Vuex.Store({
  state: {
    count: 0,
  },
})
```

### 数据访问

在组件中访问 store 中的共享数据通过以下方式访问：

```js
this.$store.state.全局数据名称
```

