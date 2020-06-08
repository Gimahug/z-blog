---
title: JavaScript学习笔记二
description: JavaScript函数学习：函数定义、arguments，rest等关键字、解构赋值、方法等。
categories:
 - 学习笔记
tags: JavaScript
---


## 定义函数

```js
function abs(x){
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}

var abs=function(x){
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}; //注意完整性
```
## 一些关键字
### arguments
+ arguments指向函数调用者传入的所有参数(可能多于函数定义的参数)，类似Array但不是Array
+ arguments常用于判断传入的参数

```js
// 此函数接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
}
```
### rest——ES6新标准
+ rest以数组形式获取额外的参数
+ rest写在参数列表的最后面，前面加...
+ 如果传入的参数没填满定义的参数，rest接收一个空数组而不是undefined

```js
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```
## 小心return语句

```js
function foo() {
    return { name: 'foo' };  //写成一行没问题
}
```

如果return语句拆分成两行

```js
function foo() {
    return         //JavaScript自动添加分号，相当于return undefined;
        { name: 'foo' };
}
```
正确写法：

```js
function foo() {
    return { 
        name: 'foo'
    };
}
```
## 变量提升
js函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部，但是只提升函数声明，不会提升变量赋值。

```js
function foo() {
    var x = 'Hello, ' + y;
    // 提升变量y的申明，此时y为undefined
    console.log(x); //Hello, undefined
    var y = 'Bob';
}
```
所以在函数内部应首先申明所有变量，常见做法：

```js
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined

    // 其他语句:
    
}
```
## 全局对象window
JavaScript默认有一个全局对象window，全局作用域的变量实际上是window的一个属性。JavaScript实际上只有一个全局作用域。

```js
'use strict';

var course = 'Learn JavaScript';
alert(window.course); 

function foo() {
    alert('foo');
}
window.foo(); //顶层函数的定义也被视为一个全局变量

window.alert('');  //alert()函数其实也是window的一个变量
```
## 定义自己的命名空间
全局变量会绑定到window上，不同的JavaScript文件使用相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突。减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中

```js
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```
## let和var
var无法定义具有局部作用域的变量

```js
'use strict';

function foo() {
    for (var i=0; i<100; i++) {
       
    }
    i += 100; // i在函数内部都是有效的
}
```
所以ES6引入了新的关键字let

```js
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) { //let声明了具有块级作用域的变量i
        sum += i;
    }
    
    i += 1; // SyntaxError
}
```

## const (ES6)

```js
const PI=3.14; //无法更改
```

## 解构赋值 (ES6)
同时对一组变量赋值
+ 数组的解构赋值

```js
'use strict';

var [x, y, z] = ['hello', 'JavaScript', 'ES6'];

//嵌套赋值
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
let [, , z] = ['hello', 'JavaScript', 'ES6'];
```

+ 对象的解构赋值

```js
'use strict';

var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;

//嵌套赋值
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
```
如果变量名和属性名不一致：

```js
// 把passport属性赋值给变量id:
let {name, passport:id} = person;
```

使用默认值：

```js
var {name, single=true} = person;
```

如果变量已被声明，可能会报错，原因是把{x, y}作为块处理了

```js
// 声明变量:
var x, y;

{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```
解决办法，加小括号：

```js
({x, y} = { name: '小明', x: 100, y: 200});
```

### 解构赋值使用场景
+ 交换变量值

```js
var x=1, y=2;
[x, y] = [y, x]
```
+ 快速获取当前页面的域名和路径

```js
var {hostname:domain,pathname:path}=location;
```

+ 对象作为函数参数时，可直接把对象属性绑定到变量中

```js
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}

buildDate({ year: 2017, month: 1, day: 1 });

buildDate({ year: 2017, month: 1, day: 1, hour: 20, minute: 15 });
```
## 方法
### 对象的方法

```js
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;  //this指向当前对象:xiaoming
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```
### 单独调用时
+ 非strict模式，this指向全局对象：window
+ strict模式下this指向undefined

```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // this指向window，返回NaN
```

```js
'use strict';

function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // Uncaught TypeError: Cannot read property 'birth' of undefined(this指向undefined)
```
### 用apply()控制this指向
apply(this指向的变量，函数的参数Array)：函数本身的方法

```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

#### call()
+ call()与apply()相似，区别是apply把参数打包进Array，call把参数顺序传入
+ 对于普通函数调用，把this指向null

```js
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```
#### 用apply()动态改变函数行为
example:统计一共调用了多少次parseInt()

```js
'use strict';

var count=0;
var oldParseInt=parseInt; //保存原函数

window.parseInt=function(){
    count+=1; //增加计数功能
    return oldParseInt.apply(null,arguments); //调用原函数
}

```














