# CSS 学习笔记

[toc]

## 基本概念

- **全称：** 层叠样式表（Cascading Style Sheets）

## 语法

- **注释：** 和 C++ 类似，包裹在 `/* */` 内。

### 一些概念

- **规则集 rule-set：** 由选择器和声明块组成。
- **选择器：** 指向需要设置样式的 HTML 元素。
- **声明块：** 一个大括号中包含一条或多条用分号分隔的声明。
- **声明：** 一个属性和一个值，用冒号分隔。

## 颜色

### RGB 颜色

使用 `rgb(red, green, blue)` 指定颜色，每个参数定义了 0 到 255 之间的颜色强度。黑色为 0,0,0，白色为 255,255,255。

使用 `rgba(red, green, blue, alpha)` 指定颜色，alpha 参数代表不透明度，介于 0 至 1.0 之间。

### HEX 颜色

使用十六进制代码 `#rrggbb` 指定颜色，每两位表示红、绿、蓝的强度。

### HSL 颜色

使用色相、饱和度和明度（HSL）来指定颜色：`hsl(hue, saturation, lightness)`。

同样有 HSLA：`hsla(hue, saturation, lightness, alpha)`。

- **色相（hue）**是色轮上从 0 到 360 的度数。0 是红色，120 是绿色，240 是蓝色。
- **饱和度（saturation）**是一个百分比值，可以看成颜色强度。0％ 是无颜色的灰色阴影，而 100％ 是全颜色。
- **亮度（lightness）**也是百分比，0％ 是黑色，50％ 是既不明也不暗，100％是白色。

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

- **背景颜色：** `background-color`。
- **背景图像：** `background-image`。值一般形如 `url("img.png")`
- **背景图像重复方式：** `background-repeat`。可以为 `repeat-x/repeat-y/no-repeat` 默认水平和垂直都重复。
- **背景图像附着：**`background-attachment`。指定背景图像滚动还是固定。可以是 `scroll/fixed`
- **背景图像位置：**`background-position`。可以为形如 `right top`。
- **简写属性：** `background`，依次填写以上五个属性。

### 边框 border
