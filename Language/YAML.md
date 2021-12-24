# YAML 学习笔记

- 读音：/ˈjæməl/
- 全程：YAML Ain't a Markup Language 或者 Yet Another Markup Language。

## 基本语法

- 缩进：
  - 只能用空格，不能用 Tab。
  - 空格数不重要，只要相同层级的元素左对齐即可。
- 大小写敏感。
- 注释：`#` 开头为注释行。

## 数据类型

- 对象：键值对的集合，又称为映射（mapping）、哈希（hashes）、字典（dictionary）。
- 数组：一组按次序排列的值，又称为序列（sequence）、列表（list）。
- 纯量（scalars）：单个的、不可再分的值。

## 对象

- 对象：键值对的集合，又称为映射（mapping）、哈希（hashes）、字典（dictionary）。

- 键值对用冒号结构表示：

  - `key: value`

  - `key: {child1: value1, child2: value2}`

  - ```yaml
    key:
        child1: value1
        child2: value2
    ```

- `?` 加空格开始一个复杂的 key（如数组），`:` 加空格开始一个复杂的 value：

  ```yaml
  ? 
      - key1
      - key2
  : 
      - value1
      - value2
  ```

  表示 key 为数组 `[key1, key2]`，值为数组 `[value1, value2]`。

## 数组

- 单行表示：`[value1, value2]`

- 多行表示：

  ```yaml
  - value1
  - value2
  ```

## 纯量

- 字符串、布尔值、整数、浮点数、Null、时间、日期。

```yaml
boolean: 
    - TRUE  # true, True 都可以
    - FALSE  # false，False 都可以
float:
    - 3.14
    - 6.8523015e+5  # 可以使用科学计数法
int:
    - 123
    - 0b1010_0111_0100_1010_1110    # 二进制表示
null:
    nodeName: 'node'
    parent: ~  # 使用 ~ 表示 null
string:
    - 哈哈
    - 'Hello world\n'  # 可以使用双引号或者单引号包裹特殊字符（如空格），单引号会自动转义字符
    - "Hello world\n"  # 双引号不会转义字符
    - 'labor''s day'  # 单引号中用两个单引号表示单引号 
    - newline
      newline2    # 字符串可以拆成多行，每个换行符会被转化成一个空格
    - | # 用 | 符号保留换行符
      line1
      line2
date:
    - 2018-02-17    # 日期必须使用 ISO 8601 格式，即 yyyy-MM-dd
datetime: 
    -  2018-02-17T15:02:31+08:00    # 时间使用 ISO 8601 格式，时间和日期之间使用 T 连接，最后使用 + 代表时区
```

## 引用

- 用 `&` 建立锚点，`<<` 表示合并到当前数据，用 `*` 引用。

```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
  
# 等价于：

defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

