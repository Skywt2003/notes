# Go 语言 学习笔记

[toc]

## 基本概念

- **文件拓展名：**`.go`。
- **代码编码：**原生支持 Unicode。

### Go 命令用法

- **程序运行（类似解释）：** `go run hello.go`。
- **生成二进制文件（类似编译）：** `go build hello.go`。
- **获取代码：** `go get gopl.io/ch1/helloworld`。下载的代码会放在 `$GOPATH/src/gopl.io/ch1/helloworld` 目录。

## 语句格式

- `{` 不能单独放在一行，否则会报错。大括号不换行党狂喜。
- 关于空格没有严格的要求。

### 语言结构

以 `hello.go` 为例：

```go
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```

包含以下部分：

- 包声明：声明属于哪个包，`package main` 表示为一个独立的程序。
- 引入包：`import "fmt"` 表示引入 `fmt` 包。`fmt` 包实现了格式化输入输出的函数。
- 函数：`func main()` 定义函数 `main()`，是每个程序必须包含的主函数。但如果存在 `init()` 则会先执行之。
- 变量：当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如 Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）。
- 语句&表达式：`fmt.Println("Hello, World!")` 输出，会在行尾自动追加换行。也可用 `fmt.Print("...")`。
- 注释：单行 `//` 开头，多行在 `/* */` 中（不可嵌套使用），与大多数语言类似。

### 标记

Go 程序由多个标记组成，可以是关键字、标识符、常量、字符串、符号。如以下 Go 语句由 6 个标记组成：

```go
fmt.Println("Hello, World!")
```

分别是 `fmt`、`.`、`PrintLn`、`(`、`Hello, World`、`)`。

- **行分隔符：**Go 中无需在行尾添加 `;` 标志语句结束，其以换行符为语句分隔符。如果要在一行写多个语句可以用行内 `;` 隔开。
- **注释：**单行 `//` 开头，多行在 `/* */` 中（不可嵌套使用），与大多数语言类似。
- **标识符：**标识符用来命名变量、类型等程序实体。规则大致与一般编程语言类似。

## 运算符

- 和 C++ 一模一样。

## 变量与常量

### 数据类型

- **布尔型**
- **数字类型：**
  - 整数类型：`uint32`、`uint64`、`int32`、`int64` 等。
  - 浮点数类型：`float32`、`float64`、`complex64`、`complex128`。
  - 其他数字类型：`uint` 和 `int`（基于架构的整数类型）；`uintptr`（无符号整型，用于存放一个指针）、`rune`、`byte`。
- **字符串类型**
- **派生类型：**指针类型（Pointer）、数组类型、结构化类型（struct）、Channel 类型、函数类型、切片类型、接口类型（interface）、Map 类型

### 变量

- **变量的声明**
  - 不初始化并指定类型：`var name type`，`type` 为类型。也可以一次声明多个变量：`var name1, name2 type`。
    - 变量的值默认为零值。零值就是系统设置的变量默认值。
    - 这些类型的零值为 `nil`：`*int`，`[]int`，`map[string] int`，`chan int`，`func(string) int`，`error`。
  - 创建时初始化并自动判断类型：`var name = value`。
  - 赋值操作符 `name := value`：相当于 `var name = value`。
    - 只能用于函数体内，不能用于全局变量。
    - 不能对于同一个变量多次声明。
- **变量的使用**
  - **必须使用。** 局部变量声明后必须使用，否则会编译错误。但是全局变量可以不使用。
  - **作用域：** 和 C++ 相同。
  - **并行赋值：** 多变量可以用一个语句赋值：`a, b, s = 1, 2 ,'hello'`。也可以并行用 `:=` 创建变量。
    - 可以用 `a, b = b, a` 交换两个变量。
    - 也可以用于一个函数返回多个返回值的情况：`a, b, c = myFunc()`。

- **空白标识符：** `_` 是一个只写变量，可用于承担函数给出的不需要的返回值。

### 常量

- 常量中的数据类型只可以是布尔型、数字型、字符串型。

- **常量的定义：**

  - **显示定义：** `const name [type] = value`

  - **隐式定义：** `const name = value`，自动判断类型

  - **批量定义：**

    ```go
    const (
        a = "abc"
        b = 1
        c = iota
        d
    )
    ```

    - **`iota` 常量：** iota 在 const 关键字出现时将被重置为 0（const 内部的第一行之前），const 中每新增一行常量声明将使 iota 计数一次（iota 可理解为 const 语句块中的行索引）。一个 `iota` 之后紧接的省略值的行都会一直以上一行 +1 为值。

### 数据结构

#### 数组

- **声明：** `var variable_name [SIZE] variable_type`
- 初始化：

#### 切片 slice



#### 范围  range



#### 结构体



#### 指针



#### map



## 流程控制

### 条件语句

- **if 语句：** 和 C++ 唯一的区别在于条件不用括号。

  ```go
  if a < b {
      /* do something */
  } else {
      /* do something */
  }
  ```

- **switch 语句：** 从上到下寻找匹配项，匹配后自动 break。

  ```go
  switch var1 {
      case val1:
          ...
      case val2, val3: /* 多个条件符合其一即可 */
      case type: /* 条件为 var1 是 type 类型 */
      case var4:
      	...
      	fallthrough /* 强制执行下一个 case 分支 */
  	case var5:
      default: /* 可选的默认分支 */
  }
  ```

- **select 语句： ** 用于通信的 switch 语句。（？）

  - 每个 case 必须是一个通信操作，要么是发送要么是接收。
  - select 随机执行一个可运行的 case。如果没有 case 可运行，将执行 default。如果 default 也没有，它将阻塞。

  ```go
  select {
      case communication clause  :
         statement(s);      
      case communication clause  :
         statement(s); 
      /* 你可以定义任意数量的 case */
      default : /* 可选 */
         statement(s);
  }
  ```

- 没有 `?:` 三目运算符。

### 循环

- **for 循环：**

  - **定长形式：** `for init; condition; post { }`

    - 和 C++ 唯一的区别在于少了个括号。

  - **条件形式：** `for condition { }`

    - 死循环：`for { }`。

  - **范围形式：** range 格式可以对 slice、map、数组、字符串等进行迭代循环：

    ```go
    for key, value := range str {
        str[key] = value
    }
    ```

- **循环控制：** `braek`，`continue` 和 `goto`。

  - 要加分号。
  - `goto`：无条件转移到循环中指定的行。用 `loopName: statement` 标记一个语句，`goto labelName;` 跳转。

## 函数

- 函数的定义：`func funcName(a, b int, c float) int { }`
  - 返回多个值： `func funcName(a, b int, c float) (int,int) { }`



**fmt.Sprintf** 格式化字符串。

