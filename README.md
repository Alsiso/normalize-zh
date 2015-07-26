### 源码解析
虽然提供了详细的文档，但是它并没有提供对应的中文版本，英文注释首先看起来不够清晰，其次对问题的解析程度也不够细化，而且也不包含问题案例，所以接下来会分章节对模块进行源码解读与整理。

## Normalize 源码解读
前面讲到的分模块解读，就是先黏贴一段源码，然后根据官方提供的注释进行翻译整理，案例解析，之后在进行整理。

源码地址：[https://github.com/necolas/normalize.css/blob/master/normalize.css][21]
源码版本：v3.0.3

### html与body 元素
```
/**
 * 1. Set default font family to sans-serif.
 * 2. Prevent iOS and IE text size adjust after device orientation change,
 *    without disabling user zoom.
 */
html {
  font-family: sans-serif; /* 1 */
  -ms-text-size-adjust: 100%; /* 2 */
  -webkit-text-size-adjust: 100%; /* 2 */
}
```
1. 设置全局的字体为sans-serif，关于中文字体的设置可参考 [Amaze UI][22]
2. 防止 iOS 横屏字号放大，同时保证在PC上 zoom 功能正常

第2个问题场景是这样，苹果IOS设备调整后会自动调整文字的大小，按照苹果的意图是为了提升用户体验，比如竖屏状态下是`14px`，转换为横屏时就变成了`20px`，把`text-size-adjust:100%`就不会调整字体大小了。

如果把值设置为`'text-size-adjust:none'`，那么就会导致用户无法放大缩小字体了。

```
/**
 * Remove default margin.
 */

body {
  margin: 0;
}
```

* 修复浏览器默认边距，统一效果


### HTML5 元素 display definitions
```
/**
 * Correct `block` display not defined for any HTML5 element in IE 8/9.
 * Correct `block` display not defined for `details` or `summary` in IE 10/11
 * and Firefox.
 * Correct `block` display not defined for `main` in IE 11.
 */

article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
```

* 修复 IE 8/9，HTML5新元素不能正确显示的问题，定义为`block`的元素
* 修复 IE 10/11，`details` 和 `summary` 定义为 `block` 的元素
* 修复 IE 11，`main`定义为 `block` 的元素

```
/**
 * 1. Correct `inline-block` display not defined in IE 8/9.
 * 2. Normalize vertical alignment of `progress` in Chrome, Firefox, and Opera.
 */

audio,
canvas,
progress,
video {
  display: inline-block; /* 1 */
  vertical-align: baseline; /* 2 */
}
```

1. 修复 IE 8/9，HTML5新元素不能正确显示的问题，定义为`inline-block`元素
2. 修复Chrome, Firefox, 和Opera的`progress`元素没有以baseline垂直对齐

```
/**
 * Prevent modern browsers from displaying `audio` without controls.
 * Remove excess height in iOS 5 devices.
 */
audio:not([controls]) {
  display: none;
  height: 0;
}
```

* 对不支持`controls`属性的浏览器，`audio`元素给以隐藏
* 移除iOS5设备中多余的高度

```
/**
 * Address `[hidden]` styling not present in IE 8/9/10.
 * Hide the `template` element in IE 8/9/10/11, Safari, and Firefox < 22.
 */

[hidden],
template {
  display: none;
}
```

* 修复 IE 7/8/9，Firefox 3 和 Safari 4 中`hidden`属性不起作用的问题
* 在 IE，Safari，Firefox 22- 中隐藏`template`元素

`hidden`是HTML5的新元素，可以对元素进行隐藏，但是低版本浏览器并不会识别它，这里统一做了样式。

`template`标签用于HTML模板，大家应该都是用过HTML模版开发页面，这个标签是按照实际需求添加的，但是模板又不能在界面上显示，所以这里统一了样式，兼容旧浏览器。

### 链接 Links
```
/**
 * Remove the gray background color from active links in IE 10.
 */

a {
  background-color: transparent;
}
```

* 去掉 IE 10+ 点击链接时的灰色背景


```
/**
 * Improve readability of focused elements when they are also in an
 * active/hover state.
 */

a:active,
a:hover {
  outline: 0;
}
```

* 去掉点击时的焦点框，同时保证使用键盘可以显示焦点框

### 语义化文本标签 Text-level semantics
```
/**
 * Address styling not present in IE 8/9/10/11, Safari, and Chrome.
 */

abbr[title] {
  border-bottom: 1px dotted;
}
```

* 修正`abbr`元素在 Firefox 外其他浏览器没有下划线的问题

语义`abbr`标签是表示简称或缩写，自身的`title`属性是完整版，但是此标签在Firefox下默认有下边框，而其他浏览器中没有，这里统一了样式。

```
/**
 * Address style set to `bolder` in Firefox 4+, Safari, and Chrome.
 */

b,
strong {
  font-weight: bold;
}
```

* Firefox3+，Safari4/5 和 Chrome 中统一设置为粗体

Firefox 3+, Safari 和 Chrome 给`b`和`strong`设置的属性是`bolder`，而不是`bold`，这里统一了样式。

```
/**
 * Address styling not present in Safari and Chrome.
 */

dfn {
  font-style: italic;
}
```
* 修正 Safari5 和 Chrome 中没有样式的问题

[`abbr`][23] 标签可标记那些对特殊术语或短语的定义，在Safari 和Chrome 里不是斜体，在这里统一了样式。

```
/**
 * Address variable `h1` font-size and margin within `section` and `article`
 * contexts in Firefox 4+, Safari, and Chrome.
 */

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
```

* 对`h1`样式进行重置

```
/**
 * Address styling not present in IE 8/9.
 */

mark {
  background: #ff0;
  color: #000;
}
```

* 修正 IE6-11 中没有样式的问题

```

/**
 * Address inconsistent and variable font size in all browsers.
 */

small {
  font-size: 80%;
}
```

* 统一`small`的字体大小

```
/**
 * Prevent `sub` and `sup` affecting `line-height` in all browsers.
 */

sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}

sup {
  top: -0.5em;
}

sub {
  bottom: -0.25em;
}
```

* 防止所有浏览器中的`sub`和`sup`影响行高

### 内嵌元素 Embedded content
```
/**
 * Remove border when inside `a` element in IE 8/9/10.
 */

img {
  border: 0;
}
```

* 去除 IE6-9 和 Firefox 3 中`a`内部`img`元素默认的边框

```
/**
 * Correct overflow not hidden in IE 9/10/11.
 */

svg:not(:root) {
  overflow: hidden;
}
```

* 修复 IE9 中的`overflow`的怪异表现

### 群组元素 Grouping content
```
/**
 * Address margin not present in IE 8/9 and Safari.
 */

figure {
  margin: 1em 40px;
}
```

* 修复 IE 8/9、Safari中margin失效

```
/**
 * Address differences between Firefox and other browsers.
 */

hr {
  box-sizing: content-box;
  height: 0;
}
```

* 修正 Firefox 和其他浏览器之间的差异

```
/**
 * Contain overflow in all browsers.
 */

pre {
  overflow: auto;
}
```

* 标签应当包含滚溢出

```
/**
 * Address odd `em`-unit font size rendering in all browsers.
 */

code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
```

* 统一代码的字体设置 



  [1]: http://necolas.github.io/normalize.css/
  [2]: http://amazeui.org/css/normalize?_ver=2.x
  [3]: http://www.w3cfuns.com/topic-12.html
  [4]: http://tantek.com/log/2004/undohtml.css
  [5]: http://yui.github.io/yui2/
  [6]: http://meyerweb.com/eric/tools/css/reset/index.html
  [7]: http://yuilibrary.com/yui/docs/cssreset/
  [8]: http://docs.kissyui.com/index-1.1.6.html
  [9]: https://github.com/necolas/normalize.css
  [10]: http://www.zhihu.com/question/20094066
  [11]: http://segmentfault.com/q/1010000002991779/a-1020000002994616
  [12]: http://segmentfault.com/q/1010000000117189
  [13]: http://jerryzou.com/
  [14]: http://nicolasgallagher.com/about-normalize-css/
  [15]: http://jerryzou.com/posts/aboutNormalizeCss/
  [16]: https://twitter.com/necolas
  [17]: https://twitter.com/jon_neal
  [18]: http://aliceui.org/docs/framework.html#base-%E9%87%8D%E8%AE%BE
  [19]: http://amazeui.org/css/normalize?_ver=2.x
  [20]: https://github.com/necolas/normalize.css/
  [21]: https://github.com/necolas/normalize.css/
  [22]: http://amazeui.org/css/typography
  [23]: http://www.w3school.com.cn/tags/tag_dfn.asp