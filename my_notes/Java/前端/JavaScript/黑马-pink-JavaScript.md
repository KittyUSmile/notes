
# ECMAScript

> JavaScript 的组成部分之一，是 JavaScript 的基础语法，由 ECMAScript 标准规定，学习这部分主要是为了后面的课程打下基础。 



# DOM 

> DOM （文档对象模型，Document Object Model）和 BOM 是 W3C 组织推荐的的标准编程接口，也是 JavaScript 的进阶部分，DOM 主要用于对页面进行交互，可以通过W3C提供的DOM接口改变网页的内容，解构和样式，这部分需要 JavaScript 基础作铺垫。DOM 通过 Web API（浏览器提供的接口）操作页面元素。

如果想查看最全的 DOM API，请转到 ：https://developer.mozilla.org/zh-CN/docs/Web/API/Document

## DOM树

> DOM 树是 DOM 结构的一种展示，看起来像棵树因此被称为 DOM 树。

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/5/2023-1-5-14-05.jpg">

-  文档：一个页面就是一个文档（document），DOM 中使用 document 来表示
-  元素：页面中所有标签都是元素（element），DOM 中使用 element 来表示
-  节点：网页中所有内容都是节点（标签、属性、文本、注释等），DOM 中使用 node 表示

## 获取元素

> 获取元素也是 DOM 的主要功能之一，通过 DOM 获取元素常用的方式有：1、根据ID获取；2、根据标签名获取；3、通过HTML5新增的方法获取；4、通过特殊元素获取。

### getElementById

> DOM 提供的通过元素 id 获取元素的方式，成功取回的返回结果是 Object，失败则是 null，该方法需要传入元素 id 作为参数。

```html
<body>
  <div id="time">2023-1-5</div>
  <script>
    var v1 = document.getElementById('time');
    console.log(v1);  // 打印 <div id="time">2023-1-5</div>
    console.log(typeof v1);  // 打印 object
  </script>
</body>
```

### getElementByTagName

> 通过标签来获取元素，成功取回的返回结果是元素对象的集合，并以伪数组（伪数组：有长度，有索引，但是没有 push 等方法）形式存储。

```html
<body>
  <li>这是第1个li</li>
  <li>这是第2个li</li>
  <li>这是第3个li</li>
  <li>这是第4个li</li>
  <li>这是第5个li</li>
  <script>
    var v1 = document.getElementsByTagName('li');
    console.log(v1);// 结果 : HTMLCollection(5) [li, li, li, li, li]
  </script>
</body>
```

如果只想获取某个元素下的某些元素，可以采用 element.getElementByTagName() 的方式：

```html
<body>
  <ol id="ol">
    <li>这是第1个li</li>
    <li>这是第2个li</li>
    <li>这是第3个li</li>
    <li>这是第4个li</li>
    <li>这是第5个li</li>
  </ol>
  <li>这是6</li>
  <li>这是7</li>
  <li>这是8</li>
  <script>
    var v1 = document.getElementById('ol').getElementsByTagName('li');
    // 结果 : HTMLCollection(5) [li, li, li, li, li]
    console.log(v1);
  </script>
</body>
```

### getElementByClassName

> 通过类名获取元素，成功取回的返回结果是元素对象的集合，该方法需要传入类名作为参数。

```html
<body>
  <div class="box">盒子1</div>
  <div class="box">盒子2</div>
  <script>
    var v1 = document.getElementsByClassName('box');
    // 结果 : HTMLCollection(2) [div.box, div.box]
    console.log(v1);
  </script>
</body>
```

### querySelector

> 根据指定的**选择器**获取到第一个元素，选择器可以是类选择器、id选择器等。

```html
<body>
  <div class="box">盒子1</div>
  <div class="box">盒子2</div>
  <p id="p1">p标签</p>
  <script>
    var v1 = document.querySelector('.box');
    console.log(v1);// 结果: <div class="box">盒子1</div>
  </script>
</body>
```

和这个方法类似的还有 **querySelectorAll()**，这个方法也是根据指定的选择器获取到对应元素，不过其会获取全部对应的元素对象，并以伪数组形式返回：

```html
<body>
  <div class="box">盒子1</div>
  <div class="box">盒子2</div>
  <p id="p1">p标签</p>
  <script>
    var v1 = document.querySelectorAll('.box');
    console.log(v1);// 结果: NodeList(2) [div.box, div.box]
  </script>
</body>
```

### body

>  获取整个 body 标签元素。获取方式为 **document.body**

```html
<body>
  <div>body</div>
  <script>
    var v1 = document.body;
    console.log(v1); // 输出 <body></body> 中的内容
  </script>
</body>
```

### documentElement

>  获取整个 html 标签元素，获取方式为 **document.documentElement**

```html
<body>
  <script>
    var v1 = document.documentElement;
    console.log(v1);// 输出整个html中的内容
  </script>
</body>
```

## 操作元素

> 操作元素可以修改元素内容或者元素样式，修改元素内容常用的方法有 element.innerText 和 element.innerHTML。

### innerText

> 获取 / 修改元素内容，innerText 不识别 HTML 元素，一切都是字符串，innerText 是非标准的，使用频率不及 innerHTML。通过 innerText 获取元素内容时会去除空格和换行。

```html
<body>
  <p>p内容
    <span>span</span>
  </p>
  <button id="btn1">按钮</button>
  <script>
    var btn1 = document.querySelector('#btn1');
    btn1.onclick = () => {
      // 去除空格和换行
      console.log(document.querySelector('p').innerText);
      document.querySelector('p').innerText = '修改后的内容';
    }
  </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/5/GIF%202023-1-5%2016-52-34.gif">

### innerHTML

> 修改元素内容，innerHTML 会识别 HTML 元素，会将其中的 HTML 元素进行渲染，innerHTML 是 W3C 标准，使用比 innerText 多。通过 innerHTML 获取元素内容时不会去除空格和换行。

```html
<body>
  <p>p内容
    <span>span</span>
  </p>
  <button id="btn1">按钮</button>
  <script>
    var btn1 = document.querySelector('#btn1');
    btn1.onclick = () => {
      // 去除空格和换行
      console.log(document.querySelector('p').innerHTML);
      document.querySelector('p').innerHTML = '<strong>修改后的内容</strong>';
    }
  </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/5/GIF%202023-1-5%2016-53-59.gif">

### 元素属性操作

> 获取元素后对元素的属性进行修改。

常用的元素属性有：src、href、id、alt、title、style等，使用时通过 **element.元素属性** 即可。

```html
<body>
  <button id="btn1">刘德华</button>
  <button id="btn2">张学友</button>
  <hr>
  <img src="./liudehua.jpg" title="刘德华" style="height: 200px;">
  <script>
    var btn1 = document.getElementById('btn1');
    var btn2 = document.getElementById('btn2');
    var img = document.querySelector('img');
    btn1.onclick = () => {
      img.src = './liudehua.jpg'
      img.title = '刘德华'
      img.style.height = '200px'
    }
    btn2.onclick = () => {
      img.src = './zhangxueyou.jpg'
      img.title = '张学友'
      img.style.height = '200px'
    }
  </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/5/GIF%202023-1-5%2017-10-08.gif">

### 表单属性操作

> 表单（ input标签）里的属性操作不能直接通过 innerText 或者 innerHTML 进行，表单有其特殊的属性操作方式。通过 DOM 可以操作表单中的如下属性：type、value、checked、selected、disabled。

```html
<body>
  <button id="btn1">按钮</button>
  <input id="text1" type="text" value="....">
  <script>
    var btn1 = document.getElementById('btn1');
    var t1 = document.getElementById('text1');
    btn1.onclick = () => {
      t1.value = 'xxx'
      btn1.disabled = true
    }
  </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/5/GIF%202023-1-5%2017-31-53.gif">

### 样式属性操作

> 通过 DOM 也可以修改元素的样式，如大小、颜色、位置等，一般通过 **element.style.样式属性** 来修改元素的样式，这种方式修改样式生成的是行内样式。只要是能设置的样式属性都能通过 DOM 的这种方式设置。

```html
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    div {
      width: 200px;
      height: 200px;
      background-color: rgb(156, 251, 236);
    }
  </style>
</head>

<body>
  <div id="div1"></div>
  <script>
    var v1 = document.getElementById('div1');
    v1.onclick = () => {
      v1.style.backgroundColor = "rgb(185, 251, 156)";
      v1.style.width = '300px';
    }
  </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2023/1/9/GIF%202023-1-9%209-53-43.gif">
如果需要变化的样式属性很多，通过 element.style 一个个写很麻烦，可以采用 element.className = 'xx' 来修改组件的 class 属性，赋予新样式。

```html
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    div {
      width: 200px;
      height: 200px;
      background-color: rgb(156, 251, 236);
    }

    .style1 {
      background-color: rgb(185, 251, 156);
      width: 300px;
      cursor: 'pointer';
      transition: all 1s;
    }
  </style>
</head>

<body>
  <div id="div1"></div>
  <script>
    var v1 = document.getElementById('div1');
    v1.onclick = () => {
      v1.className = 'style1'
    }
  </script>
</body>
```

## 事件

> 事件可以理解为触发-响应机制，它是一种能够被 JS 侦测到行为，网页中每个元素都可以触发某些事件。

事件由三部分组成：**事件源、事件类型、事件处理程序**。事件源指的是被触发的对象，比如一个按钮；事件类型是指触发了什么事件，比如被点击了；事件处理程序是指如何处理监听到的行为，比如编写一个函数弹出框。

```html
<body>
  <button id="btn1">按钮</button>
  <script>
    var btn1 = document.getElementById('btn1');
    btn1.onclick = () => {
      alert('被点击了')
    }
  </script>
</body>
```

### 常见鼠标事件

|  鼠标事件   |     触发事件     |
| :---------: | :--------------: |
|   onclick   | 鼠标点击左键触发 |
| onmouseover |   鼠标经过触发   |
| onmouseout  |   鼠标离开触发   |
|   onfouse   | 获得鼠标焦点触发 |
|   onblur    | 失去鼠标焦点触发 |
| onmousemove |   鼠标移动触发   |
|  onmouseup  |   鼠标弹起触发   |
| onmousedown |   鼠标按下触发   |



# BOM

