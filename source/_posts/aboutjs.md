---
title: js笔记
date: 2020-05-21 14:45:28
tags: JavaScript
---
## 关于call apply bind的一些认识
```
在JavaScript中，call、apply和bind是Function对象自带的三个方法，这三个方法的主要作用是改变函数中的this指向。

不传，或者传null,undefined， 函数中的this指向window对象
传递另一个函数的函数名，函数中的this指向这个函数的引用
传递字符串、数值或布尔类型等基础类型，函数中的this指向其对应的包装对象，如 String、Number、Boolean
传递一个对象，函数中的this指向这个对象

function (){   
  console.log(this);   
}       
function b(){}       
var c={name:"call"};    //定义对象c  
a.call();   //window
a.call(null);   //window
a.call(undefined);   //window
a.call(1);   //Number
a.call('');   //String
a.call(true);   //Boolean
a.call(b);   //function b(){}
a.call(c);   //Object

//常用例子 1
function eat(x,y){   
  console.log(x+y);   
}   
function drink(x,y){   
  console.log(x-y);   
}   
eat.call(drink,3,2);
//输出：5
//这个例子中的意思就是用 eat 来替换 drink，eat.call(drink,3,2) == eat(3,2) ，所以运行结果为：console.log(5);
//注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。
```