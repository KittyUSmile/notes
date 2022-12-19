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

## 1、Vue入门

### 1、简介

> vue，一套用于构建用户界面的前端框架。它可以很方便的往 HTML 页面中填充数据，只要我们遵守它的规范（不遵守的话后期维护很难）

Vue的规范和知识点：

- 指令、组件（对 UI 结构的复用）、路由、Vuex、vue组件库。



Vue的两个特性：





### 、安装调试工具

在 google 浏览器中安装好 **vue.js devtools** 插件，并设置允许访问文件网址。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-16_16-06-13.png">



<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/19/Snipaste_2022-12-16_16-07-12.png">