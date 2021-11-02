# JavaScript 学习笔记

[toc]

## 语句和语法

### 变量

- 字面量：
  - 数组（Array）字面量定义一个数组：`[40, 100, 1, 5, 25, 10]`
  - 对象（Object）字面量定义一个对象：`{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}`
  - 函数（Function）字面量定义一个函数：`function myFunction(a, b) { return a * b;}`
- 变量：`var x=5`，默认初值实际是 `undefined`。重新声明不会使值消失。
  - 函数中声明的都是局部变量
  - 局部变量优先原则
  - 给未声明的变量直接赋值，会将其视为 `window` 的一个属性（可配置全局属性），可以用 `delete vName` 删除
  - 对象和函数也是变量
- 数据类型：字符串 `String`、数字 `Number`、布尔 `Boolean`、对空 `Null`、未定义 `Undefined`、Symbol。
  - 动态类型：同一个变量可以在不同时刻给不同类型。可以用 `new` 声明类型：`var x=new Number`
  - `typeof` 操作符：返回变量的数据类型。`typeof x`
  - 字符串：[String 对象方法参考](https://www.runoob.com/jsref/jsref-obj-string.html)
    - 可以使用单引号和双引号
    - 反斜杠转义字符和 C++ 类似
    - 可作为字符数组调用，索引从 0 开始
    - `length` 属性返回长度
  - 数字：带小数点和不带小数点的数字同属数字
  - 数组：`var cars=new Array("Saab","Volvo","BMW");` 通过 `cars[0]` 调用 `Saab`。
    - 数组也是一种对象。
  - 对象：寻址方式：`name=person.lastname;` 或者 `name=person["lastname"];`
    - 对象中的方法：在对象内 `methodName : function() {  }` 创建一个对象内的方法，`objectName.methodName()` 访问
  - Null 与 Undefined：值相等但类型不等，Null 属于一种对象。

### 语句

- 使用 Unicode 字符集
- 行尾分号分隔
- `//` 和 `/* */` 注释
- 函数：`function myFunction(a,b){  }`
  - 无需声明参数类型和返回值
  - 可以用 `return` 退出或者返回值
- 运算符：
  - 也有自增和自减，也有类似 C++ 的三目运算符
  - `+` 可以用于两个字符串；如果将数字和字符串相加，数字被强转为字符串
  - 比较运算符：
    - `==`：值相等
    - `===`：绝对相等，值和类型相等（`!==` 不绝对相等）
- 条件、分支和循环
  - `if (con) {} else {}`，`if (con) {} else if {}`
  - for/while/do-while 循环和 C++ 一模一样。
    - for/in 循环：`for (x in person){}`，`person[x]` 为对象中的第 x 个值
  - 标签引用：写代码块 `list:{ }`，在其中 `break list;` 可以跳出代码块

## HTML DOM 事件

[HTML DOM 事件](https://www.runoob.com/jsref/dom-obj-event.html)

