# 一、基础认知

## 1、基础理论

### 1.1 认识网页（了解）

> 通过几个问题了解网页

**问题一：网页由哪些部分组成？**

文字、图片、音频、视频、超链接等。



**问题二：我们看到的网页背后本质是什么？**

前端程序员写的代码。



**问题三：前端的代码是通过什么软件转换成用户眼中的页面？**

通过浏览器转化（解析和渲染）成用户看到的网页。



### 1.2.1 五大浏览器

> ### 浏览器：是网页显示、运行的平台，是前端开发必备利器

常见的五大浏览器有：

- IE浏览器
- 火狐浏览器（FireFox）
- 谷歌浏览器（Chrome）
- Safari浏览器
- 欧朋浏览器（Opera）

其中谷歌浏览器占据约70%市场。



#### 1.2.2 渲染引擎

> 渲染引擎（浏览器内核）：浏览器中专门对代码进行解析渲染的部分，不同浏览器的渲染引擎也是不同的，而渲染引擎不同，解析相同代码时的速度、性能、效果也相同。

|    浏览器    |  内核   |                      备注                       |
| :----------: | :-----: | :---------------------------------------------: |
|      IE      | Trident | IE、猎豹安全、360极速浏览器、百度浏览器等的内核 |
|   FireFox    |  Gecho  |                 火狐浏览器内核                  |
|    Safari    | Webkit  |                 苹果浏览器内核                  |
| Chrome/Opera |  Blink  |             Blink其实是Webkit的分支             |



### 1.3.1 Web标准引入

> Web标准其实是为了解决不同浏览器渲染引擎不同，对代码解析效果会存在差异的问题，即引入Web标准让不同浏览器按照相同的标准显示结果，让展示的效果统一。



#### 1.3.2 Web标准构成

| 构成 |    语言    |                        说明                        |
| :--: | :--------: | :------------------------------------------------: |
| 结构 |    HTML    |                   页面元素和内容                   |
| 表现 |    CSS     | 网页元素的外观和位置等页面样式（如：颜色、大小等） |
| 行为 | JavaScript |              网页模型的定义与页面交互              |



## 2、HTML入门

> 先通过一个小例子入门HTML

<img src="https://img1.imgtp.com/2022/10/17/IEMBjYmn.png" alt="1665967976426.png" title="1665967976426.png" />

<img src="https://img1.imgtp.com/2022/10/17/9mT2uDDo.png" alt="1665968075170.png" title="1665968075170.png" />

![image-20221017085523026](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20221017085523026.png)

### 2.1 HTML页面骨架

> HTML页面类似于一篇文章，文章内容有固定结构，如开头、正文、落款等。网页也存在固定的结构的，如：整体、头部、标题、主体。当然网页的固定结构（骨架）要通过特定的HTML标签进行描述。

```html
<html>
    <head>
        <title>网页的标题</title>
    </head>
    <!--网页内容都存在于<body>中-->
    <body>
        网页的主体内容
    </body>
</html>
```



### 2.2 vscode简介和使用

> vscode是目前最常用的前端开发工具，记事本只是最简陋的开发工具，其它开发工具还有Webstorm、Sublime、Dreamweaver、Hbuilder等。vscode具有较多优点，如体积小、支持插件且插件丰富。

vscode的使用很简单，直接将目标文件夹拖进vscode右边代码区即可，并且vscode具有快捷操作，输入!然后回车或者tab即可自动生成HTML页面骨架。

<img src="https://img1.imgtp.com/2022/10/17/qimlOFp9.png" alt="1665972130921.png" title="1665972130921.png" />

在vscode中运行HTML文件也很简单，可以通过右键 -> Open In Default Browser或者快捷键Alt + B即可打开HTML文件到默认浏览器。

<img src="https://img1.imgtp.com/2022/10/17/wZ1VvwTg.png" alt="1665972366836.png" title="1665972366836.png" style="zoom:33%;" />

#### 2.2.1 vscode快捷键

> 记录vscode中较为常用的快捷键

|  快捷键  |                             作用                             |
| :------: | :----------------------------------------------------------: |
| Alt + b  |                  打开此HTML文件到默认浏览器                  |
| Ctrl + c | 复制（在vscode中该快捷键和Ctrl + v可以**行后**使用做到复制粘贴） |
| Ctrl + v |                             粘贴                             |
| Ctrl + d |         可以多选目标内容进行同时操作（多次按下即可）         |



#### 2.2.2 vscode便捷设置

> 记录一些vscode会使用到的便捷设置

|             设置             |                  作用                  |
| :--------------------------: | :------------------------------------: |
| view -> Word Wrap（Alt + z） | 让vscode中的代码根据代码区大小自动换行 |
|                              |                                        |
|                              |                                        |



## 3、语法规范

### 3.1 注释

> 程序员在写代码时也会添加注释，方便下次看到此处时想起功能和含义。注释既是给自己看的，也是给同事或者其它程序员看的。注释不会对代码产生影响，不会被解析执行。

vscode提供了快捷注释按键：ctrl + /

<img src="https://img1.imgtp.com/2022/10/17/kVSLCMkZ.png" alt="1665972848652.png" title="1665972848652.png" />



### 3.2 HTML标签构成

> HTML的标签由<、>、/、英文单词或字母组成，并可以分为单标签和双标签。

结构说明：

1. 标签由**<、>、/、英文单词或字母**组成，并且把标签中<>包括起来的英文单词或字母称为**标签名**。
2. 常见标签由两部分组成，我们称之为：**双标签**。前部分叫**开始标签**，后部分叫**结束标签**（结束标签带有/），这其中包裹内容。
3. 少数标签由一部分组成，我们称之为：**单标签**。自称一体，无法包括内容。

标签结构图：

<img src="https://img1.imgtp.com/2022/10/17/lK21Suvm.png" alt="1665973632776.png" title="1665973632776.png" />



### 3.3 标签之间的关系

> 标签之间也具有关系，如父子关系（嵌套关系）和兄弟关系（并列关系），且d标签之间仅有这两种关系。

父子关系（嵌套关系）：

```html
<head>
    <title></title>
</head>
```

兄弟关系（并列关系）：

```html
<head></head>
<body></body>
```



# 二、HTML标签学习

> 学习HTML中常用的标签。

## 1、排版标签

### 1.1 标题标签

> 标题在HTLM标签中十分重要，主要用来突出页面主题，HTML一共定义了**六个**标题标签，从一级到六级，重要程度依次递减。标题标签的特点是：文字都有**加粗增大**，但从h1 -> h6逐渐减小，并且是**独占一行**的。

代码（h系列标签）：

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
    <h1>一级标题</h1>
    <h2>二级标题</h2>
    <h3>三级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>   
</body>
</html>
```

<img src="https://img1.imgtp.com/2022/10/17/csyymCmO.png" alt="1665975604122.png" title="1665975604122.png" style="zoom: 33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/eqIno43j.png" alt="1665975685738.png" title="1665975685738.png" />

### 1.2 段落标签

> 常用作分段显示，使用时其特点是段落之间（其它标签也会）会**存在间隙**，并且同时**独占行**。

代码（分段标签）：

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
    <p>噫吁嚱，危乎高哉!蜀道之难，难于上青天!蚕丛及鱼凫，开国何茫然!尔来四万八千岁，不与秦塞通人烟。西当太白有鸟道，可以横绝峨眉巅。地崩山摧壮士死，然后天梯石栈相钩连。</p>
    <p>上有六龙回日之高标，下有冲波逆折之回川。黄鹤之飞尚不得过，猿猱欲度愁攀援。青泥何盘盘，百步九折萦岩峦。扪参历井仰胁息，以手抚膺坐长叹。</p>
</body>
</html>
```

<img src="https://img1.imgtp.com/2022/10/17/CHjSCQ5R.png" alt="1665976567855.png" title="1665976567855.png" style="zoom:33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/EuGjiFU7.png" alt="1665976604635.png" title="1665976604635.png" />

### 1.3 换行标签

> 作用是针对此行强制换行。特点是换行标签是**单标签**，并会进行强制换行。

代码（换行标签）：

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
    噫吁嚱，危乎高哉!蜀道之难，难于上青天! <br> 蚕丛及鱼凫，开国何茫然!尔来四万八千岁，不与秦塞通人烟。西当太白有鸟道，可以横绝峨眉巅。地崩山摧壮士死，然后天梯石栈相钩连。
</body>
</html>
```

<img src="https://img1.imgtp.com/2022/10/17/WaizVdmm.png" alt="1665976984974.png" title="1665976984974.png" style="zoom:33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/aKXVcmB8.png" alt="1665977014064.png" title="1665977014064.png" />

### 1.4 水平分割线

> 多用在不同主题内容之间进行分割，显示效果就是一条水平的细线。特点是**单标签**。

代码（水平分割线）：

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
    上半部分
    <hr>
    下半部分
</body>
</html>
```

<img src="https://img1.imgtp.com/2022/10/17/K0pDm47q.png" alt="1665977332689.png" title="1665977332689.png" style="zoom:33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/zsqL5veH.png" alt="1665977365390.png" title="1665977365390.png" />

## 2、文本格式化标签

> 对文本的相关操作，如加粗、下划线、倾斜、删除线等效果，具体有两种场景的代码，区别是是否需要突出该位置的重要性，在显示方面没有区别。

| 标签 |  说明  | 标签（突出重要性） |  说明  |
| :--: | :----: | :----------------: | :----: |
| <b>  |  加粗  |      <strong>      |  加粗  |
| <u>  | 下划线 |       <ins>        | 下划线 |
| <i>  |  倾斜  |        <em>        |  倾斜  |
| <s>  | 删除线 |       <del>        | 删除线 |

<img src="https://img1.imgtp.com/2022/10/17/kaeBVLXq.png" alt="1665994811836.png" title="1665994811836.png" style="zoom:33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/8CNPQZ1U.png" alt="1665994847550.png" title="1665994847550.png" />

## 3、媒体标签

### 3.1 图片标签

> 即在网页中显示目标图片。其特点是**单标签**，并且需要借助标签的属性设置来展示对应的效果，很多标签都可以通过属性来**指定显示效果**。

代码：

```html
<!--img是图片标签名，src=""是其属性，src是该属性的属性名，""中的是它的属性值-->
<!--src是标签的图片路径属性，alt是图片加载失败时的替换文本内容-->
<img src="" alt="">
```

<img src="https://img1.imgtp.com/2022/10/17/TcV1pK88.png" alt="1665995912293.png" title="1665995912293.png" style="zoom:33%;" />

[^注]: 上图中所使用的src=“”路径是相对路径，**相对路径**是指相对当前目录下的其它目录，可以为上级目录（一级目录一个../，如上一级为../th.jfif），可以为同级目录（不写文件或写成./，如th.jfif或者./th.jfif），或者下级目录（/文件名/文件，如/cat/th.jfif）。还有一种方式是设置为**绝对路径**，绝对路径是指从盘符（如C:/）开始一直到目标位置的路径。

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/9QHYbvrr.png" alt="1665995951523.png" title="1665995951523.png" style="zoom:33%;" />

#### 常用属性

|  属性  |             作用             |
| :----: | :--------------------------: |
|  src   |         指定图片路径         |
|  alt   | 图片加载失败时的展示文本内容 |
| title  |     鼠标悬停时的展示文本     |
| width  |           图片宽度           |
| height |           图片高度           |

[^注]: 当width或者height只设置了其中一个时，另外一个会根据图片比例等比变化。但是如果两个都设置了，有可能会导致图片变形。

#### 标签属性使用注意点

1. 标签的属性写在**开始标签**内部
2. 标签上可以同时存在多个属性
3. 属性之间以空格隔开
4. 标签名与属性之间必须以空格隔开
5. 属性之间没有顺序之分



### 3.2 音频标签

> 在页面中插入音频。需要注意音频标签的部分属性需要谨慎设置，如自动播放，目前音频标签只支持三种格式：MP3、Wav、Ogg。

代码：

```html
<audio src="./door_bell.mp3" controls autoplay loop></audio>
```

<img src="https://img1.imgtp.com/2022/10/17/ks7iqMyw.png" alt="1665998150456.png" title="1665998150456.png" style="zoom:33%;" />

执行效果：

<img src="https://img1.imgtp.com/2022/10/17/ElgvouX1.png" alt="1665998209975.png" title="1665998209975.png" />

#### 常用属性

|  属性名  |             功能             |
| :------: | :--------------------------: |
|   src    |           音频路径           |
| controls |        显示播放的控件        |
| autoplay | 自动播放（部分浏览器不支持） |
|   loop   |           循环播放           |



### 3.3 视频标签