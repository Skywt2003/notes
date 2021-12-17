# CSS 学习笔记

[toc]

## 基本概念

- **全称：** 层叠样式表（Cascading Style Sheets）

## 概念和语法

### 语法

- **注释：** 和 C++ 类似，包裹在 `/* */` 内。
- **规则集 rule-set：** 由选择器和声明块组成。
- **选择器：** 指向需要设置样式的 HTML 元素。
- **声明块：** 一个大括号中包含一条或多条用分号分隔的声明。
- **声明：** 一个属性和一个值，用冒号分隔。

### 单位

- `px`：像素。设置绝对大小。
- `em`：1em 等于当前字体大小，可以由用户调整。推荐使用的单位。
- `vw`：1vw 等于视口宽度（Viewport Width，即浏览器窗口大小）的 1%。

### 颜色

- **RGB 颜色：**

  - 使用 `rgb(red, green, blue)` 指定颜色，每个参数定义了 0 到 255 之间的颜色强度。黑色为 0,0,0，白色为 255,255,255。

  - 使用 `rgba(red, green, blue, alpha)` 指定颜色，alpha 参数代表不透明度，介于 0 至 1.0 之间。

- **HEX 颜色：** 使用十六进制代码 `#rrggbb` 指定颜色，每两位表示红、绿、蓝的强度。

- **HSL 颜色：** 使用色相、饱和度和明度（HSL）来指定颜色：`hsl(hue, saturation, lightness)`。同样有 HSLA。

  - **色相（hue）** 是色轮上从 0 到 360 的度数。0 是红色，120 是绿色，240 是蓝色。


  - **饱和度（saturation）** 是一个百分比值，可以看成颜色强度。0％ 是无颜色的灰色阴影，而 100％ 是全颜色。


  - **亮度（lightness）** 也是百分比，0％ 是黑色，50％ 是既不明也不暗，100％是白色。

### 字体

在 CSS 中，有五个通用字体族，所有不同的字体名称都属于这五个通用字体系列之一。

- **衬线字体（Serif）**
- **无衬线字体（Sans-serif）**
- **等宽字体（Monospace）**
- **草书字体（Cursive）：** 模仿手写体。
- **幻想字体（Fantasy）：** 装饰性/俏皮的字体。

## 布局

### 框模型（盒模型）

从内向外：

- **内容：** 实际内容。大小由 `height/width` 控制。
- **内边距：** 清除内容周围的区域。内边距是透明的。
- **边框：** 围绕内边距和内容的边框。边框以内内容才有背景。
- **外边距：** 清除边界外的区域。外边距是透明的。

## 选择器

- **简单选择器：** 根据名称、id、类来选取元素。`#id` 选择 id，`.class` 选择类，`p` 选择元素。通用选择器 `*` 选择所有元素。
  - 选择多个/种元素用逗号 `,` 隔开。
- **组合器选择器：** 通过组合器选择元素
  - **后代选择器：** 空格 ` `，匹配后代元素（包括子元素）
  - **子选择器：** 大于号 `>`，匹配子元素
  - **相邻兄弟选择器：** 加号 `+`，匹配同级（同父元素）紧随其后的元素
  - **通用兄弟选择器：** 波浪线 `~`，匹配同级（同父元素）所有元素
- **伪类选择器：** 根据特定状态选取元素
- **伪元素选择器：** 选取元素的一部分并设置其样式
- **属性选择器：** 根据属性或属性值来选取元素

### 优先级

页面中的所有样式将按照以下规则“层叠”为新的“虚拟”样式表，其中第一优先级最高：

1. 行内样式（在 HTML 元素中）
2. 外部和内部样式表（在 head 部分）
3. 浏览器默认样式

## 属性

### 背景 background

- `background-color`。
- `background-image`。值一般形如 `url("img.png")`。
- `background-repeat`，背景图像重复方式。可以为 `repeat-x/repeat-y/no-repeat`。默认水平和垂直都重复。
- `background-attachment`，背景图像附着。指定背景图像滚动还是固定。可以是 `scroll/fixed`。
- `background-position`，背景图像位置。可以为形如 `right top`。
- **简写属性：** `background`，依次填写以上五个属性。

### 边框 border

- **混合/简写：** 边框各种属性接受一到四个值
  - 填写一个值：四边。
  - 填写两个值：为上下、左右。
  - 填写三个值：上、左右、下。
  - 填写四个值：依次为上、右、下、左。
  - 可以用 `border-top/right/bottom/left(-xxx)` 设定单独边框属性的值。
- `border-style`，边框类型。
  - `solid/double/dotted/dashed ` 实线/双实线/点线/虚线
  - `groove/ridge/inset/outset` - 3D 坡口/脊线/inset/outset 边框。效果取决于 border-color 值
  - `none/hidden` 无边框/隐藏边框
  - **必需：** 如果不设置  `border-style` 属性，其他边框相关的属性都不会生效。
- `border-width`，可以设置为预定值 `thin/medium/thick` 或以 `px/pt/cm/em` 为单位表示数值。
- `border-color`。
- **简写属性：** 依次为  `border-width`，`border-style`（必需），`border-color`。
- `border-radius`，边框圆角。

### 外边距 margin

- **混合/简写：** 和 border 机制相同。
- **允许的值：**
  - `auto`：自动计算。
  - 数值：以 `px/pt/cm/em` 为单位表示数值。
  - `%`：指定以包含元素宽度的百分比计的外边距。
  - `inherit`：继承自父元素。
  - **允许负值。**
- **外边距合并：** 当两个垂直外边距相遇时，它们将形成一个外边距。
  - 只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并。

### 内边距 padding

- **混合/简写：** 和 border 机制相同。
- **允许的值：**
  - length：以 `px/pt/cm/em` 为单位表示数值。
  - `%`：指定以包含元素宽度的百分比计的内边距。
  - `inherit`：继承自父元素。
  - **不允许负值。没有 `auto`。**
- `box-sizing`：如果设置为 `border-box`，则会将其宽度固定为 `width`，使内容区域减小来满足 `padding` 的设置。

### 高度 height 宽度 width

- 设置的是元素内边距、边框以及外边距内的区域的高度或宽度。
- **混合/简写：** 和 border 机制相同。
- **允许的值：**
  - `auto`：自动计算。
  - length：以 `px/pt/cm/em` 为单位表示数值。
  - `%`：以包含块的百分比定义高度/宽度。
  - `initial`：默认值。
  - `inherit`：继承自父元素。
- `max-width`，最大宽度。可以是 length、`%` 或者 `none`。
  - 如果浏览器窗口小于其值，会出现滚动条。
  - 其值会覆盖 `width`。

### 轮廓 outline

- 轮廓在边框之外，不计元素总宽度，可以与其他内容重叠。
- **混合/简写：** 和 border 机制相同。
- `outline-style`，轮廓类型。和边框基本相同。
- `outline-width`。
- `outline-color`。除了颜色以外，可以为 `invert`（颜色反转）。
- **简写属性：** 和边框基本相同。
- `outline-offset`，轮廓偏移。指示边框与轮廓的距离，中间的间距是透明的。

### 文本属性 text/font-

#### 段落样式 text-

- `color`：文字颜色。
- `text-align`：水平对齐方式。可以为 `center/left/right/justify`。`justify` 指的是拉伸，使每行宽度相同。
- `vertical-align`：文本中图像的垂直对齐方式。可以为 `top/middle/bottom`。
- `text-decoration`：文字装饰，可以为 `overline/line-through/underline/none`。
- `text-transform`：指定文字大小写转换：`uppercase/lowercase/capitalize`。`capitalize` 指的是让所有单词第一个字母转为大写。
- `text-indent`：一段文本第一行的锁进。
- `letter-spacing`：字符之间的间距。
- `word-spacing`：单词之间的间距。
- `line-height`：行高。
- `white-space`：指定内容中空白字符处理方式，如换行。设置为 `nowrap` 禁用换行。
- `text-shadow`：文本阴影。
  - **简写属性：** 水平阴影、垂直阴影、颜色。

#### 字体样式 font-

- `font-family`：规定字体。
  - 按照顺序检索设定的字体。为了兼容性应该多写点。
  - 如果字体名称不止一个单词，则必须用引号引起来，例如："Times New Roman"。
- `font-style`：字体样式。`normal/italic/oblique` 表示正常/斜体（意大利体）/倾斜体。
- `font-weight`：字重。字体的粗细。
- `font-variant`：`normal/small-caps`，可以小型大写字母显示文本。
- `font-size`：文本大小。
  - 默认大小为 16px（1em）。
  - 可以使用绝对和相对大小。如果为绝对大小，则用户无法调整。
- **简写属性：** `font` 属性的值可以设为 `font-style`、`font-variant`、`font-weight`、`font-size/line-height`（必需）、`font-family`（必需）。由于每个属性的值都不同，故无需考虑顺序。

### 列表属性 list-

- **默认：** 默认存在内边距、外边距和默认的标记。
- `list-style-type` 指定列表项标记的类型。可以是 `circle/square/upper-roman/lower-alpha/none`，圆圈/正方形（无需列表）/罗马数字/小写字母（有序列表）/不显示。
- `list-style-position`：指示标记的位置，`outside/inside` 表示在内容外/内。
- `list-style-image` 指定图像作为列表项标记，如 `url('sqpurple.gif')`。
  - 如果无法显示，则会显示 `list-style-type` 设置的值。
- **简写属性：** `list-style`，以上述顺序设置三个属性。

### 表格属性

- `border` 表格边框。
  - 默认情况下，如果 table、th、tb 都有自己的边框，表格会显示为双边框。
- `border-collapse` 设为 `collapse` 则将表格双边框折叠为单一边框。





## Flex

### 基本概念

- 轴 axis：存在主轴（main axis）和交叉轴（cross axis）。
  - 默认主轴水平，交叉轴垂直。可以更改。
- 单元块 flex item：主轴方向大小（main size）和交叉轴方向大小（cross size）。

### 容器 container 属性

- 设为 flex 容器：`display: flex | inline-flex;`

#### flex-direction 主轴方向

- `row|row-reverse|column|column-reverse` 分别是水平左起、水平右起、垂直上起、垂直下起。

#### flex-wrap 折行方式

- ` nowrap|wrap|wrap-reverse` 分别是不折行（所有内容压在一行）、折行、反向折行。

#### flex-flow 简写上两者

#### justify-content 主轴上对齐方式

- `flex-start|flex-end|center|space-between|space-around` 分别是左对齐、右对齐、居中、两端对齐、均匀分布。

#### align-items 交叉轴上对齐方式

- `flex-start|flex-end|center|baseline|stretch` 分别是交叉轴起点对齐、终点对齐、中点对齐、首行文字基线对齐、填满。
- 默认是 stretch。

#### align-content 多轴线的对齐方式

- 用于设置 wrap 之后产生的多根轴线的对齐方式。

- `flex-start|flex-end|center|space-between|space-around|stretch` 和 align-items 类似。

### 项目 item 属性

#### order 序号

- 设置序号。根据序号排列各项目。

#### flex-basis 占据主轴空间

- 设置数值。默认为 auto。
- 设置该属性，项目的 width 或者 height（主轴方向的大小设置）会失效。

#### flex-grow 放大因子

- 设置数值，根据每根主轴所有该设置的比例调整大小布局。
- 按照 flex-basis 排列仍然有剩余空间时才生效。

#### flex-shrink 收缩因子

- 设置数值，类似 flex-grow，但是无需剩余空间。

#### flex 上三者简写

- 依次是 flex-grow、flex-shrink、flex-basis。
- 默认 auto：`1 1 auto`
- none：`0 0 none`

#### align-self 单项目特殊对齐方式

- 内容和 align-items 一样。
