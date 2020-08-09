---
description: 略读了一些章节
---

# CSS in Depth

### 相对单位

像素\(px\)是绝对单位，即 5px 放哪都一样大  
不常用的绝对单位是mm（毫米）、cm（厘米）、in（英寸）、pt（点，印刷术语，1/72英寸）、pc（派卡，印刷术语，12点）  
这些单位都可以通过公式互相换算：1in = 25.4mm = 2.54cm = 6pc = 72pt = 96px

CSS单位会根据浏览器、操作系统或者硬件适当缩放  
像素是一个具有误导性的名称，CSS像素并不严格等于显示器的像素

em 和 rem 是相对单位，它们会根据作用到的元素而变化

当智能手机出现后，开发人员再也无法假装每个用户访问网站的体验都能一样\(lll￢ω￢\)

建议用rem设置字号，但是有选择地用em实现网页组件的简单缩放

结合伪类 `:root` 以及 rem / vw，不用媒体查询也能让整个网页响应式缩放

### 理解浮动

flexbox的行为很直观，可预测性更好，浮动的学习显得有些没有必要，除非在开发中需要兼容IE系浏览器\(IE10 支持部分lexbox属性\)或者维护旧代码库。

让文字围绕图片的效果，浮动仍然是唯一的办法，这恰恰也是浮动的设计初衷。

### Flexbox

Flexible Box Layout

一行多列的布局情况下，考虑到内容加载，浏览器会重新计算每个弹性子元素的大小，用户会短暂的看到不符合预期的布局，所以整页布局一行多列的时候不推荐使用flexbox

flexbox比border-radius属性的支持范围更广（大雾

如果是行内元素，那么它给父元素贡献的高度会根据line-height计算，而不是根据padding和内容

```css
flex: 1;
/* 推荐使用简写属性 flex，flex简写会给其他子属性提供默认值
flex-grow: 1;
flex-shrink: 1;
flex-basis: 0%;
*/
```

flex-basis 的初始值是auto，如果设置了width值，则使用该值做为flex-basis，否则用元素内容自身的大小；如果flex-basis不是auto，则width属性会被忽略

flex-grow 是将容器多出来的留白分配给子元素的属性，值为非负整数，为 0 则弹性子元素的宽度不会超过flex-basis的值，非 0 则按照设置的值的比例进行分配

flex-shrink 是处理溢出宽度的属性，值为非负整数，为 0 则表示不收缩，非 0 则表示按照设置得值的比例收缩，值越大收缩得越多

[Flexbugs](https://github.com/philipwalton/flexbugs) 该项目维护了所有已知的flexbox的浏览器兼容性问题，并在大部分情况下给出了解决方案

### 响应式设计

给视口添加 [meta标签](https://developer.mozilla.org/zh-CN/docs/Mobile/Viewport_meta_tag)  
meta 标签的 content 属性里包含两个选项 1. 它告诉浏览器当解析CSS时将设备的宽度作为假定宽度，而不是一个全屏的桌面浏览器的宽度 2. 当页面加载时，它使用initial-scale将缩放比设置为100%。

```markup
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
```

优先实现移动端设计  
使用媒体查询，按照视口从小到大的顺序渐进增强网页  
使用**流式布局**（容器随视口宽度而变化）适应任意浏览器尺寸  
使用[响应式图片](https://jakearchibald.com/2015/anatomy-of-responsive-images/)适应移动设备的带宽限制

```markup
<!-- 设置src属性兼容不支持srcset属性的浏览器 -->
<!-- srcset根据浏览器宽度的不同加载不同图片 -->
<img
  alt="img"
  src="bg.jpg"
  srcset="
    bg-s.jpg 560w,
    bg-m.jpg 800w,
    bg-l.jpg 1280w
  "
/>
```

### 模块化CSS

每个模块都需要有一个独一无二的类名，如`message`，以该类名开头创建的新类名称为修饰符，如`message-error`

使用统一的命名约定，比如双连字符和双下划线，以便一眼就可以看清楚模块的结构

模块的命名应该有意义，同时也要避免使用简单地描述视觉效果的名称，可以从功能/使用场景等方面思考命名

```css
// bad
.red-box{}

// good
.alert{}
```

**单一职责原则**，模块封装的一个重要原则，每个模块应该只做一件事情

```css
// 结合Sass等预处理器，可以用多个文件来组织样式
@import 'base';
@import 'message';
@import 'button';
```

**工具类**用来对元素做一件简单明确的事，比如让文字居中、让元素左浮动，或者清除浮动，工具类是唯一应该使用`important`注释的地方

```css
.hidden {
  display: none !important;
}
```

