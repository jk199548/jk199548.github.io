---
title: JavaScript
date: 2020-06-05 14:48:49
tags: js
---
## 用于展示一些常见的js问题
#### 1、函数防抖(防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。)
```
function debounce(fn, wait) {
    var timeout = null;
    return function() {
        if(timeout !== null) 
                clearTimeout(timeout);
        timeout = setTimeout(fn, wait);
    }
}
// 处理函数
function handle() {
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));
```
## 2、节流（throttle）所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。 节流会稀释函数的执行频率。
```
1、时间戳版本
var throttle = function(func, delay) {
    var prev = Date.now();
    return function() {
        var context = this;
        var args = arguments;
        var now = Date.now();
        if (now - prev >= delay) {
            func.apply(context, args);
            prev = Date.now();
        }
    }
}
function handle() {
    console.log(Math.random());
}
window.addEventListener('scroll', throttle(handle, 1000));

2、定时器版本
var throttle = function(func, delay) {
    var timer = null;
    return function() {
        var context = this;
        var args = arguments;
        if (!timer) {
            timer = setTimeout(function() {
                func.apply(context, args);
                timer = null;
            }, delay);
        }
    }
}
function handle() {
    console.log(Math.random());
}
window.addEventListener('scroll', throttle(handle, 1000));

3、时间戳加定时器
var throttle = function(func, delay) {
     var timer = null;
     var startTime = Date.now();
     return function() {
             var curTime = Date.now();
             var remaining = delay - (curTime - startTime);
             var context = this;
             var args = arguments;
             clearTimeout(timer);
              if (remaining <= 0) {
                    func.apply(context, args);
                    startTime = Date.now();
              } else {
                    timer = setTimeout(func, remaining);
              }
      }
}
function handle() {
      console.log(Math.random());
}
 window.addEventListener('scroll', throttle(handle, 1000));
```