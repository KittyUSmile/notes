# 一、基础认知

> HTML可以看成是网页的骨架，它决定了网页的结构，而CSS可以看作是网页的表现，决定这副骨架上的肉体是什么样子，JS则决定骨架和肉体组成的机体具有什么样的行为。

## 1、CSS语法规则

> CSS写在<style>标签中，而<style>标签一般写在<head>标签中，位置一般放在<title>标签下面。同样，CSS中的注释是/* */。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/2022-10-28_182724.png">

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* CSS注释 */
        /* 结构: 选择器 {CSS属性;} */
        /* 选择器: 选择某个标签或某类标签 */
        p {
            /* 文字颜色 */
            /* 属性之间通过分号隔开*/
            color:brown;
            /* 文字大小 */
            font-size: 30px;
            /* 背景颜色 */
            background-color: antiquewhite;
        }
    </style>
</head>
<body>
    <p>hello CSS</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_093957.png"/>

[^注]: 如果给一个标签设置两个属性值不同的属性，那后面的属性会覆盖前面的属性，正如CSS的名称那样，Cascading style sheets，层叠样式表，样式会一层一层的层叠覆盖。



## 2、CSS引入方式

> CSS除了直接写在<head>标签中外（内嵌式），还有另外两种写法，外联式和行内式。如果说内嵌式是样式和结构代码在同一个HTML文件中，外联式就是把它们分开了，外联式的写法为将CSS样式单独写在一个.css文件中，然后HTML文件通过<link>标签引入css样式。而行内式则是直接写在结构标签内的样式，耦合性极强。

区别特点：

| 引入方式 |                 书写位置                 | 作用范围 |   使用场景   |
| :------: | :--------------------------------------: | :------: | :----------: |
|  内嵌式  |            写在<style>标签中             | 当前页面 |    小案例    |
|  外联式  | 写在单独的.css文件中，通过<link>标签引入 | 多个页面 |    项目中    |
|  行内式  |          写在标签的style属性中           | 当前标签 | 配合 JS 使用 |

### 内嵌式

> 内嵌式就是将样式代码写在<head>标签中。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 内嵌式样式 */
        p {
            /* 文字颜色 */
            color:brown;
            /* 文字大小 */
            font-size: 30px;
            /* 背景颜色 */
            background-color: antiquewhite;
        }
    </style>
</head>
<body>
    <p>hello CSS</p>
</body>
</html>
```

### 外联式

> 外联式将CSS样式写在单独的.css文件中，当有需要时在HTML中的<head>标签中引入即可。

css文件：

```css
p{
    color: aquamarine;
    font-size: 30px;
    background-color: antiquewhite;
}
```

HTML文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- rel="stylesheet" 意思是引入关系为'样式表'，href="" 即引入文件的地址 -->
    <link rel="stylesheet" href="my.css">
</head>
<body>
    <p>Hello CSS</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_095915.png"/>

### 行内式

> 行内式直接通过将结构样式写在结构标签中。这种写法耦合性强，一般建议通过 JS 配合使用。

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
    <div style="color: green; font-size: 30px; background-color: pink;">Hello CSS</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_100411.png"/>

## 3、CSS特性

> CSS具有三个特性，分别是继承性，层叠性和优先级。

### 1、继承性

> 即子元素具有默认继承父元素样式的特点。可以继承的常见属性有color、文字控制属性(font-style、font-weight、font-size、font-family、text-indent、text-align、line-height)，而像宽高等其它属性就不具备继承性。可以通过浏览器查看继承属性。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            color: rgb(255, 180, 180);
            font-size: 20px;
            /* 非文字属性子类无法继承 */
            height: 40px;
            background-color: rgb(149, 255, 255);
        }
        span{
            background-color: rgb(234, 255, 181);
        }
    </style>
</head>
<body>
    <div>
        这是div文字
        <span>这是一段span文字</span>
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/15/Snipaste_2022-11-15_16-44-43.png">

[^继承失效的特殊情况]: 如果元素有浏览器默认样式，则使用浏览器默认样式，此时的继承失效，如a标签继承其它颜色的父标签，如h标签继承其它设置文字大小的标签。样式显示顺序可以看作：手动设置 > 浏览器默认样式 > 继承样式。

### 2、层叠性

> 层叠性可以从两方面来看待，一方面是针对同一标签设置的多个不同样式层叠生效，如高度、宽度、颜色。另一方面是针对同一标签的多个相同样式，最先加载的样式会被后加载的样式覆盖。



### 3、优先级

> 当同时存在多个选择器对同一个标签内容样式进行设置时，优先级高的选择器样式会覆盖优先级低的选择器样式。优先级样式的展示和样式写的顺序无关。优先级其实可以看作谁设置的范围更细，谁优先级就更高。

优先级排序：

**继承 < 通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < !important**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 继承，最低优先级 */
        body{
            color: black;
        }
        /* 通配符，优先级高于继承 */
        *{
            color: aqua;
        }
        /* 标签选择器，优先级高于通配符 */
        /* !important 将该方式设置的优先级设为最高（在继承处不生效） */
        div{
            color: bisque !important;
        }
        /* 类选择器，优先级高于标签选择器 */
        .box{
            color: red;
        }
        /* id选择器，优先于高于类选择器 */
        #box{
            color:orange
        }
    </style>
</head>
<body>
    <!-- 行内样式，优先级仅次于!important -->
    <div class="box" id="box" style="color: green;">测试优先级</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_10-02-36.png">

#### 权重叠加计算

> 一般情况下**同一块元素**的样式展示可以直接通过优先级看出，但有时候可能存在多个复合选择器对**同一块**元素设置样式，这时就需要通过权重叠加来计算出最终的样式。

权重叠加计算公式（从左到右，相同则比下一位）：

|     第一级     |     第二级     |     第三级     |      第四级      |
| :------------: | :------------: | :------------: | :--------------: |
| 行内样式的个数 | id选择器的个数 | 类选择器的个数 | 标签选择器的个数 |

比较规则：

1. 依次比较当级数字，如果比较出来后，之后统统不看。
2. 如果所有数字均相同，则表示优先级相同，则比较层叠性，即谁写在后面，谁展示样式。
3. 当都是通过继承展示样式时，看哪个继承样式到达范围更细，哪个就生效。

[^注]: !important如果不是继承，则权重最高，权重叠加计算规则失效。



## 4、CSS书写顺序

> 按以下顺序排CSS属性，浏览器执行效率更高

1. 浮动 / display
2. 盒子模型：margin、border、padding、宽度高度背景色
3. 文字模式



# 二、选择器

> 选中HTML中的某个或某类结构，一般通过选择器赋予样式。选择器共有四种：标签选择器、类选择器、id选择器、通配符选择器。

## 1、基础选择器

### 1、标签选择器

> 通过选中某类标签来赋予样式，比如选中<p>标签，就能给页面中所有<p>标签中的内容赋予相同样式。

**用法：p{}**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            color: aquamarine;
            font-size: 30px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <p>Hello 标签选择器</p>
    <p>选择p标签</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_101711.png"/>

[^注]: 标签选择器适用场景是对页面中某个标签的样式均统一。标签选择器无法让同标签的内容展示不同样式。



### 2、类选择器

> 类选择器的选择主要是通过标签的class属性，给标签赋予class属性，然后就可以通过类选择器选择该类进行样式赋予。简单说就是给标签分类，分过不同分类赋予不同样式。

结构：**.类名 {属性名: 属性值}**

作用：通过类名，找到页面中所有带有这个类名的标签，设置样式

注意点：

1. **所有标签**上都有class属性，class属性的属性值被称为类名（类似于名字）
2. 类名可以由**数字、字母、下划线、中划线**组成，但不能以数字或中划线开头
3. 一个标签可以有**多个**类名，类名之间以空格隔开
4. 类名可以**重复**，一个类选择器可以同时选中多个标签

代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 选择.color类的标签 */
        .color {
            color: rgb(249, 132, 90);
        }
        /* 选择.size类的标签 */
        .size{
            font-size: 30px;
        }
        /* 选择p标签设置样式 */
        p {
            color: rgb(90, 218, 175);
            font-size: 25px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <!-- 设置类为color和size -->
    <p class="color size">Hello 类选择器</p>
    <p class="size">Hello 类选择器</p>
</body>

</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_103348.png"/>

[^注]: 类选择器的样式可以覆盖标签选择器的样式。



### 3、id 选择器

> 在样式设置的使用上，id 选择器与类选择器区别不大，但这不是 id 选择器的主要功能，id 选择器还有一个主要作用，就是配合 JS 找标签。

结构：**#id值 {属性名: 属性值}**

作用：通过 id 属性值找到页面中的标签

注意点：

1. **所有标签**都有 id 属性
2. id 属性值类似于身份证号码，在一个页面中理论上是**唯一的**，不过重复也不会报错。
3. 一个标签上**只能有一个** id 属性值
4. 一个 id 选择器**只能选中一个**标签

代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* id选择器 */
        #div1{
            color: rgb(0, 157, 255);
        }
    </style>
</head>

<body>
    <div id="div1">Hello id选择器</div>
    <!-- 前面div标签已有id为div1，这里不应该再设置id为div1 -->
    <p id="div1">11</p>
</body>

</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_105355.png"/>

### 4、通配符选择器

> 通配符选择器其实就是全标签选择器，可以选中页面中的所有标签。一般的使用场景就是消除所有标签的默认内外边距。

结构：***{属性名: 属性值}**

作用：找到页面中**所有标签**，并设置样式。

注意点：

1. 在开发中使用**极少**，只有在某些特定场景下才会使用。

代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            color: rgb(84, 163, 84);
        }
    </style>
</head>

<body>
    <div>hello 通配符选择器</div>
    <p>你好啊</p>
    <span>无所谓</span>
</body>

</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_110239.png"/>

## 2、进阶选择器



### 1、复合选择器

> 复合选择器是指可以通过写连续的父子选择器来定位某个后代标签。在使用上可以分为**后代选择器**和**子代选择器**。

#### 1、后代选择器

> 选择指定某个标签及其后代，多个选择器之间用空格隔开。

写法：**div p {}** （样例的意思是选择div标签内的所有p标签，包括儿子孙子）

效果：找到所有指定的标签，并为它们都附上对应样式。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 复合选择器选择后代标签 */
        div p{
            color: greenyellow;
        }
    </style>
</head>
<body>
    <!-- 该标签就不会受到影响 -->
    <p>这是一个孤单的p标签</p>
    <div>
        <p>这是div标签的儿子</p>
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_181459.png"/>

[^注]: 后代选择器和子代选择器之间的区别是后代选择器会选择指定标签及其子类和兄弟的子类，子代选择器只会选到那个指定的标签。

#### 2、子代选择器

> 子代选择器根据HTML标签的嵌套关系，选择父元素子代中满足条件的元素。不包括子代的子代。

写法：**选择器1 > 选择器2 {css}**

效果：在选择器1的子代中找到满足选择器2的标签，设置样式

注意点：

1. 子代**只包括了儿子**
2. 子代选择器中，选择器与选择器之间**通过 > 隔开**。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div>a{
            color:aqua
        }
    </style>
</head>
<body>
    <div>
        <a href="#">这是子代标签</a>
        <p>
            <a href="#">这是子代标签的侄子</a>
        </p>
    </div>
</body>
</html> 
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_223408.png"/>



### 2、并集选择器

> 并集选择器其实就是将多个标签写在一起统一设置样式，一起统一的可以有标签、类、id。

语法：**选择器1,选择器2 {css}**

注意点：

1. 并集选择器中的每组选择器之间通过**逗号**分隔
2. 并集选择器中的每组选择器可以是**基础选择器**或者**复合选择器**
3. 并集选择器中的每组选择器通常**一行写一个**，提高代码的可读性

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 并集选择器 允许对多种选择器进行并集*/
        p,
        div,
        #sp,
        .h1_class{
            color: rgb(0, 153, 255);
        }
    </style>
</head>
<body>
    <p>ppp</p>
    <div>div</div>
    <span id="sp">span</span>
    <h1 class="h1_class">h1</h1>
    <h2>h2</h2>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_230141.png"/>



### 3、交集选择器

> 即选择满足多个条件的选择器，比如找到 p 标签且 class=“xx” 的 p 标签。交集的写法就是在各个条件之间连着写即可。

**语法：p.box{}**

注意点：

1. 交集选择器中的选择器之间是紧挨着的，没有任何东西分隔
2. 交集选择器中如果有标签选择器，标签选择器，标签选择器必须写在最前面。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p.box{
            color: aquamarine;
        }
    </style>
</head>
<body>
    <p class="box">p标签且box</p>
    <p>p标签</p>
    <div class="box">div标签且box</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_231430.png"/>



### 4、hover伪类选择器

> 鼠标悬停在某个元素上时元素展现的状态。可以用来设置鼠标移上标签时的变化样式。

**语法：选择器:hover{style样式}**

注意：

- 伪类选择器可以选择任何一个标签。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        a:hover{
            color: rgb(59, 255, 190);
            background-color: rgb(255, 236, 239);
        }
        div:hover{
            color: pink;
        }
    </style>
</head>
<body>
    <a href="#">这是一个链接</a>
    <div>div</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/8/2022-11-08_181341.png"/>

### 5、结构伪类选择器

> CSS提供的一种选择器，通过标签关系来选择标签，比如选择div标签的第一个子元素，或者选择div标签的最后一个子元素。这种选择器的好处是可以减少对于HTML中类的依赖，有利于保持代码整洁（少取名字），通常的使用场景是查找某父级选择器中的子元素。

常用：

|        选择器         |                   说明                   |
| :-------------------: | :--------------------------------------: |
|    E:first-child{}    |  位于父标签中第一个子标签，并且是E标签   |
|    E:last-child{}     | 位于父标签中最后一个子标签，并且是E标签  |
|   E:nth-child(n){}    |   位于父标签中第n个子标签，并且是E标签   |
| E:nth-last-child(n){} | 位于父标签中倒数第n个子标签，并且是E标签 |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 找到子标签中第一个是li的 */
        li:first-child{
            background-color: pink;
        }
        /* 找到子标签中最后一个是li的 */
        li:last-child{
            background-color: pink;
        }
        /* 找到第三个子标签是li的 */
        li:nth-child(3){
            background-color: pink;
        }
        /* 找到倒数第三个子标签是li的 */
        li:nth-last-child(3){
            background-color: pink;
        }
    </style>
</head>
<body>
    <ul>
        <li>这是第1个li</li>
        <li>这是第2个li</li>
        <li>这是第3个li</li>
        <li>这是第4个li</li>
        <li>这是第5个li</li>
        <li>这是第6个li</li>
        <li>这是第7个li</li>
        <li>这是第8个li</li>
    </ul>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_18-23-15.png">

[^注]: E标签可以是其它选择器写法，不一定只能是单个标签的写法。

#### 公式

> 除了可以用n来选择第几个标签外，还可以通过有关n的公式进行选择，比如2n，2n+1等。
>
> 用法如：**li:nth-child(2n){}**

常见公式：

|     功能     |      公式       |
| :----------: | :-------------: |
|     偶数     |    2n、even     |
|     奇数     | 2n+1、2n-1、odd |
|  只选前五个  |      -n+5       |
| 从第五个开始 |       n+5       |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        li:nth-child(2n){
            background-color: pink;
        }
    </style>
</head>
<body>
    <ul>
        <li>这是第1个li</li>
        <li>这是第2个li</li>
        <li>这是第3个li</li>
        <li>这是第4个li</li>
        <li>这是第5个li</li>
        <li>这是第6个li</li>
        <li>这是第7个li</li>
        <li>这是第8个li</li>
    </ul>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_18-36-18.png">



### 6、Emmet语法

> vscode提供了简便开发的便捷语法，快速而强大。

emmet语法提示包括：

- 标签提示
- css属性提示（如输入fz可选择生成font-size，输入fz700生成font-size: 700，输入w300+h200+bgc生成width: 300, height: 200, background-color: ）

|     操作     |   示例    |                 效果                 |
| :----------: | :-------: | :----------------------------------: |
|    标签名    |    div    |             <div></div>>             |
|   类选择器   |   .red    |       <div class="red"></div>        |
|   id选择器   |   #one    |         <div id="one"></div>         |
|  交集选择器  | p.red#one |     <p class="red" id="one"></p>     |
|   子级创建   |   ul>li   |          <ul><li></li></ul>          |
|   内部文本   | ul>li{$}  |     <ul><li>1..2..3..</li></ul>      |
| 子级创建多个 |  ul>li*3  | <ul><li></li><li></li><li></li></ul> |
| 同级多个创建 |   div+p   |          <div></div><p></p>          |





# 三、字体和文本样式

## 1、字体样式

> 字体的大小、粗细、样式和格式的设置。



### 1、字体大小 font-size

> 控制文字的大小，设置时需要加上单位像素（px）。

属性名：**font-size**

取值：**数字 + px**

注意点：

- 谷歌浏览器默认文字大小是 16px
- 单位需要设置，否则不生效

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            font-size: 30px;
        }
    </style>
</head>
<body>
    <p>这是一段文字</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_112019.png"/>



### 2、字体粗细 font-weight

> 对标签内容字体的粗细进行控制，甚至可以更改<h1>标题标签中的字体粗细。

属性名：**font-weight**

取值：

1. **关键字取值**。关键字取值分两类：正常（normal）、加粗（bold），正常对应400，加粗对应700。
2. **数字取值**：100 ~ 900的整百数。

注意点：

- 不是所有字体都提供了九种粗细，因此部分字体粗细修改可能不生效。
- 实际开发中，以正常、加粗两种字体使用较多。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            font-weight: 600;
        }
    </style>
</head>
<body>
    <p>这是一段文字</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_111932.png"/>

### 3、字体样式 font-style

> 即控制字体是否倾斜。只有两种取值：正常（默认值）和倾斜。

取值：

- 正常：normal
- 倾斜：italic

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            font-style: italic;
        }
        em{
            font-style: normal;
        }
    </style>
</head>
<body>
    <div>这是一段倾斜文字</div>
    <em>我是正的</em>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_141706.png"/>

### 4、字体格式 font-family

> 即设置不同格式的字体效果，如楷体、宋体等。Windows默认是微软雅黑，Mac默认是苹方。

属性名：**font-family**

常见取值：**字体1,字体2,字体3,字体4…,字体系列**

- 字体：微软雅黑、黑体、宋体、楷体等
- 字体系列：sans-serif（非衬线系列，横撇竖勾不带）、serif（横撇竖勾带弯）、monospace（等宽字体）

渲染规则：

1. **从左向右**按照顺序查找，如果电脑中未安装此字体，则显示下一个字体。
2. 如果都不支持，则会根据操作系统，显示最后字体系列的**默认字体**。

注意点：

1. 如果字体名称中存在多个单词，可以使用**引号包裹**
2. 最后一项字体列不需要引号包裹
3. 网页开发时，尽量使用系统常见的自带字体，保证不同用户网页都可以正确显示

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            font-family: 楷体,sans-serif;
        }
        p{
            font-family: 黑体,sans-serif;
        }
    </style>
</head>
<body>
    <div>楷体格式</div>
    <p>黑体格式</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_143529.png"/>

### 5、复合写法

> 可以看出，上面设置字体的前缀都是font-，CSS提供了一种复合写法，可以省略一些代码。属性之间用空格隔开。

属性名：font（复合属性）

取值：font : style weight size family

省略要求：只能省略前两个，如果省略了就相当于设置了默认值

注意点：

- 复合写法就相当于一个普通的设置，可以被覆盖，也可以覆盖

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            font: italic 400 30px 楷体;
            font-style: normal;
        }
    </style>
</head>
<body>
    <div>这是一段文字</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_145250.png"/>

## 2、文本样式



### 1、文本缩进 text-indent

> 一般使用在文本首段的缩进中。使用的话一般和em配合使用。

属性名：**text-indent**

取值：

- 数字 + px
- 数字 + em（1em即当前标签内容的font-size的大小）

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{  
            /* 
             错误写法(缩进跟着字体大小走):
                 text-indent: 40px;
                 font-size: 20px;
            */
            /* em即当前标签内容字体大小 */
            text-indent: 2em;
        }
    </style>
</head>
<body>
    <p>射电望远镜就是利用射电波进行观测的望远镜，射电波也是电磁波谱的一部分，频率范围从高频的300吉赫兹到低频的30赫兹，相应的波长范围从1毫米到10000公里。在自然界，从闪电到宇宙天体都会发出射电波。由于星系中心的黑洞被厚厚的星际尘埃和气体阻挡，光学波段的望远镜无能为力，只能采用射电波段。毫米波已经是射电望远镜所用波长的下限，在电磁波谱上已经与红外线接壤。</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_150646.png"/>

[^注]: 使用 em 的好处是无论字体大小如何改变，缩进都不需要改。

### 2、水平对齐 text-align

> 对文本内容按照设置格式对齐，共有三种对齐格式，左对齐、居中对齐、右对齐。对齐不是文本独有的，其它标签内容都能设置对齐，只要设置其父元素即可。

属性名：**text-align**

取值：

| 属性值 |      效果      |
| :----: | :------------: |
|  left  | 左对齐（默认） |
| center |    居中对齐    |
| right  |     右对齐     |

注意点：

- 如果要让文本水平居中，text-align属性给文本所在标签（文本的父元素）设置即可。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h1{
            text-align: center;
        }
        body{
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>新闻标题</h1>
    <img src="locate.png" alt="" height="300">
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_152129.png"/>

[^注]: 像文本、span标签、a标签、input标签、img标签都可以让元素水平居中，只要在它们的父元素上设置即可。

### 3、文本修饰 text-decoration

> 用来修饰文本的样式，即给文本加上下划线、删除线、上划线、无装饰线。

属性名：**text-decoration**

取值：

|    属性值    |        效果        |
| :----------: | :----------------: |
|  underline   |   下划线（常用）   |
| line-through |  删除线（不常用）  |
|   overline   | 上划线（几乎不用） |
|     none     |  无装饰线（常用）  |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            text-decoration: underline;
        }
        p{
            text-decoration: line-through;
        }
        h4{
            text-decoration: overline;
        }
        a{
            text-decoration: none;
        }
    </style>
</head>
<body>
    <div>下划线</div>
    <p>删除线</p>
    <h4>上划线</h4>
    <a href="#">超链接</a>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_153202.png"/>

### 4、行高 line-height

> 设置文本行之间的间距。有两种取值：数字+px、倍数（字体大小的倍数）

属性名：**line-height**

取值：

- 数字 + px
- 倍数（当前标签 font-size 的倍数）

应用场景：

1. 设置文本之间的间距。
2. 网页精准布局时，可以取消上下间距。

注意点：

- 行高也可以加入复合写法，如 **font : style weight size/line-height family**。但要注意样式覆盖问题。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            /* 直接设置 */
            /* line-height: 50px; */
            /* 倍数设置 */
            /* line-height: 1.5; */
            /* 复合写法设置 */
            font: normal 300 20px/1.5 楷体;
        }
    </style>
</head>
<body>
    <p>射电望远镜就是利用射电波进行观测的望远镜，射电波也是电磁波谱的一部分，频率范围从高频的300吉赫兹到低频的30赫兹，相应的波长范围从1毫米到10000公里。在自然界，从闪电到宇宙天体都会发出射电波。由于星系中心的黑洞被厚厚的星际尘埃和气体阻挡，光学波段的望远镜无能为力，只能采用射电波段。毫米波已经是射电望远镜所用波长的下限，在电磁波谱上已经与红外线接壤。</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_155708.png"/>

## 3、扩展

### 1、颜色知识

> 在设计时的颜色取值，共有四种表示方法。分别是关键词法、RGB表示法、RGBA表示法、十六进制表示法。

属性名：

- 文字颜色：**color**
- 背景颜色：**background-color**

属性值：

|  颜色表示方式  |                      表示含义                       |                   属性值                    |
| :------------: | :-------------------------------------------------: | :-----------------------------------------: |
|     关键词     |                   预定义的颜色名                    |          red、green、blue、yellow           |
|   RGB表示法    |      由红蓝绿三原色取色，每项取值范围为：0~255      | RGB(0,0,0)、RGB(255,255,255)、RGB(255,0,0)  |
|   RGBA表示法   |   由红蓝绿三原色取色，A表示透明度，取值范围为0~1    |  RGBA(255,255,255,0.5)、RGBA(255,0,0,0.3)   |
| 十六进制表示法 | 以#开头，每两个位置表示一种原色，将其转为16进制表示 | #000000、#ff0000、#e92322，简写：#000、#f00 |

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
    <div style="color: yellowgreen;">关键词</div>
    <div style="color: rgb(170, 225, 249);">rgb表示法</div>
    <div style="color: rgb(255, 90, 131,0.3);">rgba表示法</div>
    <div style="color: #000">十六进制表示法</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/10/31/2022-10-31_163431.png"/>

# 四、背景

> 背景只作为标签或者盒子的背景展示，不影响或改变标签或盒子的结构。

## 1、背景颜色

> 字面意思，各个标签或是盒子都可以设置背景颜色，属性名字是background-color (bgc)，属性值可以有多种写法：关键字、rgb表示法、rgba表示法、十六进制等

注意点：

- 背景颜色默认是透明，rgba(0,0,0,0)。
- 背景颜色不会影响盒子大小，但是可以标识盒子的大小位置，在开发时可以用背景色进行提示使用。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            /* background-color: rgb(234, 158, 158); */
            /* background-color: yellow; */
            background-color: rgba(0,0,0,0.3);
            /* background-color: rgba(0,0,0,.3); */
            width: 300px;
            height: 200px;
        }
    </style>
</head>
<body>
    <div>背景颜色</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/9/2022-11-09_165517.png"/>

## 2、背景图片

> 给标签或盒子的背景加上图片，属性名是background-image (bgi)，属性值url(‘图片路径’)。

注意点：

- 背景图片中url可以省略引号
- 背景图片默认在水平和垂直方向平铺，默认多超少补。
- 背景图片仅给盒子起装饰效果，类似于背景颜色，不会撑开盒子

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            background-image: url(locate.png);
            background-color: rgb(255, 221, 227);
            width: 400px;
            height: 400px;
        }
    </style>
</head>
<body>
    <div>这是一段文字</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/9/2022-11-09_171206.png"/>

### 平铺

> 默认图片的铺设方式是多超少补，如果想要根据需求来铺设完整图片，可以选择背景平铺属性 background-repeat (bgr) 进行设置。

属性名：**background-repeat (bgr)**

属性值：

|   取值    |             效果             |
| :-------: | :--------------------------: |
|  repeat   | 水平和垂直方向都平铺（默认） |
| no-repeat |            不平铺            |
| repeat-x  |   沿着水平方向（x轴）平铺    |
| repeat-y  |   沿着垂直方向（y轴）平铺    |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            background-image: url(locate.png);
            background-color: rgb(255, 221, 227);
            width: 400px;
            height: 400px;
            /* background-repeat: repeat; */
            background-repeat: no-repeat;
            /* background-repeat: repeat-x; */
            /* background-repeat: repeat-y; */
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/9/2022-11-09_171953.png"/>



### 位置

> 在不平铺显示下，默认背景图片是显示在左上角位置的，如果要修改背景图片位置属性，可通过属性background-position (bgp) 来进行设置。设置背景位置有两种设置方式，名词和坐标。

属性值：**background-position：水平方向位置 垂直方向位置**

使用：

1. 名词（最多可以表示九个位置）
   1. 水平方向
      1. **left**
      2. **center**
      3. **right**
   2. 垂直方向
      1. **top**
      2. **center**
      3. **bottom**
2. 数字 + px（以图片左上角为设置点）
   1. 坐标系
      1. 原点（0,0）：盒子左上角
      2. 正x轴：水平向右
      3. 正y轴：垂直向左

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            background-image: url(locate.png);
            background-color: rgb(255, 221, 227);
            width: 400px;
            height: 400px;
            background-repeat: no-repeat;
            /* background-position: 0 0; */
            /* background-position: 20px 30px; */
            /* background-position: -50px -60px; */
            /* background-position: center center; 等价于 background-position: center;*/
            background-position: left bottom;
        
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/9/2022-11-09_173531.png"/>



## 3、属性复合写法

> 将颜色、图片、平铺、位置连起来在一个属性 background (bg) 后写。各值之间用逗号隔开，书写顺序推荐为color、image、repeat、positior，但实际上没有顺序要求。

使用举例：**background: pink url(‘01.jpg’) no-repeat center center**

注意点：

- 如果需要单独设置样式，则必须将样式写在连写的下面，否则会被覆盖
- 连写设置背景位置时如果使用名词，位置顺序调换也能识别，但如果通过坐标，则不能调换位置

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 400px;
            height: 300px;
            background: pink url("locate.png") no-repeat center center;
        }
    </style>
</head>
<body>
    <div>这是一块div</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/10/2022-11-10_100634.png">

# 五、元素显示模式

> 元素，即带尖括号的任意标签。元素显示模式，简单说就是元素的布局模式，是独占一行，还是可以多元素同行显示，元素的显示模式是可以改变的，常见的元素显示模式有块级元素、行内元素、行内块元素。

## 1、块级元素

> 块级元素的特点是独占一行（即一行只能显示一个），宽度也是父元素的宽度，高度默认由内容撑开，也可以自己设置宽高。代表标签有div、p、h系列、ul、dl、dt、dd、form、header、nav、footer等。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            background-color: rgb(184, 255, 6);
        }
        p{
            background-color: rgb(79, 255, 205);
        }
    </style>
</head>
<body>
    <div>块级元素div</div>
    <p>块级元素p</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/10/Snipaste_2022-11-10_10-46-59.png">

## 2、行内元素

> 行内元素的特点是一行可以显示多个，其宽度和高度设置无效，都由内容撑开。代表标签有a、span、b、u、i、s、strong、ins、em、del等。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        a{
            background-color: rgb(250, 255, 173);
            /* 行内元素设置宽高无效 */
            height: 200px;
            width: 300px;
        }
        span{
            background-color: rgb(168, 255, 252);
        }
    </style>
</head>
<body>
    <a href="#">行内元素a</a>
    <span>行内元素span</span>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/10/Snipaste_2022-11-10_11-00-05.png">

## 3、行内块元素

> 行内块元素的特点是一行可以显示多个，同时还可以设置宽高。代表标签有img、input、textarea、button、select等。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        img{
            height: 100px;
        }
    </style>
</head>
<body>
    <img src="./locate.png" alt="">
    <img src="./locate.png" alt="">
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/15/Snipaste_2022-11-15_16-00-59.png"/>

[^注]: <img>标签有行内块元素的特点，但是在chrome调试工具中显示结果是inline。在代码中如果行内块标签之间有换行，那么浏览器渲染显示时它们中间会有一个空格。解决这个问题可以用浮动。



## 4、元素显示模式转换

> 除了直接使用不同显示模式的元素来布局，还可以通过指定某元素并指定其元素显示模式来进行布局，即可以让默认的块级元素去显示为行内模式。使用方式为设置元素的display属性。

|         属性         |       效果       | 使用频率 |
| :------------------: | :--------------: | :------: |
|    display:block     |  转换为块级元素  |   较多   |
| display:inline-block | 转换为行内块元素 |   较多   |
|    display:inline    |  转换为行内元素  |   极少   |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            background-color: rgb(242, 255, 210);
            display: inline-block;
            height: 200px;
            width: 100px;
        }
        p{
            background-color: rgb(210, 255, 242);
            display: inline;
        }
        span{
            background-color: #ffb7b7;
            display: block;
        }
    </style>
</head>
<body>
    <div>块级元素div 变行内块</div>
    <p>块级元素p 变行内元素</p>
    <span>行内元素span 变块级元素</span>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/15/Snipaste_2022-11-15_16-16-01.png"/>

## 5、元素嵌套模式

> 即什么元素可以嵌套什么元素。一般来说按元素容器大小块级元素 > 行内块元素 > 行内元素，嵌套也是如此。一般不建议小容器元素嵌套大容器元素。一些其它情况包括p段落标签不要嵌套div、p、h等块级元素，a标签可以嵌套其它任意元素除了再嵌套a标签。



# 六、盒子模型

> 在CSS中，盒子模型可以说是最重要的部分，什么是盒子模型呢？一个div可以是盒子模型，一个span也可以是盒子模型，所以盒子模型可以理解为一个容器，内部用来放置元素，对外可以用来布局，通过盒子布局网页让组件之间的耦合更低，使用更换更方便。

**盒子模型**：

CSS中规定每个盒子自里到外分别由：**内容区域（content）、内边距区域（padding）、边框区域（border）、外边界区域（margin）**构成。

**体验代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 100px;
            width: 100px;
            background-color: rgb(255, 212, 219);
            /* 边框线 */
            border: 1px solid orange;
            /* 内边距 */
            padding: 20px;
            /* 外边距 */
            margin: 20px;

        }
    </style>
</head>
<body>
    <div>一台电脑</div>
    <div>一台电脑</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_11-07-47.png">

## 1、结构

### 1、内容区域 content

> 如果不设置内边距的话，一个标签内部其实都是内容区域。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 100px;
            width: 100px;
            background-color: rgb(255, 209, 217);
        }
    </style>
</head>
<body>
    <div>内容</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_11-22-01.png">



### 2、边框 border

> 字面意思，边框就是盒子的边框，可以设置颜色，宽度等。可以直接通过简写bg + tab键快速生成，默认设置是一起设置盒子四个方向的边框。注意，边框宽度会增大盒子所占区域面积。可以对单独边进行设置，也可以复合写法对多边设置。

写法：**border: 粗细 线条样式 颜色**

> 默认的边框设置方式，对四条边框统一设置。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 100px;
            width: 100px;
            background-color: rgb(255, 224, 229);
            /* 使用格式: border: 粗细 线条样式 颜色 不分先后顺序*/
            /* solid表示实线 dashed表示虚线 dotted表示点线*/
            border: 5px solid orange;
            /* 对单边边框设置在border后加上-方向即可 */
            /* border-left: 5px solid orange; */
        }
    </style>
</head>
<body>
    <div>边框线</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_11-32-02.png">

[^注]: 写边框属性时CSS也提供了对单独某个属性进行设置样式的方法，但一般不用，一般都是复合写法。复合写法要注意，如果只写了一个表示四边都如此，如果写了两个，或者三个，则从上右下左开始对应，剩下的看对面边即可。



### 3、内边距 padding

> 盒子模型中内容和边框的距离称为内边距，注意，内边距也会增大盒子的所占面积。可以单独对某内边距进行设置，也可以复合写法对多边内边距进行设置。

写法：**padding: 20px**

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 200px;
            height: 200px;
            background-color: pink;
            /* 当内边距只写一个时，表示距四边的内边距 */
            /* padding: 30px; */
            /* 内边距可以指定方向写，也可以复合写，复合写四个时，前后对应上右下左 */
            /* padding: 10px 20px 30px 40px; */
            /* 当复合写法为三个时，则对应 上 左右 下 */
            /* padding: 10px 20px 40px; */
            /* 当复合写法为两个时，则对应 上下 左右 */
            padding: 10px 20px;
        }
    </style>
</head>
<body>
    <div class="box">
        盒子
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_14-39-21.png">

[^注]: 在CSS3.0提供了一种设置方式，允许固定盒子大小，设置边框宽度和内边距自动压缩内容区域，保证让设置的盒子大小不会被边框和内边距撑大，使用方式为在盒子中加上属性 box-sizing: border-box;



### 4、外边距 margin

> 外边距，即盒子和上下左右的边距，使用起来和内边距一致。可以单独对某外边距进行设置，也可以复合写法对多边外边距进行设置。

写法：**margin: 20px**

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 100px;
            width: 100px;
            background-color: pink;
            /* 上下左右外边距为20px */
            /* margin: 20px; */
            /* 上下外边距为20px，左右外边距为10px */
            /* margin: 20px 10px; */
            /* 上外边距为10px，左右外边距为20px，下边距为30px */
            margin: 10px 20px 30px;
            /* 上边距 */
            /* margin-top: 20px; */
        }
    </style>
</head>
<body>
    <div>外边距</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_23-01-07.png">

## 2、问题和解决

### 1、清除默认边距

> CSS许多标签在浏览器渲染时会赋予默认内外边距，如p标签、ul标签、li标签等，我们如果要根据自己设计的样式来设置标签的话，就先需要将这些默认边距清除。

清除方法：

1. 并集选择器清除

   ```html
   body,button,dd,dl,dt,h1,h2,h3,h4,h5,h6,hr,input{
   	margin: 0;
   	padding: 0;
   }
   ```

2. 通配符选择器清除

   ```html
   *{
   	margin: 0;
   	padding: 0;
   }
   ```



### 2、版心居中

> 最常用写法，将盒子处于上下居中或者左右居中状态，一般是左右居中。通常是调整盒子外边距，设置好上下边，左右边设置为auto即可。

写法：**margin: 0 auto;**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 1000px;
            height: 200px;
            background-color: pink;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <div>
        版心居中
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/16/Snipaste_2022-11-16_23-21-53.png">

### 3、块级元素外边距

> 浏览器在对**块级元素**的外边距设置进行解析的时候，可能会出现两个现象：外边距合并和塌陷。这两个现象需要及时注意。

#### 外边距合并

> 即两个垂直布局的块级元素之间，都使用外边距隔开时，会只取其中更大的外边距，而不是二者外边距之和。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .upper{
            width: 100px;
            height: 100px;
            background-color: pink;
            margin-bottom: 20px;
        }
        .lower{
            width: 100px;
            height: 100px;
            background-color: pink;
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <div class="upper">上</div>
    <div class="lower">下</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_15-56-24.png">

#### 塌陷

> 塌陷的含义是子块级元素放在父块级元素之中，子元素设置外边距同时会作用在父元素上。解决办法有四种：1、给父级元素设置border属性  2、给父元素设置内边距padding-top  3、给父属性设置overflow: hidden属性  4、将子元素转换为行内块元素

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .father{
            width: 200px;
            height: 200px;
            background-color: rgb(228, 111, 130);
            /* 1、设置border */
            /* border: 1px solid #fff; */
            /* 2、给父属性设置内边距 */
            /* padding-top: 1px; */
            /* 3、给父属性设置overflow: hidden (最好用)*/
            overflow: hidden;
        }
        .son{
            width: 100px;
            height: 100px;
            background-color: rgb(128, 247, 128);
            margin-top: 30px;
            /* 4、将子元素转换为行内块元素 */
            /* display: inline-block; */
        }
    </style>
</head>
<body>
    <div class="father">
        <div class="son">子块</div>
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_16-09-04.png">

### 4、行内元素内外边距

> 行内元素在设置内外边距的时候都只能设置左右边距，不能设置上下边距，如果想要设置上下边距，则可以使用line-height设置。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        span{
            margin: 60px;
            padding: 60px;
            line-height: 100px;
        }
    </style>
</head>
<body>
    <span>span1</span>
    <span>span2</span>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_17-01-26.png">

# 七、浮动

## 1、伪元素

> 伪元素一般是用于页面中的非主体内容部分，可替换部分，并且伪元素是由CSS模拟出来的标签效果。伪元素一共有两个::before和::after。

|  伪元素  |                作用                |
| :------: | :--------------------------------: |
| ::before | 在父元素内容的最前面添加一个伪元素 |
| ::after  | 在父元素内容的最后面添加一个伪元素 |

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .father{
            width: 200px;
            height: 200px;
            background-color: pink;
        }
        .father::before{
            content: '老鼠';
            color: green;
            display: inline-block;
            width: 100px;
            height: 100px;
            background-color: rgb(255, 255, 115);
        }
        .father::after{
            content: '大米';
        }
    </style>
</head>
<body>
    <div class="father">爱</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/17/Snipaste_2022-11-17_18-51-32.png">

[^注]: 伪元素必须设置content属性才能生效，伪元素内容默认是行内元素。



## 2、标准流

> 标准流，又称为文档流，其实就是浏览器渲染标签时的一套排版规则，它规定了什么样的标签以什么样的方式排列元素。

常见的标准流排版规则：

1. **块级元素**：从上往下，垂直布局，独占一行。
2. **行内元素** 或 **行内块元素**：从左往右，水平布局，空间不够时自动折行。



## 3、浮动

> 浮动，字面意思，将元素浮动起来。浮动有两种作用，早期将其用作图文环绕，现在则一般用于网页布局。

特点：

1. 设置浮动的盒子会**脱离浏览器的标准流控制**，浮动元素比标准流元素**高半个级别**，可以**覆盖标准流中的元素**，但是**文字无法覆盖**。
2. 浮动默认是**顶对其**的，浮动元素可以看作更高级别的**行内块元素**，一行可以**显示多个**，也可以**设置宽高**。
3. 浮动级别比设置盒子外边距级别更高，因此浮动只能设置上下边距，左右要么就在左边，要么就在右边，**无法水平居中。**

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .one{
            width: 100px;
            height: 100px;
            background-color: pink;
            float: left;
            /* 上边距生效 */
            margin-top: 30px;
        }
        .two{
            width: 200px;
            height: 200px;
            background-color: skyblue;
            float: left;
            /* 居中不生效 */
            margin: 0 auto;
        }
        .three{
            width: 300px;
            height: 300px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div class="one">one</div>
    <div class="two">two</div>
    <div class="three">three</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/18/Snipaste_2022-11-18_10-25-03.png">



## 4、清除浮动

> 所谓清除浮动，其实是清除浮动带来的影响，有时候根据标准流排列的盒子，可能因为一些属性的改变，比如高度减小，其它盒子也会根据标准流变化位置，但是变化的盒子内部的浮动元素并不受标准流影响，这就有可能影响展示效果，所以需要清除这种浮动所带来的影响，即让浮动元素撑起盒子。

清除浮动的方法共有四种：额外标签法、单/双伪元素法、直接设置法。

### 额外标签法

> 额外标签法通过在需要清除浮动影响的父盒子中添加一个专门用来清除影响的**块级元素**，并给块级元素设置属性 clear:both，来清除浮动来带的影响，这种做法的缺点是会在页面中添加额外的标签，让HTML结构变得更复杂。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            /* height: 200px; */
            width: 800px;
            background-color: #fff;
            margin: 0 auto;
            
        }
        .left{
            height: 200px;
            width: 200px;
            background-color: rgb(255, 220, 245);
            float: left;
        }
        .right{
            height: 200px;
            width: 580px;
            background-color: rgb(207, 255, 176);
            float: right;
        }
        .box2{
            height: 300px;
            width: 1000px;
            background-color: rgb(250, 255, 194);
            margin: 0 auto;
        }
        .clearfix{
            clear:both;
        }
    </style>
</head>
<body>
    <div class="box1">
        <div class="left"></div>
        <div class="right"></div>
        <div class="clearfix"></div>
    </div>
    <div class="box2"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/18/Snipaste_2022-11-18_14-06-30.png">

### 单伪元素法

> 本质上和额外标签法差不多，只是通过CSS伪元素加标签，不用在HTML中加标签。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            /* height: 200px; */
            width: 800px;
            background-color: #fff;
            margin: 0 auto;
            
        }
        .left{
            height: 200px;
            width: 200px;
            background-color: rgb(255, 220, 245);
            float: left;
        }
        .right{
            height: 200px;
            width: 580px;
            background-color: rgb(207, 255, 176);
            float: right;
        }
        .box2{
            height: 300px;
            width: 1000px;
            background-color: rgb(250, 255, 194);
            margin: 0 auto;
        }
        .clearfix::after{
            content:"";
            display: block;
            clear: both;
        }
        /* 为了兼容性，这可写为以下（兼容IE678） */
        /* .clearfix::after{
            content:"";
            display: block;
            clear: both;
            height: 0;
            visibility: hidden;
        } */
    </style>
</head>
<body>
    <div class="box1 clearfix">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="box2"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/18/Snipaste_2022-11-18_14-16-48.png">

### 双伪元素法

> 双伪元素其实和单伪元素差不多，区别在于双伪元素法可以解决外边距塌陷问题。

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            /* height: 200px; */
            width: 800px;
            background-color: #fff;
            margin: 0 auto;
            
        }
        .left{
            height: 200px;
            width: 200px;
            background-color: rgb(255, 220, 245);
            float: left;
        }
        .right{
            height: 200px;
            width: 580px;
            background-color: rgb(207, 255, 176);
            float: right;
        }
        .box2{
            height: 300px;
            width: 1000px;
            background-color: rgb(250, 255, 194);
            margin: 0 auto;
        }
        /* 解决外边距塌陷问题 */
        .clearfix::before,
        .clearfix::after{
            content:'';
            display: table;
        }
        /* 真正清除浮动影响的标签 */
        .clearfix::after{
            clear: both;
        }
    </style>
</head>
<body>
    <div class="box1 clearfix">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="box2"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/18/Snipaste_2022-11-18_14-28-07.png">



### 设置overflow:hidden

> 直接给父元素设置overflow:hidden属性即可。

# 八、定位

> 定位也是CSS网页布局中不可获取的一环，和标准流、浮动并称为网页布局的三种方式。定位主要是通过对元素位置的精准设置来进行布局。定位主要分为三种：相对定位、绝对定位、固定定位。

**常见的应用场景**

1. 定位主要可以解决盒子与盒子重叠的问题，因为定位之后元素的层级最高，可以层叠在其它盒子上面。
2. 可以让盒子始终固定在屏幕中的某个位置。

**使用步骤**

**1、设置定位属性：position**

**常见属性值**

|        定位方式        |  属性值  |
| :--------------------: | :------: |
| 静态定位（等于不定位） |  static  |
|        相对定位        | relative |
|        绝对定位        | absolute |
|        固定定位        |  fixed   |

**2、设置偏移值**

> 偏移值设置分为两个方向，水平和垂直各选一个方向即可，选区的原则一般是就近原则（元素离哪边近用哪个）

| 方向 | 属性名 | 属性值  |      含义      |
| :--: | :----: | :-----: | :------------: |
| 水平 |  left  | 数字+px | 距离左边的距离 |
| 水平 | right  | 数字+px | 距离右边的距离 |
| 垂直 |  top   | 数字+px | 距离上边的位置 |
| 垂直 | bottom | 数字+px | 距离下边的距离 |



## 1、相对定位

> 相对定位是一种根据原先位置进行移动的定位，它除了显示位置，对原先的任何元素都没有影响，该怎么显示就怎么显示。

**属性：position: relative**

**特点**

1. 需要配合方位属性移动位置
2. 移动是相对原先位置
3. 不脱标（脱标是指设置定位后浮动属性是否改变）

**应用场景**

1. 配合绝对定位
2. 用于小范围的移动

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            /* 相对定位的特点：
                 1、仍然占据原有的位置
                 2、仍然具备标签原有的显示模式的特点
                 3、改变位置是参照自己原来的位置
            */
            position: relative;
            left: 150px;
            bottom: 150px;

            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <div class="box">box</div>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/19/Snipaste_2022-11-19_19-53-16.png">

[^注]: 如果在定位中，同时设置 left 和 right，以 left 为准，right 不生效，如果同时设置了 top 和 bottom，以 top 为准，bottom不生效。

 

## 2、绝对定位

> 绝对定位更依靠父级元素，它是相对于非静态定位的父元素进行定位移动。需要注意父级没有定位属性则依靠浏览器可视区域定位。

**属性：position: absolute**

特点

1. 需要配合方位属性实现移动
2. 如果父级元素没有定位，则将浏览器置为父元素，根据浏览器的可视区域左上角进行移动
3. 脱标

代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            /* 绝对定位的特点：
                 1、脱标，不占位置
                 2、可设置宽高，具备行内块特点
            */
            position: absolute;
            left: 150px;
            top: 200px;

            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <div class="box">box</div>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
    <p>测试定位测试定位测试定位测试定位测试定位测试定位测试定位测试定位</p>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/19/Snipaste_2022-11-19_20-06-52.png">

   

[^注1]: 在一般的工作中，如果有嵌套的标签之间需要位置相互绑定，那么一般采取的方式是 “子绝父相” 定位法，即子标签采用绝对定位，父标签采用相对定位，这样，子标签就可以方便的绑定好与父标签之间的位置关系。
[^注2]: 在工作中如果有定位元素要求位移居中，可以采用 left: 50%;  right: 50%; 和 margin-left 进行设置，或者不用 margin-left，而是采用 transform 定位即可。其中：left: 50%: 让盒子的左侧移动到父级元素的水平中心位置；margin-left: -100px: 让盒子向左移动自身宽度的一半。



## 3、固定定位

> 固定在某处的元素，无论网页如何滚动元素都固定显示在浏览器某处的定位方式。

**属性：position: fixed;**

**特点**

1. 需要配合方位属性实现移动
2. 相对于浏览器可视区域进行移动
3. 在页面中不占位置，**已经脱标**，和绝对定位一样具有行内块特点。

**应用场景：让盒子固定在屏幕中的某个位置**

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
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            div{
                position: fixed;
                left: 50px;
                top: 50px;
                width: 100px;
                height: 100px;
                background-color: pink;
            }
        </style>
    </head>
    <body>
        <div class="box"></div>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
        <p>p</p>
    </body>
    </html>
</body>
</html>
```

 <img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/20/Snipaste_2022-11-20_16-58-41.png">

## 4、层级关系

> 显示的层级关系默认是 **定位 > 浮动 > 标准流**，当同时存在多个定位时，默认情况下，写在后面的覆盖前面的。如果要制定上下，则指定属性 z-index: 数字; 即可，数字大的覆盖数字小的，数字默认是 0 ，且这个属性只有和定位配合使用时才生效。

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 150px;
            width: 150px;
        }
        .box1{
            position: absolute;
            left: 50px;
            top: 50px;
            /* 指定 z-index  */
            z-index: 3;
            background-color: rgb(255, 192, 230);
        }
        .box2{
            position: absolute;
            left: 20px;
            top: 20px;
            z-index: 2;
            background-color: rgb(141, 253, 220);
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <!-- 默认情况下定位是写在后面的覆盖前面的 -->
    <div class="box2"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_09-54-15.png">

​                                                                                                                                                                                            

# 九、装饰

> 装饰可以运用的地方很多，对齐、边框样式、阴影、隐藏等。

## 1、垂直对齐

> 浏览器默认对文字类型元素基线对齐，并提供了设置属性 vertical-algin 允许进行设置。基线对齐的使用场景很多，从居中对齐到居顶对齐都可以。

**属性：vertical-algin: middle**

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 150px;
            width: 150px;
            background-color: rgb(255, 192, 234);
            display: inline-block;
            /* 居中对齐 */
            vertical-align: middle;
        }
    </style>
</head>
<body>
    <div>
    </div>
    <button>按钮</button>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_10-18-59.png">

[^注]: 基线可以看作浏览器对文字类型元素排版的用于对齐的线（baseline），浏览器默认基线对齐。行内元素和行内块元素都被当作文字类型处理，具有基线。



## 2、光标类型

> 设置鼠标光标移动到元素上时显示的样式。

**属性名：cursor**

**常见属性值**

| 属性值  |            效果            |
| :-----: | :------------------------: |
| default |     默认值，通常是箭头     |
| pointer | 小手效果，提示用户可以点击 |
|  text   |  工字形，提示用户可以选中  |
|  move   | 十字光标，提示用户可以移动 |

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 50px;
            height: 50px;
            background-color: rgb(195, 255, 252);
            margin-right: 20px;
            display: inline-block;
            vertical-align: top;
        }
        .default{
            cursor: default;
        }
        .pointer{
            cursor: pointer;
        }
        .pointer{
            cursor: text;
        }
        .move{
            cursor: move;   
        }
    </style>
</head>
<body>
    <div class="default"></div>
    <div class="pointer"></div>
    <div class="text">aaa</div>
    <div class="move"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_10-27-50.png">

## 3、圆角边框

> 把边框变为圆角。主要用于提升用户体验，属性是 border-radius，常见取值类型有 数字 + px、百分比，其原理是看圆角占圆的百分比。

**属性：border-radius: 15%**

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            display: inline-block;
            vertical-align: middle;
            margin-right: 20px;
        }
        .one{
            width: 100px;
            height: 100px;
            background-color: rgb(203, 253, 255);
            border-radius: 15%;
        }
        .round{
            width: 100px;
            height: 100px;
            background-color: rgb(203, 253, 255);
            /* 像素代表圆半径 */
            /* border-radius: 50px; */
            /* 百分比代表圆弧度 */
            border-radius: 50%;
        }
        .capsule{
            width: 100px;
            height: 30px;
            background-color: rgb(197, 251, 255);
            border-radius: 15px;
        }
    </style>
</head>
<body>
    <div class="one"></div>
    <div class="round"></div>
    <div class="capsule"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_11-06-04.png">

[^注]: 边框的四角都可以赋值，赋值规则是从左上角开始，顺时针赋值，没赋值的看对角。如果想要一个正圆，设置50%或者宽高一半则表示正圆。如果需要设置为胶囊，则设置px为宽的一半即可。



## 4、溢出显示

> 即对盒子内容溢出部分的显示控制，常见的有显示、隐藏、滚动条等。属性是 overflow。

**属性：overflow: scroll**

| 属性值  |             效果             |
| :-----: | :--------------------------: |
| visible |     默认值，溢出部分可见     |
| hidden  |         溢出部分隐藏         |
| scroll  |  无论是否溢出，都显示滚动条  |
|  auto   | 根据是否溢出，自动显示或隐藏 |

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            height: 60px;
            width: 200px;
            background-color: rgb(192, 254, 255);
            /* overflow-hidden常用来解决外边距塌陷问题 */
            overflow: auto;
        }
    </style>
</head>
<body>
    <div class="box">上有六龙回日之高标,俯身怅惘长惆怅,
        能饮一杯无,独钓寒江雪，孤舟蓑笠翁,夜夜独戚戚,
        三更灯火五更鸡,正是男儿读书时
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_11-17-39.png">

## 5、元素隐藏

> 让某些元素本身在屏幕中不可见，一半是配合鼠标移动或点击使用。常见的属性有 visibility: hidden 或者 display: none。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 150px;
            height: 150px;
        }
        .one{
            /* visibility: 占位隐藏 使用较少*/
            /* visibility: hidden; */
            /* display: 不占位隐藏 最常使用*/
            display: none;
            background-color: rgb(198, 255, 237);
        }
        .two{
            background-color: rgb(211, 255, 211);
        }
    </style>
</head>
<body>
    <div class="one">one</div>
    <div class="two">two</div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_11-24-14.png">

## 6、元素透明

> 让某元素整体（包括）内容一起变透明。属性值是 opacity，取值为 0-1 之间的数字，1 表示完全不透明，0 表示完全透明。

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 300px;
            height: 300px;
            background-color: rgb(255, 202, 202);
            opacity: 0.4;
        }
    </style>
</head>
<body>
    <div>
        <img src="../study/images/course1.png" alt="">
        透明
    </div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_13-34-36.png">

[^注]: opacity 会让元素整体透明，包括元素里面的内容，包括文字，子元素等。



## 7、元素阴影

> 元素阴影分为文字阴影和盒子阴影，作为装饰更加好看，提升用户体验。盒子阴影的属性名为 box-shadow。

**盒子阴影属性：box-shadow: 3px 3px 15px 3px grey;**

**盒子阴影属性取值**

|   参数   |               作用               |
| :------: | :------------------------------: |
| h-shadow | 必写，阴影水平偏移量，允许为负值 |
| v-shadow |   必写，垂直偏移量，允许为负值   |
|   blur   |           可选，模糊度           |
|  spread  |        可选，阴影扩大尺寸        |
|  color   |          可选，阴影颜色          |
|  inset   |     可选，将阴影改为内部阴影     |

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 150px;
            height: 150px;
            background-color: rgb(255, 224, 255);
            border-radius: 15%;
            /* 连写参数为 水平 垂直 模糊度 阴影尺寸 阴影颜色 */
            box-shadow: 3px 3px 15px 3px grey;
        }
        div:hover{
            box-shadow: 3px 3px 10px 3px grey;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_21-12-23.png">

## 8、精灵图

> CSS中精灵图其实就是一张具有多个图标的长图片，使用时作为背景图，通过调整图片坐标露出需要的部分即可，这种做法其实是为了减小服务器请求压力。

使用步骤

1. 创建一个非行内盒子，设置好宽高。
2. 将精灵图作为盒子背景图片。
3. 调整背景图坐标露出需要的部分。

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 20px;
            height: 20px;
            background-color: rgb(255, 213, 245);
            background-image: url("./1.png");
            background-position: 4px -23px;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_20-41-09.png">

## 9、背景图片大小

> 等比方法或缩小背景图片大小。属性名是 background-size。

**属性名：background-size: 20px**

|   取值    |                      场景                      |
| :-------: | :--------------------------------------------: |
| 数字 + px |       扩大或缩小多少像素。简单方便，常用       |
|  百分比   |         按盒子最短边等比扩大或缩小图片         |
|  contain  | 将图片等比放大铺满盒子，图片完整，有可能留白。 |
|   cover   |    图片等比放大，将盒子铺满，图片可能不完整    |

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 300px;
            height: 400px;
            background-color: rgb(255, 215, 250);
            background-image: url("./2.jpg");
            /* 像素放大 */
            background-size: 300px;
            /* 等比方法，以盒子大小为比例 */
            /* background-size: 100%; */
            /* 等比放大直到一边到达盒子边框 */
            /* background-size: contain; */
            /* 等比放大，完全充满盒子 */
            /* background-size: cover; */
        }
    </style>
</head>
<body>
    <div class="box"></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_20-57-06.png">

[^注]: 复合写法就是 background: color image repeat position/size; 需要注意覆盖问题，单独样式写在连写下面。



## 10、过渡

> 让元素的样式顺滑的变化，长配合hover一起使用，增强用户的交互体验。

**属性名：transition: all 1s** 

常见取值

|    参数    |                          取值                           |
| :--------: | :-----------------------------------------------------: |
| 过渡的属性 | all：所有能过渡的属性都过渡；某个具体属性：只过渡该属性 |
| 过渡的时长 |                      数字 + s (秒)                      |

**注意点**

1. 只有默认状态和 hover 状态样式不一样的才有过渡效果
2. transition 属性只给需要过渡的元素本身加
3. transition 如果给默认状态设置，则鼠标移入移出都有过渡效果；如果给 hover 设置，则只有鼠标移入时才有过渡效果，移出时没有过渡效果。

**代码**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 200px;
            height: 150px;
            background-color: rgb(255, 216, 236);
            border-radius: 75px;
            box-shadow: 3px 3px 15px 3px grey;
            background-image: url("./3.gif");
            background-size: contain;
            transition: all 1s;
        }
        div:hover{
            width: 600px;
            height: 150px;
            background-color: rgb(178, 247, 255);
            border-radius: 75px;
            box-shadow: 3px 3px 10px 3px grey;
            background-size: cover;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_21-33-01.png">

## 11、网页图标

> 网页框的图标显示。在head中设置。

**代码**

```html
<link rel="shortcut icon" href="./favicon.ico" type="image/x-icon">
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_22-00-06.png">

## 12、图片模糊

> 将图片模糊处理，效果类似近视时看到的图像。

**代码**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    img {
        /* blur是一个函数，其中填的像素值越大图片越模糊，需要加px单位 */
      filter: blur(2px);
    }

    img:hover {
      filter: blur(0px);
    }
  </style>
</head>

<body>
  <img src="./picture1.jpg" alt="">
</body>

</html>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/16/Snipaste_2023-01-16_21-20-57.png">



# 十、扩展

> 扩展一些前端开发技巧

## 1、去除浏览器默认样式

> 浏览器对一些标签有默认的渲染样式，如标签之间的内外边距。去除它们一般用以下方式即可。

```html
<style>
    *{
        /* 快捷写法: m0+p0 */
        margin: 0;
        padding: 0;
    }
</style>
```

## 2、去除浮动影响

> 项目开发必备，去除盒子变化导致其中浮动元素带来的影响。一般通过伪元素去除。


## 3、SEO

> 即让自己的网站被搜索引擎搜索排名更排前。

**提升方法**

1. 竞价排名
2. 将网页制作成html后缀
3. 标签语义化（在合适地方使用合适标签）

**SEO三大标签**（百度搜索依据）

1. title：网页标题标签
2. description：网页描述标签
3. ketwords：网页关键词标签

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/21/Snipaste_2022-11-21_21-48-25.png">