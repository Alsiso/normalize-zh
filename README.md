## 前言
Normalize-zh.css是根据对Normalize.css的源码分析后，经过学习与整理，将源代码中的英文注释文档翻译为中文版本，方便国内的开发者学习和使用，我深知此版本一定有很多不足，希望能得到大家的理解和支持，同样也很愿意和大家一起完善。

关于源码的解读细节，可以查看文档下方，可以阅读我发布在segmentfault上的系列文章

* [关于CSS Reset 那些事（二）之 Normalize.css 源码解读](http://segmentfault.com/a/1190000003025718)
* [关于CSS Reset 那些事（三）之 Normalize-zh.css 出炉](http://segmentfault.com/a/1190000003028985)


## Normalize 源码解读

* 源码地址：[https://github.com/necolas/normalize.css/blob/master/normalize.css][21]
* 源码版本：v3.0.3

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
1. 设置全局的字体为sans-serif，关于中文字体的设置可参考 [Amaze UI][3]
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

这个问题想必大家都已经非常清楚，当低版本浏览器遇到不识别的元素时，会默认把他们当成内联元素(`inline`)，这里重新定义成为`block`元素。

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

`progress`是HTML5的新标签，可以定义进度条，但是它Chrome, Firefox, 和Opera并没有已baseline垂直对齐。


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

在IE8之前的浏览器是不支持`controls`属性，这里的办法是直接隐藏该元素

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

* 去掉点击时的`outline`焦点框，同时保证使用键盘可以显示焦点框，这个操作针对所有浏览器



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

[`dfn `][4] 标签可标记那些对特殊术语或短语的定义，在Safari 和Chrome 里不是斜体，在这里统一了样式。

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

* 修复 Firefox 4+，Safari 5 和 Chrome 中`section`和`article`内的`h1`字体大小

```
/**
 * Address styling not present in IE 8/9.
 */

mark {
  background: #ff0;
  color: #000;
}
```

* 修复 IE 6/9， Safari 5 和 Chrome中样式不呈现的问题

`mark`标签用来突出显示部分文本，设置后会有一个高亮背景，但是此标签是HTML5中的新标签，在低版本浏览器并不识别，所以需要重置样式。

```

/**
 * Address inconsistent and variable font size in all browsers.
 */

small {
  font-size: 80%;
}
```

* 在所有浏览器中统一`small`的字体大小

`small`标签在 HTML 4.01 就已经存在，HTML5 中增强了它的寓意，表示旁注信息，不过此标签在各个浏览器中呈现的字体大小不一样，在这里做了统一



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

`sup`和`sub`两个标签是用来表示上标和下标，据HTML标准中对`small`，`sub`和`sup`的大小要求都是`smaller`，但是如上所示`normalize.css`给`small`设的是80%，而`sub`和`sup`却是75%，所以为了保持一致，且不影响其他元素的行高，把两者的`line-height`设为`0`，然后设置它的垂直以baseline开始，设置`top`和`bottom`手动设置两者偏移量

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

在旧版本的浏览器中，图片默认会有一个奇丑无比的蓝色边框，这这里进行清除，统一样式。

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

`figure` 是HTML5的新标签，用做文档插图，但它在 IE 8/9 and Safari 中的默认`margin`失效，这里做了统一设置。

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

在 Firefox 中，`hr`元素的默认样式很多，和其它浏览器主要的差异有两点：
1.设置了`height`为`2px`;
2.`box-sizing`为`border-box`;
此样式对这两个问题进行重置，进行统一


```
/**
 * Contain overflow in all browsers.
 */

pre {
  overflow: auto;
}
```

* 标签设置滚动条，内容溢出时出现

大部分浏览器的`pre`在`overflow`的时候会直接溢出去，这里加上`overflow:auto`让它出现滚动条

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

* 用于修复 Safari 5 和 Chrome 中奇怪的字体设置，统一字体样式

### 表单 Forms
```
/**
 * 1. Correct color not being inherited.
 *    Known issue: affects color of disabled elements.
 * 2. Correct font properties not being inherited.
 * 3. Address margins set differently in Firefox 4+, Safari, and Chrome.
 */

button,
input,
optgroup,
select,
textarea {
  color: inherit; /* 1 */
  font: inherit; /* 2 */
  margin: 0; /* 3 */
}
```

1. 修正所有浏览器中颜色不继承的问题
2. 修正所有浏览器中字体不继承的问题
3. 修正 Firefox 3+， Safari5 和 Chrome 中外边距不同的问题

有一些浏览器会把`form`表单中的一些元素 `textarea`，`text`，`button`，`select` 中的字体和字体颜色默认会设置成用户的字体或者是浏览器的字体，并不会从父元素继承，所以这里重置了这些元素的默认样式。

```
/**
 * Address `overflow` set to `hidden` in IE 8/9/10/11.
 */

button {
  overflow: visible;
}
```

* 统一 IE 8/9/10/11 `overflow`属性为visible

在 IE 8/9/10/11里的`button`默认的`overflow`是`hidden`，这里统一为`visible`

```
/**
 * Address inconsistent `text-transform` inheritance for `button` and `select`.
 * All other form control elements do not inherit `text-transform` values.
 * Correct `button` style inheritance in Firefox, IE 8/9/10/11, and Opera.
 * Correct `select` style inheritance in Firefox.
 */

button,
select {
  text-transform: none;
}
```

* 统一各浏览器`text-transform`不会继承的问题

```
/**
 * 1. Avoid the WebKit bug in Android 4.0.* where (2) destroys native `audio`
 *    and `video` controls.
 * 2. Correct inability to style clickable `input` types in iOS.
 * 3. Improve usability and consistency of cursor style between image-type
 *    `input` and others.
 */

button,
html input[type="button"], /* 1 */
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button; /* 2 */
  cursor: pointer; /* 3 */
}
```

1. 避免 Android 4.0.* 中的 WebKit bug ，该bug会破坏原生的`audio`和`video`的控制器
2. 更正 iOS 中无法设置可点击的`input`的问题
3. 统一其他类型的`input`的光标样式

这里将可点击的按钮，统一设置鼠标样式为`pointer`，提高了可用性

```
/**
 * Re-set default cursor for disabled elements.
 */

button[disabled],
html input[disabled] {
  cursor: default;
}
```

* 重置按钮禁用时光标样式

```
/**
 * Remove inner padding and border in Firefox 4+.
 */

button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
```

* 移除 Firefox 4+ 的内边距

```
/**
 * Address Firefox 4+ setting `line-height` on `input` using `!important` in
 * the UA stylesheet.
 */

input {
  line-height: normal;
}
```

* 统一设置行高为normal

Firefox浏览器会默认设置input[type="button"]的行高为`normal !important`，这里统一样式

```
/**
 * It's recommended that you don't attempt to style these elements.
 * Firefox's implementation doesn't respect box-sizing, padding, or width.
 *
 * 1. Address box sizing set to `content-box` in IE 8/9/10.
 * 2. Remove excess padding in IE 8/9/10.
 */

input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box; /* 1 */
  padding: 0; /* 2 */
}
```

1. 修正 IE 8/9 box-sizing 被设置为`content-box`的问题
2. 移除 IE 8/9 中多余的内边距

```
/**
 * Fix the cursor style for Chrome's increment/decrement buttons. For certain
 * `font-size` values of the `input`, it causes the cursor style of the
 * decrement button to change from `default` to `text`.
 */

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
```

* 修正 Chrome 中 `input [type="number"]` 在特定高度和 `font-size` 时,下面一个箭头光标变成`cursor: text` [效果](http://gtms04.alicdn.com/tps/i4/T18kd8FCtaXXc_FhcF-330-350.gif)

```
/**
 * 1. Address `appearance` set to `searchfield` in Safari and Chrome.
 * 2. Address `box-sizing` set to `border-box` in Safari and Chrome.
 */

input[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  box-sizing: content-box; /* 2 */
}

```

1. 修正 Safari 5 和 Chrome 中`appearance`被设置为`searchfield`的问题
2. 修正 Safari 5 和 Chrome 中`box-sizing`被设置为`border-box`的问题

`searchfield`是CSS3的属性，它可以让一个`div`元素看上去像任何元素，但是浏览器支持性并不好，

```
/**
 * Remove inner padding and search cancel button in Safari and Chrome on OS X.
 * Safari (but not Chrome) clips the cancel button when the search input has
 * padding (and `textfield` appearance).
 */

input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
```

* 移除原生默认样式，统一`search`的输入框样式

```
/**
 * Define consistent border, margin, and padding.
 */

fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
```

* 定义一致的边框、外边距和内边距

```
/**
 * 1. Correct `color` not being inherited in IE 8/9/10/11.
 * 2. Remove padding so people aren't caught out if they zero out fieldsets.
 */

legend {
  border: 0; /* 1 */
  padding: 0; /* 2 */
}
```

1. 修正 IE 6-9 中颜色不能继承的问题
2. 重置内边距

```
/**
 * Remove default vertical scrollbar in IE 8/9/10/11.
 */

textarea {
  overflow: auto;
}
```

* 移除 IE8-11 中默认的垂直滚动条

```
/**
 * Don't inherit the `font-weight` (applied by a rule above).
 * NOTE: the default cannot safely be changed in Chrome and Safari on OS X.
 */

optgroup {
  font-weight: bold;
}
```

* 统一设置`optgroup`元素`font-weight`始终为`bold`

### 表格 Tables
```
/**
 * Remove most spacing between table cells.
 */

table {
  border-collapse: collapse;
  border-spacing: 0;
}

td,
th {
  padding: 0;
}

```

* 合并单元格边框，重置内边距



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