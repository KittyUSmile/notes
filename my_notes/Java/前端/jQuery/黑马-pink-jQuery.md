# 一、jQuery概述

> JQuery 其实就是一个 JavaScript 的函数库，里面封装好了很多预定义好的函数供使用，可以让我们更方便地去操作 DOM 对象。

## 1、jQuery引入

> jQuery 的引入很简单，先去官网复制精简版（或者阅读版）的 jQuery，在 vscode 中打开文件夹，创建 jquery.min.js，将内容粘贴进去，然后在其它 html 文件中即可。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/27/Snipaste_2022-11-27_16-53-30.png">

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/27/Snipaste_2022-11-27_16-54-43.png">

## 2、入口函数

> jQuery 的入口函数，在 DOM 元素加载完毕后进入。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 引入 jQuery 文件 -->
    <script src="./jquery.min.js"></script>
    <style>
        div{
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div></div>
    <script>
        //等页面DOM加载完毕再执行 JS 代码的两种方法
        // 1、
        // $(document).ready(function(){
        //     $('div').hide();
        // })
        // 2、
        $(function(){
            $('div').hide();
        })
    </script>
</body>
</html>
```



# 二、jQuery的使用

## 1、$ 对象

> jQuery 中的顶级对象，相当于原生 JS 中的 window。利用 $ 符号将元素包装成 jQuery 对象，就可以快捷地使用 jQuery 提供的方法了。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./jquery.min.js"></script>
    <style>
        div{
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div></div>
    <script>
        // 1、$ 其实也是 jQuery 的替换，这两种使用哪种都可以，以下效果相同
        // jQuery(() => {
        //     jQuery('div').hide();
        // })
        $(() => {
            $('div').hide();
        })
    </script>
</body>
</html>
```

[^注]: 通过 jQuery 的 $ 获取到的对象就是 jQuery 对象，通过原生 JS 获取到的对象就是 DOM 对象，这两方对象都只能使用各自对象的方法。

## 2、DOM 对象和 jQuery对象相互转换

> jQuery 对象其实是一个伪数组，封装了 DOM 对象，这为二者需要互相转换时提供了便利。

**DOM 对象转为 jQuery对象**

> 正常来讲通过 jQuery 的方式获取的对象都属于 DOM 对象转为 jQuery 对象的范畴。

**转换方法：$(DOM对象)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="./jquery.min.js"></script>
    <title>Document</title>
</head>
<body>
    <video src="./movie.mp4" muted></video>
    <script>
        //1、DOM 对象转换为 jQuery 对象
        //  1、直接通过 $('') 获取得到的就是 DOM 对象
        // $('video');
        let myvideo = document.querySelector('video');
        //  2、转换原生JS对象为 jQuery 对象
        // $(myvideo);

        //2、jQuery 对象转换为 DOM 对象
        // $('video')[0].play();
        $('video').get(0).play();
    </script>
</body>
</html>
```



## 3、jQuery 选择器

> jQuery 为我们提供了一种统一标准的选择器 $(‘’)，封装了 JS 原先的各种方式的选择器，让我们更好的去选择我们需要的元素。

**隐式迭代**

> 先了解一个概念。jQuery 在选择.元素时具有的一个隐式操作，当通过 jQuery 选择器选择到一批元素，并进行相同的操作时，jQuery 底层会对匹配到的元素进行循环遍历，执行相应的操作，而不用我们再循环，简化我们的操作。

### 1、标签选择器

> 通过标签或者标签的属性选择标签。

**jQuery选择器：$(“选择器”)	**

**选择方式**

|    名称    |      用法       |                          描述                          |
| :--------: | :-------------: | :----------------------------------------------------: |
|  ID选择器  |    $(“#id”)     |                    获取指定ID的元素                    |
| 全选选择器 |     $(“*”)      |                      匹配所有元素                      |
|  类选择器  |   $(“.class”)   |                 获取同一类class的元素                  |
| 标签选择器 |    $(“div”)     |                获取同一类标签的所有元素                |
| 并集选择器 |  $(“div,p,li”)  |                      选取多个元素                      |
| 交集选择器 | $(“li.current”) |                      选择交集元素                      |
| 子代选择器 |   $(“ul>li”)    |      获取亲儿子层级的元素（不会获得孙子层级元素）      |
| 后代选择器 |   $(“ul li”)    | 使用空格，代表后代选择器，获取ul下所有li元素，包括孙子 |



### 2、筛选选择器

> 通过筛选选择出想要的元素。

**选择方式**

|    语法    |     举例      |                     描述                     |
| :--------: | :-----------: | :------------------------------------------: |
|   :first   | $(“li:first”) |               获取第一个li元素               |
|   :last    | $(‘li:last’)  |              获取最后一个li元素              |
| :eq(index) | $(‘li:eq(2)’) | 获取li元素中，索引号为2的元素（下标从0开始） |
|    :odd    |  $(‘li:odd’)  |   获取到的li元素中，选择索引号为奇数的元素   |
|   :even    | $(‘li:even’)  |   获取到的li元素中，选择索引号为偶数的元素   |



### 3、jQuery 筛选方法

> 通过 jQuery 提供的方法筛选出想要的元素。

|        语法        |            举例            |                    说明                    |
| :----------------: | :------------------------: | :----------------------------------------: |
|      parent()      |     $(‘li’).parent();      |           查找自己的直接父级元素           |
|     parents()      |  $(‘li’).parents(‘.one’);  |     查找所有父级元素中class为one的元素     |
| children(selector) |  $(‘ul’).children(‘li’);   | 相当于$(‘ul>li’)，查找最亲一级的儿子li元素 |
|   find(selector)   |    $(‘ul’).find(‘li’);     |    相当于$(‘ul li’)，选择后代中的所有li    |
| siblings(selector) | $(‘.first’).siblings(‘li’) |       查找所有li兄弟节点，不包括自己       |
|  nextAll([expr])   |   $(‘first’).nextAll();    |       查找当前元素之后的所有同辈元素       |
|  prevtAll([expr])  |   $(‘.last’).prevAll();    |       查找当前元素之前的所有同辈元素       |
|  hasClass(class)   |   $(.div).hasClass(‘a’)    |   查找当前元素是否含有类 a，有则返回true   |
|     eq(index)      |       $(‘li’).eq(2)        |        查找li元素数组中索引为2的li         |

重点：**parent()、children()、find()、siblings()、eq()**



## 4、jQuery 样式操作

> 通过 jQuery 操作元素的 CSS 样式。分为两种设置方式，一种是直接在 css 属性中设置，一种是通过在 style 属性或 .css 文件中设置，再通过 jQuery 添加类进标签的方式设置。

### 1、直接设置

> 耦合较高。

**效果和使用**

**1、参数只写属性名，则是返回属性值**

```js
$('div').css('backgroundColor')		// 结果: rgb(255, 192, 203)
```

**2、参数是属性名、属性值，使用逗号分隔。**

> 设置样式操作，属性必须加引号，值如果是数字可以不用根单位和引号，jQuery会为我们处理。

```js
$('div').css('backgroundColor', 'grey');
$('div').css("width", 150);
```

**3、参数为对象形式，设置多组样式**

> 批量设置样式，属性名和属性值用冒号隔开，属性可以不用加引号，属性名用驼峰命名法。

```js
$('div').css({
    'width': 300,
    height: 300,
    'backgroundColor': 'skyblue'
})
```



### 2、通过类设置

> 减少耦合，推荐使用。注意操作类里面的参数不要加点。

**1、添加类 $(this).addClass('className');**

> 在原先类基础上追加。

```html
<script src="./jquery.min.js"></script>
<style>
    .current{
        background-color: rgb(177, 241, 255);
    }
    div{
        width: 200px;
        height: 200px;
        margin: 0 auto;
        background-color: rgb(255, 207, 232);
    }
</style>

<body>
    <div></div>
    <script>
        $(()=>{
            // 添加类
            $('div').click(function(){
                $(this).addClass('current');
            })
        })
    </script>
</body>
```

**2、移除类 $(this).removeClass('className');**

```js
// 移除类
$('div').click(function(){
    $(this).removeClass('current');
})
```

**3、切换类 $(this).toggleClass(‘className’)**

```js
$('div').click(function(){
    $(this).toggleClass('current');
})
```



## 5、jQuery 效果

> jQuery 为我们封装了很多动画效果，如显示隐藏、滑动、淡入淡出、自定义动画等。

**显示隐藏**

- **show()**
- **hide()**
- **toggle()**

**滑动**

- **slideDown()**
- **slideUp()**
- **slideToggle()**

**淡入淡出**

- **fadeIn()**
- **fadeOut()**
- **fadeToggle()**
- **fadeTo()**

**自定义动画**

- **animate()**



### 1、显示隐藏效果

> 让元素显示隐藏。

#### show()

> 显示元素

**语法：show( [ [speed], [easing], [fn] ] )**

**参数含义**

> 这些参数都是可以省略的，无动画直接显示

1. **speed**，有三种字符串可选速度：slow、normal、fast；或者直接写毫秒数值（如：1000）
2. **easing**：用来指定切换效果，默认是 “swing”，可用参数 “linear”。
3. **fn**：回调函数，在动画执行完毕后执行的函数，每次 show() 方法执行完后执行一次。

```js
$('div').show(500);
```



#### hide()

> 隐藏元素，hide() 语法和 show() 一样，参考 show() 即可。

**语法：hide( [ [speed], [easing], [fn] ] )**

```js
$('div').hide(500);
```



#### toggle()

> 来回切换元素，语法和 show() 一样。

**语法：toggle( [ [speed], [easing], [fn] ] )**

```js
$('div').toggle(500);
```



### 2、上下滑动效果

> 让元素上下滑动显示。

#### slideDown()

> 下滑效果显示

**语法：slideDown( [ [speed], [easing], [fn] ] )**

**参数含义**

> 这些参数都是可以省略的，无动画直接显示

1. **speed**，有三种字符串可选速度：slow、normal、fast；或者直接写毫秒数值（如：1000）
2. **easing**：用来指定切换效果，默认是 “swing”，可用参数 “linear”。
3. **fn**：回调函数，在动画执行完毕后执行的函数，每次 show() 方法执行完后执行一次。

```js
$('div').slideDown(500);
```



#### slideUp()

> 上滑效果显示，语法和 slideDown() 一样。

**语法：slideUp( [ [speed], [easing], [fn] ] )**

```js
$('div').slideUp(500);
```



#### slideToggle()

> 下滑上滑效果切换显示，语法和 slideDown() 一样。

**语法：slideToggle( [ [speed], [easing], [fn] ] )**

```js
$('div').slideToggle(500);
```



### 3、淡入淡出效果

> 让元素的显示呈淡入淡出效果。

#### fadeIn()

> 元素淡入效果

**语法：fadeIn( [ [speed], [easing], [fn] ] )**

**参数含义**

> 这些参数都是可以省略的，无动画直接显示

1. **speed**，有三种字符串可选速度：slow、normal、fast；或者直接写毫秒数值（如：1000）
2. **easing**：用来指定切换效果，默认是 “swing”，可用参数 “linear”。
3. **fn**：回调函数，在动画执行完毕后执行的函数，每次 show() 方法执行完后执行一次。

```js
$('div').fadeIn(500);
```



#### fadeOut()

> 元素淡出效果，语法和 fadeIn() 一样。

**语法：fadeOut( [ [speed], [easing], [fn] ] )**

```js
$('div').fadeOut(500);
```



#### fadeToggle()

> 来回切换元素淡入淡出效果，语法和 fadeIn() 一样。

**语法：fadeToggle( [ [speed], [easing], [fn] ] )**

```js
$('div').fadeToggle(500);
```



#### fadeTo()

> 调整到指定透明度。

**语法：fadeTo( [ [speed], opacity, [easing], [fn] ] )**

参数含义

> speed 和 opacity 参数不能省略，其中 opacity 是透明度，取值为 0 - 1

1. **speed**：有三种字符串可选速度：slow、normal、fast；或者直接写毫秒数值（如：1000）
2. **opacity**：透明度，取值为 0 - 1（如 0.5 或 .5）
3. **easing**：用来指定切换效果，默认是 “swing”，可用参数 “linear”。
4. **fn**：回调函数，在动画执行完毕后执行的函数，每次 show() 方法执行完后执行一次。

```js
$('div').fadeTo(500, 0.5);
```



### 4、自定义动画

> 即自定义动画效果和标签的属性变化。

**语法：animate( params, [speed], [easing], [fn] )**

**参数含义**

1. **params**：想要更改的样式属性，以对象形式传递，必须写，属性名可以不用带引号，有些属性需要采用驼峰命名法。
2. **speed**：有三种字符串可选速度：slow、normal、fast；或者直接写毫秒数值（如：1000）
3. **easing**：用来指定切换效果，默认是 “swing”，可用参数 “linear”。
4. **fn**：回调函数，在动画执行完毕后执行的函数，每次 show() 方法执行完后执行一次。

```js
$('div').animate({
    right: 400,
    top: 300,
    width: 300,
    height: 100,
    opacity: .4
}, 200);
```



### 5、hover 移入移出简化

> jQuery 还提供了 hover 鼠标移入移出写在一起操作，简便我们的使用。其中 hover 第一个参数为鼠标移入的函数操作，第二个参数为鼠标移出的函数操作。

**语法：$(‘div’).hover(function(){}, function(){})**

```js
$('div').hover(
    //鼠标移入时执行的函数
    function(){
        $(this).addClass('bgc');
    }, 
    //鼠标移出时执行的函数
    function(){
        $(this).removeClass('bgc');
    });
```

**jQuery还进行了继续简化，如果只写一个函数，那么移入移出都会执行函数。**

```js
$('div').hover(
    //鼠标移入移出时执行的函数
    function () {
        $(this).toggleClass('bgc');
})
```



### 问题及解决

> 解决一些常见问题。

#### 1、动画排队

> jQuery 有些效果通过有时长的动画展示，如果短时间内鼠标多次触发，则触发展示的动画会进入动画队列持续展示，而我们只需要鼠标持续在时才继续展示，移开了就不再展示，则可以通过jQuery提供的 stop() 方法直接结束上一次的动画展示。

**语法：$(this).stop().slideToggle()**

[^注]: stop() 方法要写在动画展示方法的前面。



## 6、jQuery 属性操作

> 对标签的属性进行操作，可以分为对固有属性和自定义属性的操作。

### 1、固有属性值

> 对标签本身具有的属性进行操作，比如 a 标签具有的 href 属性。

#### 获取属性

**语法：$(‘选择器’).prop(‘属性’);**

```js
$('a').prop('href');
// 复选框
$('input').prop("checked");
```



#### 设置属性

**语法：$(‘选择器’).prop(“属性”, “属性值”)**

```js
$('a').prop('href',"https://www.sina.com.cn/");
```

[^注]: 如果要获取被选中的复选框，jQuery 提供了一种选择器：$(‘元素:checked’) 



### 2、自定义属性值

> 对元素的自定义属性进行操作，自定义属性值通过 attr() 方法进行获取和设置。

#### 获取属性

**语法：$(‘选择器’).attr(“属性”);**

```html
<body>
    <a index="1">超链接</a>
    <script>
        $(()=>{
            console.log($('a').attr('index'));
        })
    </script>
</body>
```



#### 设置属性

**语法：$(‘选择器’).attr(“属性”, “属性值”);**

```html
<body>
    <a index="1">超链接</a>
    <script>
        $(()=>{
            $('a').attr('index',"4");
        })
    </script>
</body>
```



### 3、数据缓存 data

> 通过数据缓存也可以对元素的属性进行操作，只不过数据缓存设置的属性不会显示在 DOM 页面中，就相当于是一个不显示，但存在的属性。

#### 获取属性

**语法：$(‘选择器’).data(“属性”);**

```js
$('a').data("key");
```



#### 设置属性

**语法：$(‘选择器’).data(“属性”, “属性值”);**

```js
$('a').data('key',"value");
```

[^注]: 通过 data() 获取 h5 自定义属性时，就不需要加 data- 了，而且返回的数据是数字型。



## 7、jQuery 文本属性值

> jQuery 提供了和原生 JS 类似的方法供我们对文本属性值进行操作，如 html()、text()、val()

### html()

> 获取和设置元素内容（包含标签）

**获取元素内容**

**语法：$(‘元素’).html();**

```html
<body>
    <div>
        <span>内容</span>
    </div>
    <script>
        $(()=>{
            console.log($('div').html());
        })
    </script>
</body>
```

**设置元素内容**

**语法：$(‘元素’).html(“内容”);**

```html
<body>
    <div>
        <span>内容</span>
    </div>
    <script>
        $(()=>{
            $('div').html("<p>p标签</p>");
        })
    </script>
</body>
```



### text()

> 获取和设置元素的文本内容（只包含内容）

**获取元素文本内容**

**语法：$(‘元素’).text()**

```html
<body>
    <div>
        <span>内容</span>
    </div>
    <script>
        $(()=>{
            console.log($('div').text());
        })
    </script>
</body>
```

**设置元素文本内容**

**语法：$(‘元素’).text(“内容”)**

```html
<body>
    <div>
        <span>内容</span>
    </div>
    <script>
        $(()=>{
            $('div').text("内容修改");
        })
    </script>
</body>
```



### val()

> val() 用于获取表单内的值

**获取表单内容**

**语法：$(‘元素’).text()**

```html
<body>
    <input type="text" value="请输入内容"></input>
    <script>
        $(()=>{
            console.log($('input').val());
        })
    </script>
</body>
```

**设置表单内容**

**语法：$(‘元素’).text(‘内容’)**

```html
<body>
    <input type="text" value="请输入内容"></input>
    <script>
        $(()=>{
            $('input').val("默认内容");
        })
    </script>
</body>
```



## 8、jQuery 元素操作

> 主要是对选择器选择出来的一批元素进行遍历、创建、添加或删除操作。

### 1、遍历元素

> 如果只想对一批元素进行相同操作，则直接选择进行操作即可，隐式迭代会为我们完成迭代工作，但是如果想要循环进行不同操作，jQuery 则为我们提供了 each() 方法，each() 方法有两种用法。

**语法1：$(‘ 选择器 ’).each( function( index,  domEle) {} )**

**说明**

1. each() 方法遍历匹配的每一个元素，产生的对象是 **DOM 对象**。
2. 回调函数中的两个参数：**index 表示每个元素的索引号**，**domEle 表示每个 DOM 元素对象**，并不是 jQuery 对象。如果想要使用 jQuery 方法，可以将 DOM 对象转换为 jQuery 对象：**$(domEle)**

```html
<body>
    <h3>1</h3>
    <h3>2</h3>
    <h3>3</h3>
    <script>
        $(()=>{
            var arr = ['skyblue', '#c7ffe4', '#ffc7cc']
            var sum = 0;
            $('h3').each(function(index, domEle){
                $(domEle).css('color', arr[index]);
                sum += Number.parseInt($(domEle).text());
            })
            console.log(sum);
        })
    </script>
</body>
```

**语法2：$.each($(‘ 选择器 ’), function(i, ele) {})**

> 这种语法主要用于遍历数据，处理数据的。当然传入的不一定是选择器，也可以传入一个数组或者一个对象。遍历数组时，i为下标，ele为元素。遍历对象时，i为属性名，ele为属性值。

```html
<body>
    <h3>1</h3>
    <h3>2</h3>
    <h3>3</h3>
    <script>
        $(() => {
            var arr = ['skyblue', '#c7ffe4', '#ffc7cc']
            var sum = 0;
            $.each($('h3'), function (index, domEle) {
                $(domEle).css('color', arr[index]);
                sum += Number.parseInt($(domEle).text());
            })
            console.log(sum);
        })
    </script>
</body>
```



### 2、创建元素

> 创建出元素并放置在不同位置。放置可以分为内部添加和外部添加，内部添加添加成儿子，外部添加添加成兄弟。

**内部添加**

1. $("选择器").append("内容");		 //把内容放入目标元素内部的后面
2. $("选择器").prepend("内容");      //把内容放入目标元素内部的前面

**外部添加**

1. $("选择器").before("内容");       //把内容放入目标元素前面
2. $("选择器").after("内容");        //把内容放入目标元素后面

```html
<body>
    <ul>
        <li>原先的li</li>
    </ul>
    <div class="test">原先的div</div>
    <script>
        $(()=>{
            // append 加在儿子最后
            $('ul').append('<li>后来的li,追加在后面</li>')
            // prepend 加在儿子前面
            $('ul').prepend('<li>后来的li,追加在前面</li>')
            
            // before 加在自己前面
            $('.test').before('<div>加在前的div</div>')
            // after 加在自己后面
            $('.test').after('<div>加在后的div</div>')
        })
    </script>
</body>
```



### 3、删除元素

> 删除指定的元素。

**删除语法**

1. $("选择器").remove()	//删除匹配的元素
2. $("选择器").empty()	  //删除匹配的元素中的子节点
3. $("选择器").html(“”)       //清空匹配的元素内容

```html
<body>
    <p>p元素</p>
    <ul>
        <li>li1</li>
        <li>li2</li>
        <li>li3</li>
    </ul>
    <div>我的内容</div>
    <script>
        $(()=>{
            // 删除匹配到的元素
            $('p').remove();

            // 删除匹配到的元素的子节点
            $('ul').empty();

            // 清空匹配的元素的内容
            $('div').html("");
        })
    </script>
</body>
```



## 9、jQuery 事件

### 1、事件注册

> jQuery 为我们提供了一种 on 方式进行事件注册，原来我们可以使用 onclick、mouseover 等方法注册事件，但是现在使用 on 让事件注册更加方便。on 事件注册有两种语法。

**语法1**

```js
$('选择器').on({
    mouseenter:function(){},
    click:function(){},
    mouseleave:function(){}
})
```

**语法2（当事件相同时）**

```js
$('选择器').on("mouseenter click mouseleave", function(){})
```

### 2、事件处理

> on 还可以进行事件的处理，并且优势相对较多，比如可以进行事件委托（委派），将定位在子类身上的事件绑定在父类身上，这样就可以操作未来动态创建的元素，原来通过隐式迭代的方式无法做到对未来动态元素操作的功能。

**事件委托语法（委派）**

```js
$('选择器').on('操作','子类标签',function(){})
```

```html
<body>
    <ul>
        <li>这是第1个li</li>
        <li>这是第2个li</li>
        <li>这是第3个li</li>
    </ul>
    <script>
        $(()=>{
            $('ul').on('click', 'li', function(){
                console.log(11);
            })
            // 动态新增的li,上面绑定的点击事件对动态新增的元素依然生效
            $('ul').append("<li>这是新增的li</li>")
        })
    </script>
</body>
```

### 3、事件解绑

> jQuery 提供了事件解除绑定的方法 off()

**语法**

```js
$('p').off()	// 解绑 p 元素所有事件处理程序

$('p').off('click')		// 解除 p 元素上的 click 事件

$('ul').off('click','li');		// 解除事件委托
```





## jQuery尺寸、位置操作





## 其它操作

### 1、计算结果保留n位小数

**用法：(x * y).toFix(n);**

```js
let val = (2.60 * 2).toFixed(2);
console.log(val)	// 结果: 5.20
```

