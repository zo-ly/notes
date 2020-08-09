---
description: 一点记录
---

# 函数式编程入门经典

## 第一章 函数式编程简介

### 一些编程范式

_命令式编程范式_\(imperative\)\): 让机器按照规定的命令执行下去，即一步一步告诉计算机先做什么在做什么

_声明式编程_\(declarative\)：是以数据结构的形式来表达程序执行的逻辑。它的主要思想是告诉计算机应该做什么，但不指定具体要怎么做。

_函数式编程_\(functional\)最重要的特点是“函数第一位”，即函数可以出现在任何地方，把函数作为参数传递给另一个函数，不仅如此你还可以将函数作为返回值。

### 函数式编程

创建仅依赖输入就可以完成自身逻辑的函数

函数式编程和声明式编程是有所关联的，因为他们思想是一致的：即只关注做什么而不是怎么做。但函数式编程不仅仅局限于声明式编程。

* 函数式编程的好处
  * 大多说函数式编程的好处来自于纯函数
* 纯函数
  * 给定输入返回相同输出，不改变外部环境
* 纯函数的好处
  1. 可测试
  2. 可读性高
  3. 可缓存
  4. 不用担心代码并发执行
* JavaScript更像一种多范式语言，但是非常适合函数式编程范式

## 第二章 JavaScript函数基础

* 使用箭头函数代码要简洁得多
* `var` 在函数内不管在哪个块，总是全局的定义变量
* `babel` `bael-cli` `bael-node` 的配置和使用

## 第三章 高阶函数

### 一等公民

当一门语言允许函数作为任何其他数据类型使用时，函数被称为这门语言的一等公民

> 在编程语言中，一等公民可以作为函数参数，可以作为函数返回值，也可以赋值给变量。  
> In general, a value in a programming language is said to have ﬁrst-class status if it can be passed as a parameter, returned from a subroutine, or assigned into a variable.  
> --《[Programming Language Pragmatics](https://www.cs.rochester.edu/~scott/pragmatics/)》
>
> 函数作为一等公民是函数式编程的必要条件，高阶函数\(higher-order functions\)，就是使用函数作为参数的函数，它在函数式编程中很常见  
> First-class functions are a necessity for the functional programming style, in which the use of higher-order functions is a standard practice.  
> --[wikipedia](https://en.wikipedia.org/wiki/First-class_function)

高阶函数式是接受函数作为参数 _并且_ \| _或者_ 返回函数作为输出的函数

* 高阶函数就是定义抽象
* 抽象让我们专注于预定的目标而无须关心底层的系统概念
* 大多数高阶函数都会与闭包一起使用

## 第四章 闭包与高阶函数

为什么叫 “闭包（Closure）”

> a function which **closes over** the **environment\(scope\)** in which it was defined  
> 在一个封**闭**的此法作用域中，将某些自由变量**包**在定义它的函数中

* 闭包就是一个**内部函数**（另一个函数内部的函数），有三个可访问的作用域 1. 自身声明的 2. 全局变量的 3. **外部函数的**
* 闭包的缺点，常驻内存，内存消耗对脚本性能产生负面影响

## 第五章 数组的函数式编程

* 把函数应用于一个值并创建一个新值的过程称为投影（不改变自身而产生新值）
* 设置累加器并遍历数组以生成一个单一元素的过程称为归约数组，重复该过程称为归约操作
* 投影函数 `map` `filter` `concat` `reduce` `zip`，让使用数组变得简单，理解这些函数的运行机制有助于对函数式有更加深入的思考

```javascript
const map = (array, fn) => {
  let results = [];
  for (let value of array) {
    results.push(fn(value));
  }
  return results;
}

const filter = (array, fn) => {
  let results = [];
  for (let value of array) {
    fn(value) && results.push(value);
  }
  return results;
}

const concatAll = (array) => {
  let results = [];
  for(let value of array) {
    results.push.apply(results, Array.isArray(value) ? value : [value]);
  }
  return results;
}

const reduce = (array, fn, initValue) => {
  let acc = array[0];
  if (initValue !== undefined) {
    acc = initValue;
    for (const value of array) {
      acc = fn(acc, value);
    }
  } else {
    for(let i = 1;i < array.length;i++) {
      acc = fn(acc, array[i]);
    }
  }
  return acc;
}

const zip = (leftArr, rightArr, fn) => {
  let results = [];
  const maxLen = Math.min(leftArr.length, rightArr.length);
  for (let i = 0;i < maxLen;i++) {
    results.push(fn(leftArr[i], rightArr[i]));
  }
  return results;
}

const arrayUtils = {
  map,
  zip,
  filter,
  reduce,
  concatAll
};
```

## 第六章 柯里化与偏应用

* 柯里化就是把一个多参数函数转换为一个嵌套的一元函数的过程
  * 只接受一个参数的函数称为一元函数，接受两个参数的函数称为二元函数，接受n个参数的函数称为n元函数
  * 接受可变数量的参数称为变参函数，通过`arguments`可捕获该函数的额外参数
  * es6的扩展运算符，更简洁、更清晰的表示一个函数接受可变的参数
* 柯里化函数有助于移除很多函数中的样板代码（boilerplate code，几乎不变的、重复的代码）

`arguments` 是一个类数组对象，是所有（非箭头）函数中都可用的局部变量

```javascript
// 类数组对象
var my_object = {
  '0': 'zero',
  '1': 'one',
  '2': 'two',
  '3': 'three',
  '4': 'four',
  length: 5
};
```

`Array.prototype.slice.call(arguments)` 将 `arguments` 转换为一个真正的数组 [参考](https://stackoverflow.com/questions/7056925/how-does-array-prototype-slice-call-work)

```javascript
const curry = (fn) => {
  if (typeof fn !== 'function') {
    throw Error('no function provided');
  }
  return function curried(...args) {
    if (args.length < fn.length) {
      return function () {
        return curried.apply(null, args.concat([].slice.call(arguments)));
      };
    }
    return fn(...args);
  }
}
```

* 偏函数的应用称为偏应用
* 柯里化和偏函数都是将多参函数转换为接受更少参数函数的方法，区别在于
  * 柯里化是将函数转换为多个嵌套的一元函数
  * 偏函数可以接受不只一个参数，它被固定了部分参数作为预设，并可以接受剩余的参数

```javascript
const partial = (fn, ...partialArgs) => {
  return function (extraArg) {
    let args = [].concat(partialArgs);
    for (let i = 0;i < args.length;i++) {
      if (args[i] === undefined) {
        args[i] = extraArg;
      }
    }
    return fn.apply(null, args);
  };
}
```

* 我们需要 `curry` 或者 `partial`, 但并不是同时需要
* 柯里化和偏应用一直是函数式编程的工具，该用 `curry` 还是 `partial` 应该视情况而定，取决于具体场景

## 第七章 组合与管道

Unix的一些理念

* 每个程序只做好一件事情
* 重新构建要好于在复杂的旧程序中添加“新属性”
* 每个程序的输出应该是另外一个尚未可知的程序的输入

组合与管道

* 无需创建新的函数就可以通过基础函数解决问题
* 小函数 组合为 大函数，简单的函数容易阅读、测试和维护
* 组合与管道做相同的事情，只是数据流方向不同而已
* 从左至右处理数据流的过程称为管道（pipeline）或序列（sequence）

小巧的 `compose` 或 `pipe` 用处很大，能够让开发者通过定义良好的小函数按需组合成复杂的函数

```javascript
const compose = (...fns) =>
  (value) => fns.reverse().reduce((pre, current) => current(pre), value);

const pipe = (...fns) =>
  (value) => fns.reduce((pre, current) => current(pre), value);

const identity = (it) => {
  console.log('[debug]', it);
  return it;
}
```

## 第八章 函子\(Functor\)

用一种纯函数式的方式帮助我们处理错误

函子是一个实现了map契约的对象，在遍历每个对象值得时候生成一个新的对象

* 函子是一个持有值的容器
* 函子实现了map方法（单纯的持有值是几乎没有应用场景的）
* Pointed函子是一个函子的子集，它具有实现了`of`契约的接口

```javascript
const Container = function(val) {
  this.value = val
}

Container.of = function(val) {
  return new Container(val)
}

Container.prototype.map = function(fn) {
  return Container.of(fn(this.value))
}
```

实现map函数的方式提供了不同类型的函子

```javascript
/*
  MayBe函子
  1. 避免麻烦的 null 或 undefined 检查
  2. 所有map函数都会被调用
*/
const MayBe = function(val) {
  this.value = val;
}

MayBe.of = function(val) {
  return new MayBe(val);
}

MayBe.prototype.isNothing = function() {
  return !this.value
}

MayBe.prototype.map = function(fn) {
  return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this.value));
}

const splitStr = (str) => str.split(' ');
MayBe.of(null).map(splitStr).map(splitStr);

/*
  Either函子
  确定报错原因，以便分析问题
*/
const Nothing = function(val) {
  this.value = val;
}
Nothing.of = function(val) {
  return new Nothing(val);
}
Nothing.prototype.map = function(fn) {
  return this;
}

const Some = function(val) {
  this.value = val;
}
Some.of = function(val) {
  return new Some(val);
}
Some.prototype.map = function(fn) {
  return Some.of(fn(this.value));
}

let Either;
try {
  Either = Some.of(/*sth.*/)
} catch {
  Either = Nothing.of(/*err msg*/)
}
Either.map(/*do sth.*/).map(/*do sth.*/)
```

## 第九章 深入理解 Monad

Monad有助于扁平化 `MayBe`数据

* Monad就是一个含有 `chain` 方法的函子

```javascript
MayBe.prototype.join = function() {
  return this.isNothing() ? MayBe.of(null) : this.value;
}

MayBe.prototype.chain = function(fn) {
  return this.map(fn).join()
}
```

Monad和函子\(Functor\)是函数式领域的概念，如果需要深入理解，可研读《[Scala函数式编程](https://book.douban.com/subject/26772149/)》

## 第十章 使用 Generator

书中使用 Generator 主要是用来解决 JavaScript 中的异步回调问题

* 同步：函数在执行时会阻塞调用者，并在执行完毕后返回结果
* 异步：函数在执行时不会阻塞调用者，一旦执行完毕就会返回结果
* 惰性求值：代码直到调用时才会执行
* Generator 是 ES6 规范的一部分，被捆绑在语言层面
  * 不能无限制地调用 `next` 从 Generator 中取值，Generator 如同序列，一旦序列中的值被消费，就不能再次消费它
  * `yield` 使 Generator 函数暂停了执行并将结果返回给调用者
  * `done` 是一个判断 Generator 序列是否已被完全消费的属性

