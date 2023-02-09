#   入门



## 编写位置

> \<script>标签有三种编写方式，JS 代码只能写在其中，三种编写方式如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <!-- 1、编写到script标签内部,script标签可以写在head或者body标签中 -->
  <script>
    // alert('你好')
  </script>
  <!-- 2、将js编写到外部的js文件中，并通过script标签引入，注意: 引入式不能再在script标签中编写代码 -->
  <script src="./script/script.js"></script>
</head>

<body>
  <!-- 3、直接将js代码编写到指定属性中 -->
  <button onclick="alert('hello')">点我一下</button>
  <!-- 也可以在超链接中跳转中编写js代码, 注意: 链接中的javascript:表示超链接点击后执行一段js代码 -->
  <a href="javascript:alert('hello')">超链接</a>
</body>

</html>
```



## 基本语法

> JS中规定的一些语法。

1. 注释分为单行和多行注释，符号分别为 // 和 /**/
2. JS 严格区分大小写。
3. 在 JS 中多个空格和换行会被忽略，可以利用这一特点对代码进行格式化。
4. JS 中每一条语句都应该以分号结尾，JS 提供了自动添加分号的机制，不加也是可以的。



## 常量

> 在 JS 中，使用 const 来声明常量，常量只能赋值一次，赋值完后不能修改。

```html
<script>
	const PI = 3.1415926;
    PI = 2;//Uncaught TypeError: Assignment to constant variable.
</script>
```



# 数据类型

> JS 中一共有七种数据类型，分别是数值（Number）、大整数（Bigint）、字符串（String）、布尔值（Boolean）、空值（Null）、未定义（Undefined）、符号（Symbol）。

## 数值（number）

```html
<script>
    /*
        数值(Number)
          - 在 JS 中所有的整数和浮点数都是Number类型
          - JS 中的数值并不是无限大的，当数值超过一定范围后会显示近似值，与原值有一定偏差，并且再大会以科学计数法表示。
          - JS 中有一个特殊值: Infinity, 表示无穷, 当数值表示范围超过一个特定范围时, 会直接返回 Infinity
          - JS 中还有另一个特殊值: NaN, Not a Number, 表示非法数值
     */
    let a = 10
    a = 3.14
    a = 99999999999999999911 // 出现偏差
    a = 99999 ** 99999 // **表示次方, 返回Infinity
    a = 0.1 + 0.2 // 浮点数计算, 二进制运算通病
    a = 1 - 'a' // 返回 NaN
    console.log(a);
</script>
```



## 大整数（bigInt）

```html
<script>
    /* 
      大整数(BigInt)
        - 大整数可以用来表示一些比较大的整数, 没有范围上限, 内存多大就能表示多大
        - 大整数使用 n 结尾表示
    */
    let a = 9999999999999999999999999999999999n
    /* 
      以下是以其它进制的方式定义数字:
        二进制 0b
        八进制 0o
        十六进制 0x
      定义方式为其它进制, 使用则仍是十进制
    */
    a = 0b1010
    a = 0x10
    a = 0xff
    console.log(a);
</script>
```



## 字符串（string）

```html
<script>
    /*
      字符串
        - 在 JS 中使用单引号或者双引号来表示字符串, 但不能前后混用, 如有需要, 可以采用转义字符 \, 字符串不能直接换行，需要加 \ 
        - 转义字符 \
          \" => "
          \' => '
          \\ => \
          \t => 制表符(tab缩进)
          \n => 换行
    */
    let a = "hello";
    a = 'hello'
    /*
      模板字符串
        - 在 JS 中还提供了一种模板字符串，用 `` 来表示，可以在里面通过 ${} 动态嵌入其它变量
    */
    let name = '猪';
    console.log(`${a} : ${name}`);
</script>
```



## 布尔值（boolean）

```html
<script>
	/*
		布尔值(boolean)
			- 布尔值主要用来进行逻辑判断
			- 布尔值只有两个 true 和 false
			- 使用 typeof 检查一个布尔值时会返回 'boolean' 字符串
	*/
    let bool = true // 真
    bool = false //假
	console.log(typeof bool) // 'boolean'
</script>
```



## 空值（null）

```html
<script>
    /* 
      空值（null）
        - 空值用来表示空对象
        - 空值只有一个值: null
        - 使用 typeof 检查一个空值时会返回 'object', 所以typeof无法检查空值
    */
    let a = null;
    console.log(typeof a);  // 'object'
</script>
```



## 未定义（undefined）

```html
<script>
    /* 
      未定义（undefined）
        - 当声明一个变量而没有赋值时，它的值就是 undefined
        - Undefined 类型只有一个值: undefined
        - 使用 typeof 检查一个Undefined类型的值时，会返回 'undefined'
    */
    let b;
    console.log(b); // 'undefined'
</script>
```



## 类型检查

> JS 中提供了 typeof 的方式检查值的类型，可以让程序员得到需要的数据类型。

```html
<script>
    /*
      typeof 运算符
        - typeof 用来检查值的类型，它会根据值类型的不同返回不同的结果
    */
    let a = 10
    let b = 10n
    console.log(typeof a);
    console.log(typeof b);
</script>
```



## 类型转换

### => string

> 转换为字符串的方法可以分为隐式和显示，显示有两种，直接调用变量.toString() 方法或者使用 String() 函数进行转换即可。隐式转换即直接通过 **变量 + 空字符串**，让 JS 帮我们自动转成 string。

```html
<script>
    /* 
      toString()方法
        - 需要注意, null和undefined中没有toString()方法
    */
    let a = 10; // a: number 10
    a = a.toString()  // a: string '10'
    a = true  // a: boolean true
    a = a.toString()  // a: string 'true'

    /*
      String()函数
        - String() 函数可以将其它类型转换为字符串, 并且没有类型限制
    */
    let b = null; // b: object null
    b = String(b);  // b: string 'null'
</script>
```



### => number

> JS 提供将其它类型转换为数值的方法有多种，如 Number() 函数，parseInt() 函数和 parseFloat() 函数

```html
<script>
    /* 
      => number
        1、使用Number()函数来将其它类型转换为数值
          - 字符串:
            - 如果字符串是一个合法的数字，则会自动转换为对应的数值
            - 如果字符串不是合法数字，则转换为NaN
          - 布尔值:
            - true 转换为 1, false 转换为 0
          - null 转换为 0
          - undefined 转换为 NaN

        2、专门用来将字符串转换为数值的两个方法
          parseInt() -- 将一个字符串转换为一个整数
            - 解析时, 会自左向右读取一个字符串，直到读取到字符串中所有的有效整数
          parseFloat() -- 将一个字符串转换为浮点数
            - 解析时, 会自左向右读取一个字符串，直到读取到字符串中所有的有效整数和有效小数
    */
    let a = '123'; // 123
    a = 'abc';  // NaN
    a = '11px'  // NaN
    a = '3.1415'  // 3.1415
    a = ''  // 0
    a = '   ' // 0
    a = true  // 1
    a = false // 0
    a = null  // 0
    a = undefined // NaN
    a = Number(a);
    console.log(typeof a, a);
</script>
```



### => boolean

> JS 中提供显式方式 Boolean() 来将其它类型转换为 boolean 类型。

```html
<script>
    /*
      1、使用Boolean()函数将其它类型转换为布尔值
        - 数字
          - 0 和 NaN 转换为 false
          - 其余都是 true
        - 字符串
          - 空串转换为 false
          - 其它都是 true
        - null 和 undefined 都转换为 false
        - 对象: 对象都会转换为 true
        - 所有表示空性的、没有的、错误的值都会转换为 false: 0、NaN、空串、null、undefined、false
    */
    let a = 1 // true
    a = -1  // true
    a = 0  // false
    a = NaN  // false
    a = Infinity  // true

    a = 'abc' // true
    a = 'true' // true
    a = 'false' // true
    a = ''  //false
    a = " " // true
    a = null // false
    a = undefined // false
    a = Boolean(a)
    console.log(typeof a, a);
</script>
```



# 运算符



## 算术运算符

> JS 提供的算术运算符，如加减乘除、取余、幂、开方等。

```html
<script>
    /*
      运算符（操作符）
        - 运算符可以用来对一个或多个操作数（值）进行运算
        - 算术运算符:
            + 加法运算符
            - 剑法运算符
            * 乘法运算符
            / 除法运算符
            ** 幂运算
            % 模运算
    */
    let a = 1 + 1 // 2
    a = 10 - 5  // 5
    a = 10 / 5  // 2
    a = 10 / 0  // Infinity
    a = 10 ** 2 // 100
    a = 9 ** .5 // 3
    a = 10 % 3  // 1
    /* 
      JS 是一门弱类型语言，当进行运算时会通过自动的类型转换来完成运算
    */
    a = 10 - '5'  // 5
    a = 10 + true // 11
    a = 5 + null // 5
    a = 6 - undefined // NaN
    a = '1' + '2' // '12'
    /* 
      当任意一个值和字符串做加法运算时，它会先将其他值转换为字符串，然后再做拼串操作，可以利用这一特点完成类型转换操作
    */
    a = true + '' // 'true'
    console.log(a);
</script>
```



## 赋值运算符

```html
<script>
    /*
      赋值运算符用来将一个值赋值给一个变量
        =     将符号右侧的值赋值给左侧变量
        ??=   空赋值, 只有当变量原先的值为null或者undefined时, 才会对变量进行赋值
        +=    加等于
        -=    减等于
        /=    除等于
        %=    取模等于
        **=   幂等于
    */
    let a = 10;
    a = null;
    a ??= 100;  // 100
    a = 5;
    a ??= 100;  // 5
    console.log(a);
</script>
```



# 对象

> JS 提供的一种复合数据类型，相当于一个容器，在对象中可以存储不同类型的属性数据

## 创建方式

> JS 提供的对象创建方式有多种方式，选择合适的方式进行创建即可。

```html
<script>
    // 创建对象方式1
    let obj = new Object();

    // 方式2(方式1的简化)
    obj = Object();

    // 方式3(字面量方式)
    obj = {}
    
    console.log(obj);	//{}
</script>
```

## 属性

> 存储在对象中的属性。

### 规范

> 即使早期 JS 对从程序编写规范要求不高，但针对对象仍有一定限制，如果要较好地操作对象，就需要遵守 JS 规定的对象的一些使用规范。

#### 属性名

> 对象中的属性名命名也有一定规范，参考如下

```html
<script>
    /*
      属性名
        - 通常情况下属性名就是一个字符串，所以对于属性名没什么要求。
          	但是如果属性名过于特殊，就不能直接使用，而是需要通过['']来设置（使用时也通过['']来使用）。
          	正常情况下还是建议属性名也按照标识符的规范来命名。
        - 第二种情况则是属性名是一个符号symbol,这个使用较少,这里不过多讨论。
    */
    let obj = {};
    obj.name = "悟空";
    // obj.if = '男';   // 标识符命名,不建议,但能用
    // obj['123@#!aq'] = 'wow';   // 不符合命名规范的特殊命名, 通过['']也可以使用,但不建议
    console.log(obj);   // {name: '悟空', if: '男', 123@#!aq: 'wow'}
</script>
```

#### 属性值

```html
<script>
    let obj = {}
    /* 
      属性值
        - 对象的属性值可以是任意的数据类型, 可以是基本类型, 也可以是对象
    */
    // 属性可以是基本类型  
    obj.a = 18
    obj.b = 'hello'
    obj.c = true
    obj.d = 123n
    // 属性也可以是对象
    obj.e = {}
    obj.e.name = '猪八戒'
    console.log(obj); // {a: 18, b: 'hello', c: true, d: 123n, e: {name: '猪八戒'}}
</script>
```

### 操作

#### 添加/修改

> 在 JS 中，属性添加 / 修改 操作都是互相适用的的。

```html
<script>
    let obj = {}
    // 属性添加方式一: 对象.属性名 = 属性值;
    obj.age = 18
    // 属性添加方式二: 对象['属性名'] = 属性值;
    obj['gender'] = "男"
    // 通过中括号的方式添加属性可以引用变量作为属性名
    let str = "address"
    obj[str] = '花果山'
    // 而直接通过.设置的方式则是直接将.后面的字符串作为属性名,不会使用到引用
    obj.str = '哈哈'
    console.log(obj); // {age: 18, gender: '男', address: '花果山', str: '哈哈'}
</script>
```

#### 读取

> 在 JS 中，读取属性可以通过 **对象.属性名** 或者 **对象[‘属性名’]** 来读取，若没有该属性则会返回 **undefined**

```html
<script>
    let obj = {}
    obj.age = 18
    obj['gender'] = "男"
    let str = "address"
    obj[str] = '花果山'
    obj.str = '哈哈'

    console.log(obj.age);     // 18
    console.log(obj.gender);  // 男
    // 不带引号是引用          
    console.log(obj[str]);    // 花果山
    // 带引号是字符串
    console.log(obj['str']);  // 哈哈
    console.log(obj.str);     // 哈哈
</script>
```

#### 删除

> 对象中删除属性使用 delete 运算符，通过 **delete 对象.属性名** 或者 **delete 对象[‘属性名’]** 删除某个属性。删除属性不存在也不会报错，程序会继续执行。

```html
<script>
    let obj = {}
    obj.age = 18
    obj['gender'] = "男"
    let str = "address"
    obj[str] = '花果山'
    obj.str = '哈哈'
 
    delete obj[str]
    delete obj.str
    console.log(obj); // { age: 18, gender: '男' }
</script>
```

#### 判断

> 判断对象中是否存在某个属性名，使用 **属性名 in 对象** 来判断，存在返回 true，不存在返回 false

```html
<script>
    let obj = {}
    obj.age = 18
    obj['gender'] = "男"
    let str = "address"
    obj[str] = '花果山'
    obj.str = '哈哈'
    /* 
      in 运算符
        - 用来检查对象中是否含有某个属性
        - 语法: 属性名 in obj
        - 如果存在返回 true, 不存在返回 false
    */
    console.log("b" in obj);    // false
    console.log('str' in obj);  // true
    console.log(str in obj);    // true
</script>
```

#### 枚举

> 枚举即枚举属性，把对象中可被枚举的属性通过 **for in** 语句一一枚举出来

```html
<script>
    let str = 'address';
    let obj = {
        age: 18,
        ['gender']: '男',
        [str]: '花果山',
        str: '哈哈',
        [Symbol()]: '不可枚举属性'
    }
    /* 
      枚举属性使用的 for-in 语句:
        - 语法:  
            for(let name in 对象){
              语句...
            }
        - for-in 的循环体会执行多次, 直到将对象中所有可枚举属性遍历一次, 每次读取出来的属性会赋给我们定义的变量中
        - 注意: 不是所有的属性都可以被枚举, 比如: 使用符号symbol添加的属性就不能被枚举
    */
    for (let name in obj) {
        // 这里要获取属性名对应的属性值, 只能通过对象[属性名]引用的方式去获取
        console.log(name, obj[name]);
    }
</script>
```

