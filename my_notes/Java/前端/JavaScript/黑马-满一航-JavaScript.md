# 一、JS入门

> JS是一种脚本语言，可以用来更改页面内容，控制多媒体、制作图像、动画等。

**JS代码位置**

```javascript
<script>

</script>
```

**引入JS脚本**

```javascript
<script src="js脚本路径"></script>
```

- 需要注意：到框架之后，JS引入方式会有所不同。

**入门案例**

```javascript
document对象：整个HTML文档所组成的一个对象。

document.getElementById()：在文档中根据标签ID查找元素。

let a = document.getElementById()：将右边 JS 的执行结果赋值给左边变量。

a.innerText = “22”：将 a 变量指向的元素的内容修改为 “22”。
```



# 二、变量与数据类型

## 1、声明变量

> JS 中可以用来声明变量的共有三个：let、const、var。

### let

> let 是临时变量声明，作用域为{}或者当前函数。

```javascript
// 分号可加可不加，但建议加
let 变量名 = 值;
```

- let 声明的变量可以被多次赋值，例如

```javascript
let a = 100;  //初始值是 100
a = 200;   //ok, 被重新赋值为 200
```



### const

> const 声明的是常量，只能赋值一次，当然这种赋值是指基本类型，如果是引用常量，其内部是可变的。

```javascript
const b = 300; //初始值是 300
b = 400;  //error, 不能再次赋值
```

- const 并不意味着它引用的内容不可修改，例如

```javascript
const c = [1,2,3];
c[0] = 4;  //ok, 数组内容被修改为[4,2,3]
c = [5,6];  //error, 不能再次赋值
```



### var

> var 是全局变量，作用域为当前函数。记住：能用 let 的地方就别用 var。

```javascript
var f = 200;
f = 100;
```



## 2、基本类型

> JS 中的基本类型有八种，分别是 undefined、null、string、number、bigint、boolean、symbol。

### undefined

> 不存在类型。由 JS 默认提供。

**出现undefined的几种情况**

1. 函数表达式执行没有返回结果时，会出现 undefined。
2. 数组元素不存在，或者对象不存在时，也会出现 undefined。
3. 声明了变量，没有赋初值时，也会出现 undefined。



### null

> 空值。由程序员主动指定，效果和 undefined 一样，二者的共同点为都没有属性、方法，并称为 Nulllish。



### string

> JS 中的字符串类型。共有三种写法，双引号、单引号、反斜号。

```javascript
let s1 = "string";  //ok, 双引号
let s2 = 'string';  //ok, 单引号
let s3 = `string`;  //ok, 反斜号
```

**string提供了一种字符串拼接更方便的方式**

```javascript
// 传统拼接方式
let name = zhangsan;
let age = 18;

let uri = "/test?name=" + name + "&age=" + age

// 模板字符串拼接方式 [使用反引号]
let name = zhangsan;
let age = 18;

let uri = `/test?name=${name}&age=${age}`;
```



### number

> JS 提供的双精度浮点数。

```javascript
10 / 3;	 // 结果: 3.3333333335
```

既然是浮点小数，那么可以除零

```javascript
10 / 0;	 // 结果: Infinity 正无穷大
10 / -0	 // 结果: -Infinity 负无穷大
```

浮点小数都有运算精度问题，例如

```javascript
2.0 - 1.1	 // 结果: 0.899999999
```

**字符串转数字**

```javascript
parseInt("10");		 // 结果是数字 10
parseInt("10.5");	 // 结果是数字 10, 去除了小数部分
parseInt("10") / 3;  // 结果仍视为 number 浮点数，因为结果为 3.33333335

parseInt("abc");	 // 转换失败，结果是特殊值 NaN(Not a Number)

var a = `10` - 0	 // 结果为 10，JS会自动转换
```



### bigint

> 真正的整数，需要以 n 作结尾。

```javascript
10n / 3n; 	// 结果 3n，按整数除法处理
```



### boolean

> JS 中，并不是只以 true 和 false 来作判断，而是以 Truthy 和 Falsy 来做判断，Truthy 包含了 true，Falsy 包含了 false。

比如

```javascript
let b = 1;

if(b){ 	//true
    console.log(b);
}
```

**这时就有一个规则，当需要条件判断时，这个值被当作 true 还是 false，当作 true 的值归类为 Truthy，当作 false 的值归类为 falsy。除了 falsy，其它都被看成 Truthy。**

**falsy**

- false
- Nullish（null，undefined）
- 0，0n，NaN
- “”，’‘，空反引号



### symbol

> 使用很少，暂且不提



## 3、对象类型

> 在 JS 中，对象类型千千万，常用的有 Function、Array、Object等

### Function

> 在 JS 中函数本质上是对象。可以参与赋值，有属性，有方法、可以作为方法参数、可以作为方法返回值。并且函数在定义时其作用域就定下来了。

#### 普通函数

**定义**

```javascript
function 函数名(参数){
    //函数体
    return 结果;
}
```

**例如**

```javascript
function add(a,b){
    return a + b;
}
```

**调用函数**

```javascript
函数名(实参);
```

**例如**

```javascript
add(1,2);	// 返回 3
add(1);	 	// 返回 NaN，这时 b 没有定义，是 undefined, undefined 做数学运算结果就是 NaN
add('a', 'b')	 // 返回 ab
add(4,5,6)	// 返回 9，第三个参数没有被用到，不会报错
```

**函数提供默认参数**

> JS 允许设置函数参数时设置默认参数，不传参时则使用默认参数。

```javascript
function pagination(page = 1, size = 10){
    console.log(page, size);
}
```



#### 匿名函数

**语法**

```javascript
function(参数){
    // 函数体
    return 结果;
}
```

**例如**

```javascript
(function(a,b){
    return a + b;
})
```

匿名函数有两种使用场景

**第一种是定义完后立刻调用**

```javascript
(function(a,b){
    return a + b;
})(1,2);	 // 结果: 3
```

**第二种是作为其他对象的方法**

```javascript
document.getElementById("p1").onclick = (function(){
   console.log("1"); 
});
```



#### 箭头函数

> 可以看作匿名函数更简便的写法。

**写法**

```javascript
(参数) => {
    // 函数体
    return 结果;
}
```

- 如果只有一个参数，（）可以省略
- 如果函数体内只有一行代码，{} 可以省略

**如**

```java
document.getElementById("p1").onclick = () => console.log("1");
```



### Array

> JS 提供的数组类型对象

**语法**

```javascript
// 创建数组
let arr = [1,2,3];

// 获取数组元素
console.log(arr[0]);	 //输出 1

// 修改数组元素
arr[0] = 5;		// 数组元素变成了 [5,2,3]

// 遍历数组元素，其中 length 是数组属性，代表数组长度
for(let i = 0; i < arr.length; i++){
    console.log(arr[i]);
}
```

#### push、shift、splice

```javascript
let arr = [1,2,3];

// push: 在数组右边加入元素
arr.push(4);	// 结果: arr[1,2,3,4]

// shift: 移除数组左边的元素
arr.shift();	// 结果: arr[2,3,4]

// splice: 移除指定索引和长度的元素
arr.splice(1,1);	// 结果: arr[2,4]
```

#### join

```javascript
let arr = ['a', 'b', 'c'];

// join: 将数组以某种格式连接成字符串
arr.join()		// 结果: 'a,b,c'
arr.join('-')	// 结果: 'a-b-c'
arr.join('_')	// 结果: 'a_b_c'
```

#### map、filter、foreach

> 这三个函数在执行时都不会对原有数组产生变化，生成的都是新数组。

**map**

```javascript
let arr = [1,2,3];

function a(i){ 
    return i * 10;
}
arr.map(a);	  	// 结果: [10,20,30]

// 或者使用箭头函数
arr.map(i => i * 10);
```

- 传递给 map 的函数，参数代表旧元素，返回值代表新元素

**map 的内部实现（伪代码）**

```javascript
function map(a){	 // 参数是一个函数
    let narr = [];
    for(let i = 0; i < arr.length; i++){
        let o = arr[i];  // 旧元素
        let n = a(o);	 // 新元素
        narr.push(n);
    }
    return narr;
}
```



**filter**

```javascript
let arr = [1,2,3,4];

arr.filter( i => i % 2 == 1); 	// 结果: [1,3]
```

- 传给 filter 的函数，参数代表旧元素，返回值 true 时留下元素。



**forEach**

```js
let arr = [1,2,3,4];

arr.forEach( i => console.log(i));  //结果: 1 2 3 4
```



### Object

> JS 中的对象。

**语法**

```javascript
let obj = {
    属性名: 值,
    方法名: 函数,
    get 属性名() {},
    set 属性名(新值) {}
}
```

#### 样例

**例1**

```js
let stu = {
    name: "小明",
    age: 18,
    study: function(){
        console.log(this.name + "爱学习");
    }
}
```

**例2**

```js
// 属性写外面
let name = "小明";
let age = 18;
let study = function(){
    console.log(this.name + "爱学习");
}

let stu = {name, age, study};
```

**例3（重点）**

```js
let stu = {
    name: "小明",
    age: 18,
    // 对象方法简写，只能在方法内简写
    study(){
        console.log(this.name + "爱学习");
    }
}
```

**例4**

```js
let stu = {
    _name: null,  //类似于Java中的私有成员变量，但是JS实际上允许访问，这只是一种约定
    get name(){
        console.log("进入了get");
        return this._name;
    },
    set name(name){
        console.log("进入了set");
        this._name = name;
    }
}
```

调用 setter 和 getter

```js
stu.name = "小明";		// 结果: 进入了set 小明
console.log(stu.name)	 // 结果: 进入了get 小明
```



#### 属性增删

> JS 中，Object 允许属性和方法的动态增删。

- 在 Java 中，对象都是以类作为模板来创建的，对象不能脱离类模板的范围，一个对象的属性，能用的方法都是在类定义时确认好的。
- JS 的对象中，不需要什么模板，它的属性和方法可以随时加减。

```js
let stu = {name:'张三'};

stu.age = 18;				// 添加属性
delete stu.age				// 删除属性

stu.study = function(){		// 添加方法
    console.log(this.name + "在学习");	
}
```

**添加 get，set 方法**

```js
let stu = {_name:null};

Object.defineProperty(stu, "name", {
    get(){
        return this._name;
    },
    set(name){
        this._name = name; 
    }
})
```



#### this

> JS 中的 this 和 Java 中一样，都是隐式参数，Java 中对象的 this 会隐式地出现在构造函数的第一个参数处，JS   也不例外，但是 JS 中的 this 和函数运行时的上下文相关。

**例如，一个非对象内部的函数**

```js
function study(subject){
    console.log(this.name + "在学习" + subject);
}

study("js")				// 结果:  在学习 js
```

- 出现上面结果的原因是非对象内部的函数在执行时，全局对象 window 被当作了 this，window 对象的 name 属性默认是 “”。

**同样的函数，如果是对象内部的函数**

```js
function study(subject){
    console.log(this.name + "在学习" + subject);
}

let stu = {
	name: "小白",
    // 引用外部方法
    study
}

study("js")				// 结果: 小白在学习 js
```

- 这种情况下，this 就是对象本身了。

**或者可以动态改变 this**

```js
function study(subject){
    console.log(this.name + "在学习" + subject);
}

let stu = {
	name: "小白",
    study
}

let stu = {name:"小黑"};
study.call(stu, "js");		// 结果: 小黑在学习 js
```

- study.call 在执行时，把 call 函数中第一个参数作为 this 传入。

**有另外一个知识点，在箭头函数内出现的 this，以及匿名函数的 this**

**匿名函数 this**

```js
let stu = {
    name: "小花",
    friends: ["小白","小黑","小明"],
    play:function(){
        this.friends.forEach(function(e){
            console.log(this.name + "与" + e + "在玩耍");
        })
    }
}

stu.play();
/**
 结果:
    与小白在玩耍
    与小黑在玩耍
    与小明在玩耍
*/
```

-  匿名函数实际上成了非对象内部的函数，this 变成了 window

**箭头函数 this**

```js
let stu = {
    name: "小花",
    friends: ["小白","小黑","小明"],
    play:function(){
        this.friends.forEach(e => {
            console.log(this.name + "与" + e + "在玩耍");
        })
    }
}

stu.play();
/**
  结果:
	小花与小白在玩耍
	小花与小黑在玩耍
	小花与小明在玩耍
*/
```

- this.name 所在的函数是箭头函数， this 要看它外层的 play 函数，play 又是属于 stu 的方法，因此 this 代表 stu 对象

**不用箭头函数同时让 this 属于对象**

```js
let stu = {
    name: "小花",
    friends: ["小白","小黑","小明"],
    play:function(){
        let o = this;
        this.friends.forEach(function(e){
            console.log(o.name + "与" + e + "在玩耍");
        })
    }
}

stu.play();
/**
  结果:
	小花与小白在玩耍
	小花与小黑在玩耍
	小花与小明在玩耍
*/
```



#### 原型继承

> JS 中的继承属于对象继承，每个对象中有一个 __proto\_\_ 属性，该属性指向父对象，如果没有父对象，则默认父对象是 Object 。调用对象.属性会根据**原型链**层级往上找，直到找不到为止。

**继承案例**

```js
let father = {
    f1: '父属性',
    m1: function(){
        console.log("父方法");
    }
}
// Object.create(); 以()中对象为父对象创建对象
let son = Object.create(father);

console.log(son.f1);		// 打印: '父属性'
son.m1();					// 打印: '父方法'
```

- 案例中，father 是父对象，son 去调用 m1 方法 或 f1 属性时，自身对象没有，就到父对象中找。
- son 自己可以添加自己的属性和方法。
- son 里有特殊属性 __proto\_\_ 代表它的父对象，当然，JS 中的术语将父对象称为**原型对象**。

**基于函数的原型继承**

> JS 出于方便的原因，提供了一种基于函数的原型继承，其实就是使用函数来创建具有某个父类对象的子类对象。

函数的职责

1. 负责创建子对象，给子对象提供属性、方法，功能上相当于构造方法
2. 函数有个特殊的属性 prototype，它就是函数创建的子对象的父对象，这个属性的作用就是为新对象提供原型对象。

```js
function cons(f1){
    //定义子对象this，给子对象提供属性和方法
    this.f1 = f1;
    this.m1 = function(){
        console.log("子方法");
    }
}
// cons.prototype 就是父对象
cons.prototype.f2 = '父属性';
cons.prototype.m2 = function(){
    console.log('父方法');
}
```

- 以上只是通过函数定义了这个对象及其原型对象，想要创建出来还需要通过 new 关键字。

```js
let son = new cons('子属性');

console.dir(son);
```

- 子对象存在的 __proto\_\_ 属性就是函数的 prototype 属性，其实就是原型对象。



### JSON

> 在 JS 中，提供了 JSON 对象，JSON 对象的语法格式和普通的 JS 对象相似。

**一个普通的 JSON 对象**

```js
{
    "name":"张三",
   	"age": 18
}
```

**一个普通的 JS 对象**

```js
{
    name:"张三",
    study(){
        console.log("我爱学习");
    }
}
```

**二者区别**

1. 本质不同
   - JSON 对象本质上是一个**字符串**，它的职责是作为客户端和服务器之间传递数据的一种格式，其属性都只是样子货。
   - JS 对象是货真价实的对象，可以有属性，方法。
2. 细节不同
   - JSON 属性对应的值只能有 **null、true/false、数字、字符串（双引号）、对象、数组**。其它一律不行，包括方法也不行。

**JSON 字符串转 JS 对象**

```js
// JSON 字符串要用 `` 框起, 且 JSON 字符串中不能有非法值，否则报错
let json = `{
    "name":"张三",
    "age":18
}`
let o = JSON.parse(json);
```

**JS 对象转 JSON 字符串**

```js
// JS 对象转 JSON 只转属性，方法不转
let o = {
    name:"张三",
    age:18
}
let json = JSON.stringify(o);
```



## 4、各路名词

> JS 中命名不好理解的名词

### 1、作用域

> 通过 console.dir(对象) 可以查看对象的属性。



### 2、闭包



### 3、高阶函数

> 接收函数作为参数的函数称作高阶函数。如 map函数、filter函数、forEach函数。



### 4、回调函数

> 作为参数传入到函数中的函数被称作回调函数。和高阶函数一个作为传入方，一个作为接收方。



# 三、运算符与控制语句

> JS 中运算符基本和 Java 中一致。

- **\+	-	*	/ %	****
- **\+=    -=    *=    /=    \*\*=**
- **\++    - -**
- **位运算、移位运算**
- **==    !=    >    >=    <    <=**
- **\===    !==**
- **&&    ||    !**
- **??    ?.**
- **…**
- **解构赋值**

| 运算符 |                      作用                      |         举例          |
| :----: | :--------------------------------------------: | :-------------------: |
|   **   |                      次方                      |    3 ****** 2 = 9     |
|  ===   | 严格相等运算符，判断**类型**和**值**是否都相等 | ’1‘ **===** 1;  false |

------



> JS 中控制语句基本也和 Java 一致。

- **if … else**
- **switch**
- **while**
- **do … while**
- **for**
- **for … in**
- **for … of**
- **try … catch**



## 运算符

> 这里介绍一些 JS 独特的运算符。

### ??

> nulllish 判断运算符，判断目标变量是否 \=== undefined 或 === null。

引入需求：**如果参数 n 没有传递参数或传递参数为 null，赋予默认值男**。

**传统做法**

```js
function test(n){
    if(n === undefined || n === null){
        n = '男';
    }
    console.log(n);
}
```

**使用 ?? 替代**

```js
function test(n){
    n = n ?? '男';
    console.log(n);
}
```



### ?.

> 一般用于属性判断，若判断某个属性为空，则返回 undefined。

引入需求：**函数参数是一个对象，函数内部需要调用对象的一些属性，并不要报错。**

```js
let stu1 = {
    name:"张三",
    address:{
        city:'北京'
    }
};

let stu2 = {
    name:"李四",
}

let stu3 = {
    name:"王五",
    address: null
}
```

访问对象的子属性

```js
function test(stu){
    console.log(stu.address.city);		// 如果传入stu对象是 stu2 或 stu3，则报错
}
```

如果想不报错，可以采用**传统办法**解决

```js
function test(stu){
    if(stu.address === undefined || stu.address === null){
        console.log(undefined);
        return;
    }
    console.log(stu.address.city);
}
```

**使用 ?. 替代**

```js
function test(stu){
    console.log(stu.address?.city);
}
```



### …

> 展开运算符，也是用于简便 JS 操作。

**作用1：打散数组传递给多个参数**

```js
let arr = [1,2,3];

function test(a,b,c){
    console.log(a,b,c);
}

test(...arr)		// 输出: 1,2,3
```

**作用2：复制数组或对象**

**复制数组**

```js
let arr1 = [1,2,3];

let arr2 = [...arr1];		// 产生新数组
```

**复制对象**

```js
let obj1 = {name:'张三',age:18};

let obj2 = {...obj1};		// 复制对象( JS 中对象复制是浅拷贝，再深一层的对象属性其实是引用了)
```



**作用3：合并数组或对象**

**合并数组**

```js
let a1 = [1,2];
let a2 = [3,4];

let b1 = [...a1,...a2]		// 结果: [1,2,3,4]
let b2 = [...a2,...a1]		// 结果: [3,4,1,2]
```

**合并对象**

```js
let o1 = {name:'张三'};
let o2 = {age:18};
let o3 = {address:'北京'};
let o4 = {name:'李四'};

let ob1 = {...o1,...o2,...o3};		// 结果: {name: '张三', age: 18, address: '北京'}
let ob2 = {...o1,...o2,...o4};		// 结果(先前属性被覆盖): {name: '李四', age: 18}
```



### []{}

> 解构赋值运算符。[]用于数组解构，{}用于对象解构。

**[]的两种使用场景**

**用在声明变量时**

```js
let arr = [1,2,3];

let a,b,c = arr			// 结果: undefined undefined [1,2,3]
let [a,b,c] = arr		// 结果: 1,2,3
```

**用在声明参数时**

```js
let arr = [1,2,3];

function test(a,b,c){
    console.log(a,b,c);
}
test(arr);				// 结果: [1,2,3] undefined undefined

function test([a,b,c]){
    console.log(a,b,c);
}
test(arr);				// 结果: 1 2 3
```



**{}的两种使用场景**

**用在声明变量时**

```js
let obj = {name:'张三',age:18};

let {name,age} = obj;
console.log(name,age);			// 结果: 张三 18

let {name,egg} = obj;
console.log(name,egg);			// 结果(解构名字要对上): 张三 undefined
```

**用在声明参数时**

```js
let obj = {name:"张三",age:18};

function test({name, age}){
    console.log(name, age);
}

test(obj);						// 结果: 张三 18
```



## 控制语句

> 这里介绍一些 JS 中独特的控制语句。

### for in

> for in 主要用来遍历对象

```js
let father = {name:'张三', age:18, study:function(){}};

for(const n in father){
    console.log(n);				// 结果: name age study
}
```

- const n 表示遍历出来的**属性名**
- 注意1：**方法名也能被遍历出来（方法名也算一种特殊属性）**
- 注意2：**遍历子对象时，父对象的属性会跟着遍历出来**

```js
let son = Object.create(father);
son.sex = '男';

for(const n in son){
    console.log(n);				// 结果: sex name age study
}
```

- 注意3：**如果想通过 for in 获取属性值，要使用 [] 语法，而不能用 . 语法。**

```js
for(const n in son){
    console.log(n, son.n);		// 结果: sex undefined name undefined age undefined study undefined
    console.log(n, son[n]);		// 结果: sex 男 name 张三 age 18 study f(){}
}
```



### for of

> for of 主要用来遍历数组，当然可以遍历 Map、Set 等对象。

```js
let a1 = [1,2,3];

for(const i of a1){
    console.log(i);				// 结果: 1 2 3
}

let a2 = [
    {name:'张三',age:18},
    {name:'李四',age:20},
    {name:'王五',age:22}
]

for(const obj of a2){			// 结果: 张三 18 李四 20 王五 22		
    console.log(obj.name, obj.age);
}

// 解构写法
for(const {name,age} of a2){	// 结果: 张三 18 李四 20 王五 22	
    console.log(name, age);
}
```



### try catch

> 基本类似于 Java 中的 try catch。

```js
let stu1 = {name:'张三', age:18, address: {city: '北京'}};
let stu2 = {name:'张三', age:18};

function test(stu){
    try{
        console.log(stu.address.city);
    }catch(e){
        console.log("出现异常..", e);
    }finally{
        console.log("最终执行..");
    }
}
```

