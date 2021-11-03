# HTML5 学习笔记

[toc]

## 基本概念

### 关于

- **全称：** 超文本标记语言（Hyper Text Markup Language）。
- **标记语言。** HTML 不是一种编程语言，而是一种标记语言（markup language）。
- **编码。** HTML5 中默认的字符编码是 UTF-8。
- **空格空行忽略。** 当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格、空行都会被算作一个空格。
- **大小写不敏感。** 但是推荐使用小写。

### 概念

- **文档：** 就是页面。

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



## 标签

### 块布局



### 注释

 `<!-- This is a comment -->`
#### 条件注释

针对石器时代走来的 IE。

```html
<!--[if IE 8]>
    .... some HTML here ....
<![endif]-->
```

### 文本格式化

#### 标题、段落和文字

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

#### 计算机输出

- `<pre>` 定义预格式文本。`<code>` 定义计算机代码。
- `<kbd>` 定义键盘码；`<samp>` 定义计算机代码样本；`<tt>` 定义打字机代码；`<var>` 定义变量。

#### 引用和术语定义

- **长引用：** `<blockquote>`。
- **短引用：** `<q>`。
- `<abbr>` 定义缩写。`<acronym>` 定义首字母缩写。
- `<address>` 定义地址。
- `<bdo>` 定义文字方向。`<bdo dir="rtl">This text will be written from right to left</bdo>`
- `<cite>` 定义引用、引证。`<dfn>` 定义一个定义项目。

### 富文本

#### 表格

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

##### 有序列表

```html
<ul>
	<li>Coffee</li>
	<li>Milk</li>
</ul>
```

##### 无序列表

```html
<ol>
	<li>Coffee</li>
	<li>Milk</li>
</ol>
```

##### 自定义列表

```html
<dl>
	<dt>Coffee</dt>
	<dd>Black hot drink</dd>
	<dt>Milk</dt>
	<dd>White cold drink</dd>
</dl>
```

### 多媒体

#### 链接

 `<a href="http://www.w3school.com.cn">link</a>`。

- **Href 属性：** 定义链接目标。
  - **末尾斜杠：** 应该在 URL 最后加上斜杠 `/`，否则会引起重定向。
  - **邮件链接：** 将 `mailto:someone@microsoft.com?subject=Hello%20again` 作为一个 URL。
- **Target 属性：** 定义被链接的文档在何处显示。
  - `target="_blank"` 使在新标签页打开文档。
  - `target="_top"` 跳出框架。
- **Name 属性：** `<a name="tips">显示的文本</a>` 创立一个锚，`<a href="#tips">提示</a>` 创建指向其的链接。
  - 如果浏览器找不到锚，则会跳到页面顶端。
- **Id 属性：** 和 name 作用一样。

#### 图像

 `<img src="w3school.jpg" width="104" height="142" />`

- **Src 属性：** 源。
- **Alt 属性：** 替换文本。
- **Width 和 Height 属性：** 宽度和高度。



## 属性



## 样式

- **颜色：** 仅有 16 种颜色名被 W3C 的 HTML 4.0 标准支持：aqua、black、blue、fuchsia、gray、green、lime、maroon、navy、olive、purple、red、silver、teal、white、yellow。其他颜色必须用十六进制颜色值表示。
- **外部样式表：** `<link rel="stylesheet" type="text/css" href="mystyle.css">`
- **内部样式表：** `<style type="text/css"></style>`
- **内联样式：** `<p style="color: red; margin-left: 20px">`



