---
title: JavaScript学习笔记
description: JavaScript入门学习：语句结构、字符串和对象等数据类型、Set和Map等ES6新增标准。
categories:
 - 学习笔记
tags: JavaScript
---

## 语句结构
### 条件判断

```js
if(expression){
    do();
}else if(expression){
    do();
}else{
    do();
}
```

### 循环
+ for循环

```js
for(i=0;i<arr.length;i++){
    console.log(arr[i]);
}
```

+ for...in循环

```js
for(var key in zhi){
    console.log(key);
}

for(var i in arr){
    console.log(a[i]); //注意对Array的循环得到String
}
```

+ while循环
+ do while循环

## 数据类型

+ ===比较运算符不会转换数据类型
+ ==会自动转换数据类型，慎用！
+ isNaN()函数  :not a number
+ abs()绝对值

### 变量

+ 动态语言:变量类型不固定

```js
var a=123;
a='jasdh';
```

+ 打印

```js
console.log(); //不会弹出对话框
alert(); //会弹出对话框

// 使用strict模式，强制var声明变量(不用var声明变量会视为全局变量，可能导致错误)
'use strict';
```

### 数组

```js
var arr=[1,true,'abc',null];
arr[1];             //true
arr.length;         //4
arr.indexOf('abc'); //2
```

+ slice()

```js
arr.slice(0,3);         //截取0-2部分元素，返回新的Array
arr.slice(2);           //从索引3-结束
var aCopy=arr.slice();  //复制一个Array
```

+ 向尾部添加或删除

```js
arr.push()  //返回新长度
arr.pop()   //返回弹出元素
```

+ 向头部添加或删除

```js
arr.unshift(); //返回新长度
arr.shift();   //返回删除元素
```

+ 排序

```js
arr.sort();    //排序
arr.reverse(); //逆序
```

+ 万能方法splice()

```js
arr.splice(2,3,'Alone','long'); 
/*从位置2开始删除3个元素，并插入两个元素,返回删除的所有元素组成的数组*/
```

+ concat()

```js
var added=arr.concat(2,'strong',false); //返回新的Array
```

+ join()

```js
var arr=[1,2,3];
arr.join('-'); //把所有元素用指定字符串连接起来，返回一个字符串'1-2-3'
```


### 字符串

+ ES6新标准！！多行字符串：

```js
` 多行
字符
   串`    //反引号括起来
```

普通模板字符串

```js
var message='hello,'+name;
```

+ ES6新标准！！模板字符串

```js
var message='hello,${name}';
```

+ ！！注意，字符串是常量，不会改变,对字符串的操作只会返回新的字符串

```js
var a='sdja';
a[1]='d'; //不会有变化
```

+ 一些字符串操作

```js
toUpperCase();
toLowerCase();
indexOf();
substring();
```


### 对象——无序的集合

+ 格式

```js
var zhi={  //注意有等号

    属性：值, //注意逗号隔开

    name:zhiyunayuan,

    //包含特殊字符，无效变量名
    'middle-school':'no.1 middle school';

    agent:female  //不需加逗号
 
};
```

+ 访问有效变量名

```js
zhi.name; 
zhi['name'];
```

+ 无效变量名只能通过[]访问

```js
zhi['middle-school'];
```

+ 添加/删除属性

```js
zhi.age=22;
delete zhi.age;
```

+ 检测属性(可能是继承得到的)

```js
'name' in zhi;  //true
'toString' in zhi;  //true
```

+ 检测自身拥有属性(非继承得到)

```js
zhi.hasOwnProperty('name');     //true
zhi.hasOwnProperty('toString'); //false
```

### Map和Set——ES6新增数据类型

#### Map，为了应对JavaScript对象的一个问题：键值对的键必须是字符串

```js
'use strict';

var m=new Map([['zhi',99],['jiang',98],['richael',80]]);
m.get('zhi');

var newM=new Map();
newM.set('mishell',70); //添加新的键值对
newM.set('mishell',89); //89
newM.has('mishell'); //true
newM.delete('mishell');
```

#### Set

一组key的集合，且key不重复。

```js
var s1=new Set();//创建一个空Set
var s2=new Set([1,2,3,3]);//用一个Array初始化Set,重复元素自动被过滤

s1.add(4);
s1.add(4);//自动被过滤
s1.delete(3);
```

### iterable:ES6新增类型

遍历Map和Set无法像Array使用下标，为了统一集合类型，引入iterable类型（包括Array，Map，Set）。ES6新的遍历方法：for...of

```js
var a=['a','b','c'];
var s=new Set(['A','B','C']);
var m=new Map([[1,'a'],[2,'b'],[3,'c']]);

for(var x of a)
    console.log(x); //'a','b','c'
}
for(var x of s){
    console.log(x); //'A','B','C'
}
for(var x of m){
    console.log(x[0]+'='+x[1]); //1='a',2='b',3='c'
}
```

for..in和for..of的区别：

+ for..in遍历的是对象的属性，而Array实际上也是对象，它的数组下标（索引）就是属性，而元素是属性值

+ for..of遍历的是集合的元素

更好的方式：使用iterable内置的forEach方法

+ Array的参数：element,index,array

```js
var a=['a','b','c'];
a.forEach(function(element,index,array){ //每次迭代回调一个函数
    //element:当前元素的值
    //index:当前索引
    //array:Array对象本身
    console.log(element + ', index = ' + index);
    /*A, index = 0
      B, index = 1
      C, index = 2
    */

});
```

+ Set的参数:element, sameElement, set

```js
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
```

+ Map的参数:value, key, map

```js
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```


























