# HTML5 学习笔记

[toc]

## 基本概念

### 关于

- **全称：** 超文本标记语言（Hyper Text Markup Language）。
- **标记语言。** HTML 不是一种编程语言，而是一种标记语言（markup language）。
- **编码。** HTML5 中默认的字符编码是 UTF-8。
- **空格空行忽略。** 当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格、空行都会被算作一个空格。
- **大小写不敏感。** 但是推荐使用小写。
- **文件路径：** 与 Linux 系统文件路径逻辑类似，`..` 代表上层文件夹，`/ ` 代表网站根目录，默认相对路径以当前文件位置始。亦可使用以 URL 表示的绝对文件路径。

### 编码和字符集

- **从 ASCII 到 UTF-8。**
  - ASCII：
    - 0 到 31（以及 127）之间的值为控制字符。
    - 32 到 126 的值表示字母、数字和符号。
    - 不使用 128 到 255 之间的值。
  - UTF-8：
    - 0 到 127 的值 UTF-8 与 ASCII 相同。
    - 不使用 128 到 159 之间的值。
    - 对于 160 到 255 之间的值，UTF-8 与 ANSI 和 8859-1 相同。
    - UTF-8 从值 256 继续，包含超过 10000 个不同字符。
- **文档编码：** 使用 `<meta charset="UTF-8">` 指定 UTF-8 字符集。
- **URL 编码：** URL 编码使用 `%` 后跟随两位十六进制数来替换非 ASCII 字符。使用 `+` 来替换空格。[参见](https://www.w3school.com.cn/tags/html_ref_urlencode.asp)。

#### 字符实体

- HTML 中有部分保留字符。为了显示这些字符，必须要使用字符实体替代。
  - 空格 `&nbsp;`
  - 小于号 `&lt;`
  - 大于号 `&gt;`
  - 与符号 `&amp;`
  - 双引号 `&quot;`
- 用 `&#num;` 显示 UTF-8 编号为 num 的字符。

### 语法概念

- **文档：** 就是页面。

  - 推荐使用 `.html` 拓展名而不是 `.htm`。

  - 一个最基本的 HTML5 实例：

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="UTF-8">
    	<title>Title of the document</title>
    </head>
    
    <body>
    	Content of the document......
    </body>
    
    </html>
    ```

- **标签：** 由尖括号包围的关键词，比如 ` <html>`。

  - **成对。** 通常成对出现，如 `<html></html>`。分别为开始标签（开放标签）和结束标签（闭合标签）。

- **元素：** HTML 元素指的是从开始标签到结束标签的所有代码。可以看成「内容」。

  - **空元素：** 在开始标签中结束的元素，如 `<br>`。`<br />` 是更好的方式。

- **属性：** 标签中的名称/值对，如 `name="value"`。HTML5 支持四种属性语法：

  - **空 Empty：**  `<input type="text" value="John Doe" disabled>`
  - **无引号 Unquoted**：  `<input type="text" value=John Doe>`
  - **双引号 Double-quoted**：  `<input type="text" value="John Doe">`
  - **单引号 Single-quoted**：  `<input type="text" value='John Doe'>`

## 标签 Tags

### 文档类型声明

文章首行应声明文档类型：`<!DOCTYPE html>`。

### 布局

#### 主部分 body

body 的属性在最新的标准中已经被废弃。

#### 框架 frameset 

用于在框架内显示网页。这个标签不能和 `body` 同时使用。

```html
<frameset cols="25%,75%">
   <frame src="frame_a.htm">
   <frame src="frame_b.htm">
</frameset>
```

- **rows、columns 属性：** 规定了每行每列占屏幕面积。如 `10%,50%,40%`。
- 其中的 frame 标签定义框架中的 html 文档。
  - **src 属性：** 源。
  - **noresize 属性：** 设为 noresize 禁止用户手动调整边框。否则用户可以拖动边框调整大小。

#### 块和内联 div/span

- **块级元素（block）：** h1，p，ul，table 等，一般以新行开始。
  - `<div>` 可以作为块级元素容器。
- **内联元素（inline）：** b，td，a，img 等，通常不会以新的行开始。
  - `<span> ` 可作为内联元素的容器。

#### 头部 head 中

HTML5 中 head 是可以省略的。默认把 body 之前的部分都认为是 head。

- **头部容器 Head：** `<head>` 标签中包含脚本，样式表引用、元信息等。以下标签都可以添加到 `<head>`。
- **标题 Title：** 定义文档的标题。
- **Base：** 为页面上的所有链接规定默认地址或默认目标（target），如 `<base target="_blank" />`。
- **Link：** 定义文档与外部资源之间的关系。`<link rel="stylesheet" type="text/css" href="style.css" />`。
- **Style：** 定义一些样式信息。其中写 css 代码。
- **Meta：** 提供关于 HTML 文档的元数据。一般用于搜索引擎索引等。**只能位于 `<head>` 容器中。**
  - **页面描述：** `<meta name="description" content="Free Web tutorials on HTML, CSS, XML" />`。
  - **关键词：** `<meta name="keywords" content="HTML, CSS, XML" />`。
- **Script：** JavaScript 脚本或者外部脚本引用。

#### HTML5 语义元素

本质上是一些有特定功能的 div。

- header：页眉。
- nav：导航。
- section：节。是有主题的内容组，通常具有标题。
- article：文章。规定独立的自包含内容。文档有其自身的意义，并且可以独立于网站其他内容进行阅读。
- main：文档的主内容。
- aside：主要内容之外的内容（如侧栏）。
- footer：页脚。
- details：额外的细节。
- summary：details 元素的标题。
- time：包含日期和时间。
- figure 和 figcaption：图片（或图表等其他内容）搭配标题。
- mark：重要或者强调的文本。

### 注释 !--

 `<!-- This is a comment -->`
#### 条件注释 !--[if IE8]

针对石器时代走来的 IE。

```html
<!--[if IE 8]>
    .... some HTML here ....
<![endif]-->
```

### 文本格式化

#### 标题、段落和文字 h1/p

- **标题：** `<h1></h1>` 到 `<h6></h6>`。
- **段落：** `<p></p>`。
  - 默认情况下，HTML 会自动地在块级元素前后添加一个额外的空行，比如段落、标题元素前后。
- **分割线：** `<hr />`。
- **折行：** `<br />`。
  - 也可以使用 `<hr>` 和 `<br>`，但是更推荐加上斜杠 `/`。
- **粗体/斜体：** `<b>` 和 `<i>`。
- **大号/小号字：** `<big>` 和 `<small>`。
- **下标/上标字：** `<sub>` 和 `<sup>`。
- 着重字/加重语气： `<em>` 和 `<strong>`；插入字/删除字： `<ins>` 和 `<del>`。

#### 计算机输出 pre/code

- `<pre>` 定义预格式文本。保留所有多余的空格和折行。
- `<code>` 定义计算机代码。code 不会保留多余的空格和折行。
- `<var>` 定义变量。可以用于数学公式。
- `<kbd>` 定义键盘输入；`<samp>` 定义计算机输出；`<tt>` 定义打字机代码。

#### 引用和术语定义 blockqute

- **长引用：** `<blockquote>`。
- **短引用：** `<q>`。
- `<abbr>` 定义缩写。`<acronym>` 定义首字母缩写。
- `<address>` 定义地址。
- `<bdo>` 定义文字方向。`<bdo dir="rtl">This text will be written from right to left</bdo>`
- `<cite>` 定义引用、引证。`<dfn>` 定义一个定义项目。

### 富文本

#### 表格 table

由 `<table>` 定义。

```html
<table border="1">
	<tr>
		<td>row 1, cell 1</td>
		<td>row 1, cell 2</td>
	</tr>
	<tr>
		<td>row 2, cell 1</td>
		<td>row 2, cell 2</td>
	</tr>
</table>
```

- **属性：**
  - **边框：** `border="1"` 显示边框。默认不显示。
- **表头：** 由 `<th>`（Table ）定义。
- **行：** 每行由 `<tc>`（Table Column）定义。
  - **表格数据：** 由 `<td>`（Table Data）定义。
- **占位符：** 为了避免不显示无内容单元格的边框，可以在空单元格内放置空格占位符。`&nbsp;`。

#### 列表

##### 有序列表 ul/li

```html
<ul>
	<li>Coffee</li>
	<li>Milk</li>
</ul>
```

##### 无序列表 ol/li

```html
<ol>
	<li>Coffee</li>
	<li>Milk</li>
</ol>
```

##### 自定义列表 dl/dt

```html
<dl>
	<dt>Coffee</dt>
	<dd>Black hot drink</dd>
	<dt>Milk</dt>
	<dd>White cold drink</dd>
</dl>
```

### 多媒体

#### 链接和锚 a

 `<a href="http://www.w3school.com.cn">link</a>`。

- **href 属性：** 定义链接目标。可以是一个 URL（页面）或者目标元素的标识符（位置）。
  - **链接到 URL：**
    - **末尾斜杠：** 应该在 URL 最后加上斜杠 `/`，否则会引起重定向。
    - **邮件链接：** 将 `mailto:someone@microsoft.com?subject=Hello%20again` 作为一个 URL。
  - **链接到锚：**
    -  `<a name="tips">显示的文本</a>` 创立一个锚，`<a href="#tips">提示</a>` 创建指向其的链接。
    - 如果浏览器找不到锚，则会跳到页面顶端。
  - **链接到 URL 中的锚：** 也可以使用 URL+标识符的方式：`https://skywt.cn/index.html#tips`。
- **target 属性：** 定义被链接的文档在何处显示。
  - `target="_blank"` 使在新标签页打开文档。
  - `target="_top"` 跳出框架。
  - `target="iframe_a"` 使链接在 name 为 iframe_a 的 iframe 中打开。

#### 图像 img

 `<img src="w3school.jpg" width="104" height="142" />`

- **src 属性：** 源。
- **alt 属性：** 替换文本。
- **width 和 height 属性：** 宽度和高度。

##### figure 和 figcaption

组合图片和图片的解释。figcaption 包含图片的解释（标题）文本。HTML5 的新语义标签。

```html
<figure>
   <img src="pic_mountain.jpg" alt="The Pulpit Rock" width="304" height="228">
   <figcaption>Fig1. - The Pulpit Pock, Norway.</figcaption>
</figure> 
```

#### 内联框架 iframe

iframe 用于在网页内显示网页。

- **src、width、height 属性：** 同图像。
- **frameborder 属性：** 是否显示边框。设置为 0 则不显示边框。

### JavaScript

- `<script>` 标签中包含一段 JavaScript 代码。
- `<noscript>` 标签中包含针对不支持 JavaScript 浏览器显示的替代内容。

## 属性

此处为所有标签的一般属性。

### id

- HTML id 属性用于为 HTML 元素指定唯一的 id。
- **大小写敏感。**
- **不许重复。** 一个 HTML文档中不能存在多个有相同 id 的元素。
- **调用语法：** `#idName`。

### Name



### Class 类

设置标签的类，可以用 css 定义其不同样式。



## 样式 Style

- **颜色：** 仅有 16 种颜色名被 W3C 的 HTML 4.0 标准支持：aqua、black、blue、fuchsia、gray、green、lime、maroon、navy、olive、purple、red、silver、teal、white、yellow。其他颜色必须用十六进制颜色值表示。
- **外部样式表：** `<link rel="stylesheet" type="text/css" href="mystyle.css">`
- **内部样式表：** `<style type="text/css"></style>`
- **内联样式：** `<p style="color: red; margin-left: 20px">`

