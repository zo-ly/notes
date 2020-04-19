# 第一章 函数式编程简介

*命令式编程范式*(指令式编程): 让机器按照规定的命令执行下去

函数式编程是一种编程范式，以此可以创建仅依赖输入就可以完成自身逻辑的函数

- 函数式编程的好处
  - 大多说函数式编程的好处来自于纯函数
- 纯函数
  - 给定输入返回相同输出，不改变外部环境
- 纯函数的好处
  1. 可测试
  2. 可读性高
  3. 可缓存
  4. 不用担心代码并发执行
- JavaScript更像一种多范式语言，但是非常适合函数式编程范式

# 第二章 JavaScript函数基础

- 使用箭头函数代码要简洁得多
- `var` 在函数内不管在哪个块，总是全局的定义变量
- `babel` `bael-cli` `bael-node` 的配置和使用

# 第三章 高阶函数

### 一等公民

当一门语言允许函数作为任何其他数据类型使用时，函数被称为这门语言的一等公民

> 在编程语言中，一等公民可以作为函数参数，可以作为函数返回值，也可以赋值给变量。  
> In general, a value in a programming language is said to have ﬁrst-class status if it can be passed as a parameter, returned from a subroutine, or assigned into a variable.  
--《[Programming Language Pragmatics](https://www.cs.rochester.edu/~scott/pragmatics/)》


>函数作为一等公民是函数式编程的必要条件，高阶函数(higher-order functions)，就是使用函数作为参数的函数，它在函数式编程中很常见  
>First-class functions are a necessity for the functional programming style, in which the use of higher-order functions is a standard practice.  
--[wikipedia](https://en.wikipedia.org/wiki/First-class_function)

高阶函数式是接受函数作为参数 *并且* | *或者* 返回函数作为输出的函数

- 高阶函数就是定义抽象
- 抽象让我们专注于预定的目标而无须关心底层的系统概念
- 大多数高阶函数都会与闭包一起使用

# 第四章 闭包与高阶函数

为什么叫 “闭包（Closure）”

> a function which **closes over** the **environment(scope)** in which it was defined  
>在一个封**闭**的此法作用域中，将某些自由变量**包**在定义它的函数中

- 闭包就是一个**内部函数**（另一个函数内部的函数），有三个可访问的作用域
  1. 自身声明的
  2. 全局变量的
  3. **外部函数的**

- 闭包的缺点，常驻内存，内存消耗对脚本性能产生负面影响

# 第五章 数组的函数式编程

- 把函数应用于一个值并创建一个新值的过程称为投影（不改变自身而产生新值）
- 设置累加器并遍历数组以生成一个单一元素的过程称为归约数组，重复该过程称为归约操作
- 投影函数 `map` `filter` `concat` `reduce` `zip`，让使用数组变得简单，理解这些函数的运行机制有助于对函数式有更加深入的思考

```js
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
