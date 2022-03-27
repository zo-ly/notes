---
description: 过了一下文档，做了一点总结
---

# Babel 浅入浅出

## Babel 的安装

```shell
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

## Babel 可以做什么

- Babel 就是一个 js 代码转换器，将新的es语法转换为 es5 或者 es3 的代码，从而是最终的运行环境（浏览器/node环境等）可以执行代码
- Babel 借助 core-js 可以给 js 代码添加 polyfill，以便最终在浏览器运行的代码能够正常使用那些 api；Babel 始终能提供对最新 es 提案的支持
- 借助 Babel 可以将一些框架编写的格式转换标准的 es 代码，如 `jsx` `.vue` 文件
- js本身是弱类型语言，如果产品用户量很大，一些小错误也会导致严重的 bug，所以就有了 `flow` `typescript` 两种强类型的编程方式，借助强类型的语言静态分析能力，可以规避不少问题，Babel 也能将其转换为标准的 es 代码
- 在 node 环境中，Babel 提供了`babel-register`，实现 ES6 的文件被 node 加载时自动转换为 ES5 的代码

> 有些大厂团队，利用Babel在做源码方面的处理(codemods)，因为大厂人多，每个人开发习惯都有差异，为了规范代码风格，提高代码质量，借助Babel，将每个人的源码做一定的标准化转换处理，能够提升团队整体的开发质量
>
> node 运行环境，目前对 es6 的 modules 支持不是很好，Babel 提供了另外解决方案，可以把 es6 编写的 modules 在被require 的时候，自动进行代码转换，虽然没法评定这个方案的优劣，但是也能感受到 Babel 为了让开发人员能够使用最新ES语法这方面确实很努力

## Babel 的使用

### 不对 Babel 不做任何配置，一段 ES6 的代码经过 Babel 处理，还是原来那段代码

### Babel 的功能都是通过 plugin 来完成的

------

开发中最常见的方式，与 webpack、gulp 等构建工具结合起来使用，`babel-loader`等

```js
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['@babel/preset-env', { targets: "defaults" }]
          ],
          plugins: ['@babel/plugin-proposal-class-properties']
        }
      }
    }
  ]
}
```

------

最底层的直接利用 api 调用 Babel

```js
const fs = require('fs');
const babel = require('@babel/core');

const source = fs.readFileSync('./main.js').toString();

babel.transform(source, {
    configFile: '../babel.config.js'
}, function(err, result) {
    const { code, ast } = result;

    console.log(code);
    console.log(ast);
});
```

------

配合 config 文件进行设置使用，如：`babel.config.js` `.babelrc.js` `babel.config.json` `.babelrc`

```json
{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
          "ios": "10"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

也可以在 `package.json` 中配置

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "babel": {
    "presets": [ ... ],
    "plugins": [ ... ],
  }
}
```

------

Babel 提供了命令行工具`babel-cli`，可以直接通过命令行对文件或文件夹进行转换

```shell
npx babel script.js --out-file script-compiled.js --source-maps
```

## Polyfill

从 Babel 7.4.0 开始已经废弃了 `@babel/polyfill`

Babel 对代码的 polyfill，是利用另外两个库来做的：`core-js` (to polyfill ECMAScript features)和regenerator-runtime

core-js 目前升级到了3.x版本，跟2.x区别很大；regenerator-runtime 没有什么变化

当 core-js 升级的时候，preset-env 也会升级，所以能调整要注入的 polyfill。 这一层都是 babel 在做的，开发者无需关心

`core-js@3`现在是一个完全模块化的标准库，每个 polyfill 都是一个单独的文件，所以除了全部引入，还可以考虑单独引入，这样能够减少浏览器等运行环境已经实现了的特性的 polyfill

> [core-js](https://github.com/zloirock/core-js) 还是值得学习的，将来很有可能会直接使用这个库里面的东西，所以需要掌握它是如何组织ES的各个模块实现的

## Plugins

`plugin`是指导 Babel 如何执行代码转换的小型 JavaScript 程序，如：`transform-typeof-symbol`，`plugin-transform-arrow-functions`

分为三类：

- syntax 语法类
- transform 转换类
- proposal 也是转换类，指代那些对 ES Proposal 进行转换的 plugin （如：`lastItem`）

syntax plugin 会被 transform plugin 依赖，用于语法解析

## Presets

为了避免一个一个地添加`plugin`，Babel提供了`presets`，是一个预先确定的插件集，`@babel/preset-env` 就是 Babel 官方定义的`presets`

可以创建自己的 `presets` 来定义你需要的插件组合

看一些例子：

- `@babel/preset-env` [link](https://github.com/babel/babel/tree/main/packages/babel-preset-env)
- `babel-preset-gatsby` [link](https://github.com/gatsbyjs/gatsby/tree/master/packages/babel-preset-gatsby)

## @babel/runtime

1. 它跟 preset-env 提供的 polyfill 适用的场景是完全不同，runtime 适合开发库，preset-env 适合开发 application
2. runtime 与 preset-env 的 **polyfill** 不能同时启用
3. runtime 的 polyfill 不判断目标运行环境

因为 Babel 在转换过程中，会利用很多Babel自己的工具函数：helpers。在不经过优化的时候，每个文件都会单独包含这些helpers 代码，如果文件很多，就会导致大量的重复代码，所以 Babel 专门推出了`transform-runtime`来对这些 helpers 进行自动提取和其它优化

## 其他

Babel 一系列的包都以`@babel`开头，是 npm 包的一种形式，@符号开始的包，如`@babel/preset-env`，代表的是一类有 scope 限定的 npm 包，作用是只有 scope 的主体公司、机构和个人，才能往这个 scope 里面添加新的包；所以以 @ 开头的包具有权威性，一定是官方推出或者认可的包

Browserslist 和 Babel 的结合，与 eslint/stylelint 的结合

## 参考

- [1] [Babel Docs](https://babeljs.io/docs/en/)
- [2] [大佬的 Blog](https://blog.liuyunzhuge.com/categories/Javascript/babel/)
