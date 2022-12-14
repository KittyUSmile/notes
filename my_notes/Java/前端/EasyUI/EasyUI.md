# EasyUI入门

> EasyUI 其实是一个封装了大量的 jQuery 插件的框架，可以用来简化 ajax、jquery 的操作，并且它内部提供的部分标签自带了 CSS 样式。

## 1、学习重点

> 即学习 EasyUI 需要重点关注和掌握的部分，分为 插件实现 和 插件内容。

**插件声明**

> 即让一个标签变成 EasyUI 插件的方法，有两种声明方法，class样式 和 编程声明。

```html
1、HTML标签 + class样式
<a href="#" class="easyui-linkbutton">我是EasyUI按钮</a>

2、HTML标签 + 基于编程实现
<a href='#' id='btn'>我是EasyUI按钮</a>
<script>
	$('选择器').插件名();
</script>
```

**插件实现**

> 即一个 EasyUI 插件具有的属性、事件和方法，每一个 EasyUI 插件都有。

```apl
属性: 插件的样式、图标、宽度，高度。
事件: 单击事件，双击事件等。
方法: 修改插件属性，状态等。
```



## 2、环境搭建

> 先去官网下载 EasyUI 文件。

https://www.jeasyui.net/download

### 1、资源文件介绍

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/30/Snipaste_2022-11-30_15-43-10.png">



### 2、环境搭建步骤

> 步骤：1、导入需要的css样式文件：easyui.css、icon.css、icons、images。2、导入相关的 js 文件：jquery.min.js、jquery.easyui.min.js、jquery.easyui-lang-zh_CN.js。以下引入按顺序引入。

```html
<link rel="stylesheet" href="../easyui.css">
<link rel="stylesheet" href="../icon.css">
<script src="../jquery.min.js"></script>
<script src="../jquery.easyui.min.js"></script>
<script src="../easyui-lang-zh_CN.js"></script>
```



# 插件

> 这里展示比较常用的一些插件。这部分建议综合视频中的重点插件去官网查阅。
>
> https://www.jeasyui.cn/document/index/index.html

## 按钮 linkbutton

> EasyUI 提供的按钮插件。

### 声明

**声明1：\<a  href="#"  class="easyui-linkbutton">xxxx\</a>**

**声明2：$('#btn').linkbutton();**

```html
<body>
    <!-- EasyUI具有的两种声明方式 -->
    <!-- 1、标签声明 -->
    <a href="#" class="easyui-linkbutton">按钮超链接1</a>
    
    <!-- 2、绑定声明 -->
    <a href="#" id="btn">按钮超链接2</a>
    <script>
        $('#btn').linkbutton();
    </script>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/30/Snipaste_2022-11-30_16-25-36.png">

### 属性

> 通过 data-options=“属性名:属性值, 属性名:属性值…” 设置属性，或者通过编程方式设置属性。

```html
<body>
    <!-- 1、标签声明属性 -->
    <a href="#" class="easyui-linkbutton" data-options="iconCls:'icon-search',size:'large'">按钮超链接1</a>
    
    <!-- 2、编程声明属性 -->
    <a href="#" id="btn">按钮超链接2</a>
    <script>
        $('#btn').linkbutton({
            iconCls:'icon-cancel',
            size:'large'
        });
    </script>
</body>
```

### 事件

> 事件在 EasyUI 中相当于属性，写也是写在 data-options 中。

```html
<body>
    <!-- 1、标签声明属性 -->
    <a href="#" class="easyui-linkbutton" data-options="
            iconCls:'icon-search',
            size:'large',
            onClick:clickEvent">按钮超链接1</a>

    <!-- 2、编程声明属性 -->
    <a href="#" id="btn">按钮超链接2</a>
    <script>
        $('#btn').linkbutton({
            iconCls:'icon-cancel',
            size:'large',
            onClick:clickEvent
        });
        function clickEvent(){
            alert("点击..");
        }
    </script>
</body>
```



### 方法

```html
<body>
    <!-- 2、编程声明属性 -->
    <a href="#" id="btn">按钮超链接2</a>
    <script>
        $('#btn').linkbutton({
            iconCls:'icon-cancel',
            size:'large',
            onClick:function(){
                // 调用方法
                $('#btn').linkbutton('resize',{width:300,height:100});
            }
        });
        function clickEvent(){
            alert("点击..");
        }
    </script>
</body>
```



## 面板 panel

> easyui 提供的面板框。

```html
<body>
    <div id="p" class="easyui-panel" title="面板panel" style="width:500px;height:150px;padding:10px;
        background:#fafafa;" data-options="iconCls:'icon-save',closable:true,collapsible:true,minimizable:true,maximizable:true">
        <p>panel content.</p>
        <p>panel content.</p>
    </div>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/30/Snipaste_2022-11-30_17-27-31.png">

## 窗口 window

> 窗口，继承于 panel 

```html
<body>
    <a class="easyui-linkbutton">按钮</a>
    <div id="win" class="easyui-window" title="My Window" style="width:600px;height:400px" data-options="iconCls:'icon-save',modal:true,shadow:true">
    <div class="easyui-layout" data-options="fit:true">
        <div data-options="region:'north',split:true" style="height:100px"></div>
        <div data-options="region:'center'">
            The Content.
        </div>
    </div>
</div>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/30/Snipaste_2022-11-30_18-35-06.png">



## 对话框 dialog

> 对话框，继承于 window 和 linkbutton。

```html
<body>
    <div id="dd" class="easyui-dialog" title="My Dialog" style="width:400px;height:200px;" data-options="iconCls:'icon-save',resizable:true,modal:true">
        Dialog Content.
    </div>
</body>
```

<img src="https://run-notes-pictures.oss-cn-hangzhou.aliyuncs.com/2022/11/30/Snipaste_2022-11-30_18-50-39.png">



## 消息框 messager

> 不需要依赖标签的插件。

```html
<body>
    <script>
        $.messager.alert('警告','警告消息');
        $.messager.confirm('确认','您确认想要删除记录吗？',function(r){
            if (r){
                alert('确认删除');
            }
        });
    </script>
</body>
```



## 表单 form

> 使用较多



### 文本框 textbox

```html
<body>
    <form action="#">
        用户名: <input type="text" name="username" id="username"><br>
    </form>
    <script>
        // textbox 输入框
        $('#username').textbox({
            width:180,
            height:60,
            buttonText:"提交",
            prompt:'请输入用户名',
            required:true,
            missingMessage:'请输入用户名',
            validType:['email','length[12,30]'],
            multiline:true,
            onClickButton:function(){
                if($(this).textbox('isValid')){
                    alert($(this).textbox('getValue'));
                    $(this).textbox('clear');
                }else{
                    alert('信息不合法');
                }
            }
        })
    </script>
</body>
```



### 数值输入框 numberbox

> 只能输入数值。

```html
<body>
    <form action="#">
        年龄: <input type="text" name="age" id="age"><br>
    </form>
    <script>
        // numberbox 数值输入框
        $('#age').numberbox({
            min: 0,
            max:150,
            groupSeparator:','
        })
    </script>
</body>
```



### 日期输入框 datebox

```html
<body>
    <form action="#">
        生日: <input type="text" name="birth" id="birth"><br>
    </form>
    <script>
        // 日期输入框
        $('#birth').datebox({
            required:true,
            editable:false
        })
    </script>
</body>
```



### 文件选择框 filebox

```html
<body>
    <form action="#">
        上传: <input type="text" name="upload" id="upload"><br>
    </form>
    <script>
        // 文件选择框
        $('#upload').filebox({
            buttonText:'选择文件',
            buttonIcon:'icon-save',
            width:400
        })
    </script>
</body>
```



## 数据表格 datagrid