# 第一章 函数式编程简介

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