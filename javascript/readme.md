
# JavaScript

## 事件流有哪些阶段 ?

包括三个阶段: 事件捕获阶段, 处于目标阶段, 事件冒泡阶段

## 为什么要引入 requestAnimationFrame() ?

最平滑的动画的最佳循环间隔是 1000ms/60 约为 17ms.

之前的动画是通过 setInterval() 或者 setTimeout() 进行实现, 但他们都不是十分准确. 两个方法中, 第二个参数是将动画函数添加到 UI 线程队列的时间, 并不是真正执行动画函数的时间, 如果队列前面有其他的任务, 则需要等前面的任务完成了才会执行动画函数.

而 requestAnimationFrame() *不需要指定动画函数的执行间隔*, 浏览器会决定最佳的调用动画的间隔.

当我们需要重绘动画时, 再次调用 requestAnimationFrame() 即可.

```js
var start = null;
var element = document.getElementById('my');

function step(timestamp) {
  if (!start) start = timestamp;
  var progress = timestamp - start;
  element.style.transform = 'translateX(' + Math.min(progress / 10, 200) + 'px)';
  if (progress < 2000) {
    window.requestAnimationFrame(step);
  }
}

window.requestAnimationFrame(step);
```

## 什么是构造函数 ?

构造函数使用 `new` 进行调用, 在 ECMAScript 中, 使用构造函数来创建 *特定类型* 的对象.

创建自定义的构造函数, 从而定义 *自定义对象类型* 的属性和方法

## 构造函数 和 普通函数 有什么区别 ?

唯一区别: 调用方式不同

构造函数使用 new 进行调用

任何函数使用 new 进行调用就是构造函数, 任何函数不用 new 进行调用就是普通函数

## 什么是原型 ?

每个函数都有一个 prototype 属性, 是一个指针, 指向一个对象, 这个对象包含由特定类型的所有实例共享的属性和方法

好处: 让所有对象实例共享原型对象上所包含的属性和方法

## 什么是原型链 ?

通过构造函数 `new` 调用的实例, 会有一个内部指针 `[[Prototype]]` 指向构造函数的原型对象,

当读取某个对象的属性时, 会对该名字的属性执行一次搜索,

搜索从对象实例本身开始, 如果在实例上找到了给定名字的属性, 返回该属性的值, 如果没找到, 则搜索实例上 `[[Prototype]]` 指针指向的原型对象, 在原型对象上查找具有给定名字的属性

## 如何构建一个原型链 ?

将一个类型的实例, 赋给另一个构造函数的原型

