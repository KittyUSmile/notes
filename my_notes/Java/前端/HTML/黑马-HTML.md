# 一、基础认知

## 1、基础理论

### 1.1 认识网页（了解）

> 先通过几个问题来了解下网页

**问题一：网页由哪些部分组成？**

文字、图片、音频、视频、超链接等。



**问题二：我们看到的网页背后本质是什么？**

前端程序员写的代码。



**问题三：前端的代码是通过什么软件转换成用户眼中的页面？**

通过浏览器转化（解析和渲染）成用户看到的网页。



### 1.2 五大浏览器

> 浏览器：是网页显示、运行的平台，是前端开发必备利器

常见的五大浏览器有：

- IE浏览器
- 火狐浏览器（FireFox）
- 谷歌浏览器（Chrome）
- Safari浏览器
- 欧朋浏览器（Opera）

其中谷歌浏览器占据约70%市场。



#### 1.2.1 渲染引擎

> 渲染引擎（浏览器内核）：浏览器中专门对代码进行解析渲染的部分，不同浏览器的渲染引擎也是不同的，而渲染引擎不同，解析相同代码时的速度、性能、效果也相同。

|    浏览器    |  内核   |                      备注                       |
| :----------: | :-----: | :---------------------------------------------: |
|      IE      | Trident | IE、猎豹安全、360极速浏览器、百度浏览器等的内核 |
|   FireFox    |  Gecho  |                 火狐浏览器内核                  |
|    Safari    | Webkit  |                 苹果浏览器内核                  |
| Chrome/Opera |  Blink  |             Blink其实是Webkit的分支             |



### 1.3 Web标准引入

> Web标准其实是为了解决不同浏览器渲染引擎不同，对代码解析效果会存在差异的问题，即引入Web标准让不同浏览器按照相同的标准显示结果，让展示的效果统一。



#### 1.3.1 Web标准构成

| 构成 |    语言    |                        说明                        |
| :--: | :--------: | :------------------------------------------------: |
| 结构 |    HTML    |                   页面元素和内容                   |
| 表现 |    CSS     | 网页元素的外观和位置等页面样式（如：颜色、大小等） |
| 行为 | JavaScript |              网页模型的定义与页面交互              |



## 2、HTML入门

> 先通过一个小例子入门HTML

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/image-20221017085523026.png">

### 2.1 HTML页面骨架

> HTML页面类似于一篇文章，文章内容有固定结构，如开头、正文、落款等。网页也存在固定的结构的，如：整体、头部、标题、主体。当然网页的固定结构（骨架）要通过特定的HTML标签进行描述。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_21-41-14.png">



### 2.2 vscode简介和使用

> vscode是目前最常用的前端开发工具，记事本只是最简陋的开发工具，其它开发工具还有Webstorm、Sublime、Dreamweaver、Hbuilder等。vscode具有较多优点，如体积小、支持插件且插件丰富。

vscode的使用很简单，直接将目标文件夹拖进vscode右边代码区即可，并且vscode具有快捷操作，输入!然后回车或者tab即可自动生成HTML页面骨架。

在vscode中运行HTML文件也很简单，可以通过**右键 -> Open In Default Browser**或者快捷键**Alt + B**即可打开HTML文件到默认浏览器。

#### 2.2.1 vscode快捷键

> 记录vscode中较为常用的快捷键

|      快捷键      |                             作用                             |
| :--------------: | :----------------------------------------------------------: |
|     Alt + b      |                  打开此HTML文件到默认浏览器                  |
|     Ctrl + c     | 复制（在vscode中该快捷键和Ctrl + v可以**行后**使用做到复制粘贴） |
|     Ctrl + v     |                             粘贴                             |
|     Ctrl + d     |         可以多选目标内容进行同时操作（多次按下即可）         |
| Ctrl + Shift + ` |                       打开一个终端程序                       |
| Shift + Alt + F  |                          格式化代码                          |
|   Alt + Shift    |                            多选行                            |
|     Ctrl + `     |                        呼出终端控制台                        |



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

vscode提供了快捷注释按键：**ctrl + /**



### 3.2 HTML标签构成

> HTML的标签由<、>、/、英文单词或字母组成，并可以分为单标签和双标签。

结构说明：

1. 标签由**<、>、/、英文单词或字母**组成，并且把标签中<>包括起来的英文单词或字母称为**标签名**。
2. 常见标签由两部分组成，我们称之为：**双标签**。前部分叫**开始标签**，后部分叫**结束标签**（结束标签带有/），这其中包裹内容。
3. 少数标签由一部分组成，我们称之为：**单标签**。自称一体，无法包括内容。



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

## 4、字符实体

> 字符实体指的是通过字符表示一个既定的符号，一般用于有编译冲突的字符。比如浏览器最多只识别一个HTML中的空格，要想多打几个空格，则使用符号&nbsp ； 即可。

字符实体的结构是：&英文;

常见的字符实体有：

| 显示结果 |  描述  | 实体名称 |
| :------: | :----: | :------: |
|          |  空格  | &nbsp ;  |
|    <     | 小于号 |  &lt ;   |
|    >     | 大于号 |  &gt ;   |
|    &     |  和号  |  &amp ;  |
|    “     |  引号  | &quot ;  |
|    ‘     | 单引号 | &apos ;  |
|    ￥    |   元   |  &yen ;  |





# 二、HTML标签学习

> 学习HTML中常用的标签。

## 1、排版标签

### 1、标题标签

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

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112501.png"/>

### 2、段落标签

> 常用作分段显示，使用时其特点是段落之间（其它标签也会）会**存在间隙（上下之间）**，并且同时**独占行**。

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

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112542.png"/>

### 3、换行标签

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

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112626.png"/>

### 4、水平分割线

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

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112702.png"/>

## 2、文本标签

> 对文本的相关操作，如加粗、下划线、倾斜、删除线等效果，具体有两种场景的代码，区别是是否需要突出该位置的重要性，在显示方面没有区别。

| 标签 |  说明  | 标签（突出重要性） |  说明  |
| :--: | :----: | :----------------: | :----: |
| <b>  |  加粗  |      <strong>      |  加粗  |
| <u>  | 下划线 |       <ins>        | 下划线 |
| <i>  |  倾斜  |        <em>        |  倾斜  |
| <s>  | 删除线 |       <del>        | 删除线 |

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112739.png"/>

## 3、媒体标签

### 3.1 图片标签

> 即在网页中显示目标图片。其特点是**单标签**，并且需要借助标签的属性设置来展示对应的效果，很多标签都可以通过属性来**指定显示效果**。

代码（图片标签）：

```html
<!--img是图片标签名，src=""是其属性，src是该属性的属性名，""中的是它的属性值-->
<!--src是标签的图片路径属性，alt是图片加载失败时的替换文本内容-->
<img src="" alt="">
```

[^注]: src=“./”路径是相对路径，**相对路径**是指相对当前目录下的其它目录，可以为上级目录（一级目录一个../，如上一级为../th.jfif），可以为同级目录（不写文件或写成./，如th.jfif或者./th.jfif），或者下级目录（/文件名/文件，如/cat/th.jfif）。还有一种方式是设置为**绝对路径**，绝对路径是指从盘符（如C:/）开始一直到目标位置的路径。

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112821.png"/>



#### 常用属性

|      属性      |                 作用                 |
| :------------: | :----------------------------------: |
|      src       |             指定图片路径             |
|      alt       |     图片加载失败时的展示文本内容     |
|     title      |         鼠标悬停时的展示文本         |
|     width      |               图片宽度               |
|     height     |               图片高度               |
| vertical-align | 调节图片垂直对齐方式（middle为居中） |

[^注]: 当width或者height只设置了其中一个时，另外一个会根据图片比例等比变化。但是如果两个都设置了，有可能会导致图片变形。

#### 标签属性使用注意点

1. 标签的属性写在**开始标签**内部
2. 标签上可以同时存在多个属性
3. 属性之间以空格隔开
4. 标签名与属性之间必须以空格隔开
5. 属性之间没有顺序之分



### 3.2 音频标签

> 在页面中插入音频。需要注意音频标签的部分属性需要谨慎设置，如自动播放，目前音频标签只支持三种格式：MP3、Wav、Ogg。

代码（音频标签）：

```html
<audio src="./door_bell.mp3" controls autoplay loop></audio>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140014.png"/>

#### 常用属性

|  属性名  |             功能             |
| :------: | :--------------------------: |
|   src    |           音频路径           |
| controls |        显示播放的控件        |
| autoplay | 自动播放（部分浏览器不支持） |
|   loop   |           循环播放           |



### 3.3 视频标签

> 在页面中插入视频。同样设置自动播放属性时要谨慎设置，目前视频标签只支持三种格式：MP4、WebM、Ogg。

代码（视频标签）：

```html
<video src="./movie.mp4" controls autoplay muted loop></video>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140109.png"/>

#### 常用属性

|   属性   |                            功能                             |
| :------: | :---------------------------------------------------------: |
|   src    |                         视频的路径                          |
| controls |                       显示播放的控件                        |
| autoplay | 自动播放（谷歌浏览器需要配合加上muted属性实现静音自动播放） |
|   loop   |                          循环播放                           |



## 4、链接标签

> 链接的作用是进行跳转，跳转对象可以是一个网址，可以是一个文件，也可以什么都不是。其特点是这是一个双标签，内部可以包括内容，链接放在a标签中，点击a标签内容之后跳到指定页面。

代码（链接标签）：

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
    <!-- 跳转网址 -->
    <a href="https://www.baidu.com" target="_blank">百度</a>
    <br>
    <!-- 跳转本地地址 -->
    <a href="./th.jfif" target="_blank">猫猫</a>
    <br>
    <!-- 空链接（一般在网站开发初期，我们还不知道跳转地址时，则书写链接地址为#） -->
    <a href="#" target="_blank">哪也不去</a>
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140234.png"/>

[^注]: 1、如果想要点击链接之后另外起一个网页展示链接内容，则可以配置a标签的target属性，当target=“_blank”时效果为在新窗口中跳转，会保留下原网页。当target=“_self”时，也是跳转标签的默认值，会直接在原网页上加载链接内容，覆盖掉原网页。    



## 5、列表标签

> 用来按照固定格式展示数据的一种标签，一共有**三种**不同的列表标签，分别是**无序列表**、**有序列表**和**自定义列表**。

### 5.1 无序列表

> 在网页中的表示就是一组无顺序之分的列表，其特点是列表的每一项前默认显示的是**圆点**。

**标签组成**：

| 标签名 |                说明                |
| :----: | :--------------------------------: |
|   ul   | 作为无序列表的整体，用于包裹li标签 |
|   li   |           表示一个无序行           |

注 ul标签中只允许包含li标签，li标签可以包含**任意内容**（可以包括另一个ul或其它）

**代码（无序列表）**：

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
    <h2>水果列表</h2>
    <ul>
        <li>榴莲</li>
        <li>香蕉</li>
        <li>苹果</li>
        <li>哈密瓜</li>
        <li>火龙果</li>
    </ul>
</body>
</html>
```

**执行效果**：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140316.png"/>

[^注]: 如果要去除列表标签的默认样式，可以采用ul属性中的 list-style: none 去掉



### 5.2 有序列表

> 在网页中表示一组有顺序之分的列表数据，特点是列表的每一项前默认都会显示序号标识。

**标签组成**：

| 标签名 |              说明              |
| :----: | :----------------------------: |
|   ol   | 有序列表的整体，用来包裹li标签 |
|   li   |         表示一个有序行         |

[^注]: ol标签中只允许包含li标签，li标签可以包含**任意内容**

**代码**：

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
    <h2>成绩排行</h2>
    <ol>
        <li>小天</li>
        <li>小丽</li>
        <li>小明</li>
    </ol>
</body>
</html>
```

**执行效果**：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140352.png"/>



### 5.3 自定义列表

> 通常有些列表我们不希望使用有序或无序时就可以使用自定义列表，这种列表也会用于网页底部的导航，其特点是默认情况下dt与dd之间有缩进效果。

**标签组成**：

| 标签名 |                说明                 |
| :----: | :---------------------------------: |
|   dl   | 自定义列表的整体，用于包裹dt/dd标签 |
|   dt   |          自定义列表的主题           |
|   dd   |     自定义列表中对应主题的内容      |

[^注]: dl标签中只允许包含dt/dd标签，dt/dd标签可以包含**任意内容**

**代码**：

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
    <dl>
        <dt>帮助中心</dt>
        <dd>购物指南</dd>
        <dd>账户管理</dd>
        <dd>订单列表</dd>
    </dl>
</body>
</html>
```

**效果展示**：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140444.png"/>



## 6、表格标签

> 将表格以标签形式表现出来，在网页中以行+列的单元格的方式展示数据，比如：学生成绩表。

### **基本标签**

| 标签名 |              说明              |
| :----: | :----------------------------: |
| table  |    表格整体，可以包含多个tr    |
|   tr   |    表格每行，可以包含多个td    |
|   td   | 表格的单元格，可以包含具体内容 |

[^注]: 标签中的嵌套关系为：table > tr > td

和表格**搭档**的其它标签：

| 标签名  |    名称    |                           作用                           |
| :-----: | :--------: | :------------------------------------------------------: |
| caption |  表格标题  |  一般写在table标签里，作为表格的标题，展示在表格正上方   |
|   th    | 表头单元格 | 与td标签类似，区别在th内容**居中且加粗**，一般作为列标题 |

表格中还存在一种**结构标签**，结构标签的作用主要是让代码**易于辨识**，即告诉开发者哪部分是表格标题，哪部分是表格内容，哪部分是表格末尾，浏览器仅会对其各自所占大小做相应处理，让表格内容部分占据更多空间。

| 标签名 |                名称                |
| :----: | :--------------------------------: |
| thread | 表格头部，一般用其包裹表格标题部分 |
| tbody  | 表格主题，一般用其包裹表格内容部分 |
| tfoot  | 表格底部，一般用其包裹表格底部内容 |

```html
<table border="1" width="500" height="300">
    <caption>学生成绩单</caption>
    <thead>
        <tr>
            <th>姓名</th>
            <th>成绩</th>
            <th>评语</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>小明</td>
            <td>100分</td>
            <td>可喜可贺</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>总结</td>
            <td>A+</td>
            <td>三好学生</td>
        </tr>
    </tfoot>
</table>
```



### 表格属性

> 表格也具有相关属性，可以设置表格基本的展示效果，以下是较为常用的属性。

| 属性名 | 属性值 |    效果    |
| :----: | :----: | :--------: |
| border |  数字  | 边框的宽度 |
| width  |  数字  | 表格的宽度 |
| height |  数字  | 表格的高度 |

[^注]: 实际开发时最好针对具体的样式使用CSS进行设置

代码：

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
    <table border="1" width="500" height="300">
        <caption>学生成绩单</caption>
        <thead>
            <tr>
                <th>姓名</th>
                <th>成绩</th>
                <th>评语</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>小明</td>
                <td>100分</td>
                <td>可喜可贺</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>总结</td>
                <td>A+</td>
                <td>三好学生</td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140653.png"/>

### 表格合并

> 表格合并可以分为**水平合并**（跨列合并）和**垂直合并**（跨行合并）。注意合并单元格只能在**同一个结构标签中**的单元格才能合并，不能跨结构标签合并，即不能跨像thead、tbody、tfoot这样的结构标签合并单元格。

合并单元格的**步骤**：

1. 先明确要合并哪几个单元格。

2. 通过**左上原则**，来确定保留哪个删除哪个，即单元格合并只能保留左边或者上边的。

   1. 上下合并：保留最上的，删除其他
   2. 左右合并：保留最左的，删除其它

3. 给保留的那个单元格设置：**跨行合并**（rowspan）或者**跨列合并**（colspan）

   | 属性名  |       属性值       |                          说明                          |
   | :-----: | :----------------: | :----------------------------------------------------: |
   | rowspan | 合并的单元格的个数 | 跨行合并，将多行的单元格进行垂直合并，并保留最上面那个 |
   | colspan | 合并的单元格的个数 | 跨列合并，将多列的单元格进行水平合并，并保留做左边那个 |

代码：

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
    <table border="1" width="500" height="300">
        <caption>学生成绩单</caption>
        <thead>
            <tr>
                <th>姓名</th>
                <th>成绩</th>
                <th>评语</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>小明</td>
                <td rowspan="2">100分</td>
                <td rowspan="2">可喜可贺</td>
            </tr>
            <tr>
                <td>小李</td>
                <!-- <td>100分</td> -->
                <!-- <td>可喜可贺</td> -->
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td>总结</td>
                <td colspan="2">A+</td>
                <!-- <td>三好学生</td> -->
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140608.png"/>

## 7、表单标签

> 当业务中出现登录、注册或者搜索相关功能时需要考虑加上的标签。功能是向后端发送用户输入的数据。

### 7.1、input 系列标签

> 用于对用户输入或选择的数据进行收集，可以根据设置input标签的属性来使用不同的功能。在用户登录页、注册页使用较多。

input标签重要的属性有 **type** 属性，其不同的值对应不同的展示和收集效果。

| type属性值 |                             说明                             |
| :--------: | :----------------------------------------------------------: |
|    text    |             表示一个文本框，可以用于输入单行文本             |
|  password  | 表示一个密码框，可以用来输入密码，输入的数据显示时默认用小圆点代替 |
|   radio    |             表示单选框，用于多选一，且只能选一个             |
|  checkbox  |              表示多选框，用于多选多，可以选多个              |
|    file    |                   文件选择，可用于上传文件                   |
|   submit   |             提交按钮，可以用于此处数据收集的提交             |
|   reset    |               重置按钮，可以将原输入的数据重置               |
|   button   |           普通按钮，可以配合js绑定事件，默认无功能           |

代码：

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
    文本框：<input type="text"><br>
    密码框：<input type="password"><br>
    单选框：<input type="radio"></input><br>
    多选框：<input type="checkbox"><br>
    文件选择：<input type="file"><br>
    <input type="submit">
    <input type="reset">
    <input type="button" value="普通按钮">
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140831.png"/>

#### 7.1.1 文本框 text

> 一般用在网页中对于输入单行文本的内容收集，可以用于用户名输入或者其它允许显示展示的单行输入。属于input标签的type属性的一种。type=“text”。

常用的 input **搭配属性**：

|   属性名    |                      说明                      |
| :---------: | :--------------------------------------------: |
| placeholder | 占位符。提示用户输入内容，用户输入内容后消失。 |

[^注]: placeholder属性可以针对input标签中多个type进行使用，作用是提示用户输入，一般text、password都可以使用。

代码：

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
    <input type="text" placeholder="用户名"><br>
    <input type="password" placeholder="请输入密码">
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_140915.png"/>

[^注]: 文本框可以通过设置border:0 设置边框宽度的属性



#### 7.1.2 单选框 radio

> 用于当需要多选一的单选表单中，比如性别选择。type=“radio”

常用的 **input** 搭配属性：

| 属性名  |                             说明                             |
| :-----: | :----------------------------------------------------------: |
|  name   | 用于分组。有相同name属性值的单选框为一组，一组中同时只能有一个被选中 |
| checked |                 一般用于设置默认选中的单选框                 |

代码：

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
    性别：
    <input type="radio" name="gender" checked>男</input>
    <input type="radio" name="gender">女</input>
</body>
</html>
```

执行效果：

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_141003.png"/>

#### 7.1.3 文件上传 file

> 需要上传文件时使用，允许单个和多个文件的上传。默认是单文件上传。type=“file”。

常用的搭配属性：

|  属性名   |        说明        |
| :-------: | :----------------: |
| multifile | 允许选择多文件上传 |

代码：

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
    文件上传：<input type="file" multiple></input>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b78a10f447.png">

#### 7.1.4 提交 submit/重置 reset

> input标签中type为 submit、reset、button的都是按钮，其中 submit 和 reset 一般和表单一起使用，而普通 button 按钮则绑定 JS 一起使用。

代码：

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
    <form action="">
        账号：<input type="text"><br>
        密码：<input type="password"><br><br>
        <!-- value属性设置该标签展示的名字 -->
        <input type="reset" value="重置按钮">
        <input type="submit" value="提交按钮">
        <input type="button" value="普通按钮">
    </form>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b7cb9a2985.png">

### 7.2、button 按钮系列

> 按钮也有一个 type 类型的属性，可取submit、reset、button值，效果和 input 中的相同。

代码：

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
    <button type="submit">提交</button>
    <button type="reset">重置</button>
    <button type="button">普通按钮</button>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b7eaae1539.png">

### 7.3、select 下拉菜单标签

> 提供用户下拉选择的表单控件。

标签组成：

- select 标签：下拉菜单的整体（父）
- option 标签：下拉菜单的每一项（子）

常见属性：

- selected：下拉菜单的默认选中

代码：

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
    <select>
        <option>滑冰</option>
        <option>开车</option>
        <option selected>游泳</option>
    </select>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b80e20e489.png">

### 7.4、textarea 文本域标签

> 在网页中提供可输入多行文本的表单控件。标签名为textarea。该标签通过设置属性行宽可能不准确，一般使用中都是通过CSS标签来设置。

常见属性：

- cols：设置文本域内可见宽度
- rows：设置文本域内可见行数

注意点：

- 右下角可以拖拽改变大小
- 实际开发时针对样式效果推荐采用CSS设置

代码：

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
    <textarea cols="30" rows="10"></textarea>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b82a78c2c6.png">

[^注]: 如果要取消右下角拖拽放大功能，设置样式 resize:none; 即可



### 7.5、label 标签

> 一般用于绑定标签内容和标签之间的关系。标签名为 label。

在使用时有两种使用方法。

##### **方法一**

1. 使用 label 标签把标签内容包起来。
2. 在表单标签添加上 id 属性。
3. 在 label 标签中的 for 属性中设置和 id 属性对应的值。

代码：

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
    性别：
    <input type="radio" name="gender" id="nan"><label for="nan">男</label></input>
    <input type="radio" name="gender" id="nv"><label for="nv">女</label></input>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/12/22/635b865de94ac.png">

##### 方法二

1. 直接使用 label 标签把表单内容和表单标签一起包裹起来
2. 需要把 label 标签的 for 属性删除即可

代码：

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
    性别：
    <label>
        <input type="radio" name="gender">男</input>
    </label>
    <label>
        <input type="radio" name="gender">女</input>
    </label>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/2022-10-28_172126.png"/>

## 8、语义化标签

> 语义化标签其实就是布局标签，用来对网页分块布局使用，可以分为无语义的布局标签和有语义的布局标签。含语义和不含语义的区别是该标签是否被认为赋予了某种含义。

### 无语义布局标签

> 一般而言就是<div>和<span>两个标签。其中 div 标签在同一行只能显示一个（独占一行），span 标签一行可以显示多个。默认这些标签都是没有任何效果的，如果要加上颜色或其它效果，使用CSS即可。

代码：

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
    <div>这是div标签</div>
    <div>这是div标签</div>

    <span>这是span标签</span>
    <span>这是span标签</span>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/2022-10-28_170954.png"/>

### 含语义布局标签

> 含语义标签在HTML5时推出，多用于手机端。这些含语义的标签虽然被赋予了某种含义，但是仍然要通过CSS来设置样式和效果。

标签：

| 标签名  |    语义    |
| :-----: | :--------: |
| header  |  网页头部  |
|   nav   |  网页导航  |
| footer  |  网页底部  |
|  aside  | 网页侧边栏 |
| section |  网页区块  |
| article |  网页文章  |

[^注]: 以上标签显示特点和 div 一致，但是比 div 多了被赋予的语义。

代码：

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
    <header>网页头部</header>
    <nav>网页导航</nav>
    <footer>网页底部</footer>
    <aside>网页侧边栏</aside>
    <section>网页区块</section>
    <article>网页文章</article>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/2022-10-28_172430.png"/>

