# The Note of Go Programming Language

# Chapter 1

# 2 程序结构

## 2.1 命名

Go语言中的25的关键字

| :------- | :---------- | :----- | :-------- | :----- |
| break    | default     | func   | interface | select |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

30多个预定义的名字

| 内建常量 | true false iota nil |
| 内建类型 | int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64 uintptr float32 float64 complex128 complex64 bool byte rune string error |
| 内建函数 | make len cap new append copy close delete complex real imag panic recover |

> 如果一个名字在函数内部定义，那么它的作用域就仅限于函数内部。如果是在函数外部定义，那么将在当前包的所有文件中有效。名字的开头字母的大小写决定了名字在包外的可见性。大写表示可以在包外被访问，小写则表示不能。包本身的名字一般用小写字母。

## 2.2 声明

Go语言主要有四种声明语句：
* var - 变量
* const - 常量
* type - 类型
* func - 函数

## 2.3 变量

变量声明语法如下：
```
var 变量名字 类型 = 表达式
```

> 其中“类型”或“= 表达式”两个部分可以省略一个。如果省略的是类型，那么将根据表达式来推导出变量的类型信息。如果表达式部分被省略，那么将使用零值初始化该变量。

* 数值类型变量对应的零值是：0
* 布尔类型变量对应的零值是：false
* 字符串变量对应的零值是：""
* 接口或引用类型（包括slice，map，chan和函数）变量对应的零值是：nil
* 数组或结构体等聚合类型对应的零值是：每个元素或字段都是零值

```
var s string
fmt.Println(s)  // ""
```

```
var i, j, k int                  // int, int, int
var b, f, s = true, 2.3, "four"  // bool, float64, string
```

```
var f, err = os.Open(name)  // os.Open returns a file and an error
```

### 2.3.1 简短变量声明

在函数内部，有一种称为简短变量声明语句的形式可用于声明和初始化局部变量。

语法格式如下：
```
名字 := 表达式
```

```
i := 100      // an int
i, j := 0, 1  // int, int
```

```
f, err := os.Open(name)
```

> 简短变量声明语句中必须至少要声明一个新的变量

```
f, err := os.Open(infile)
f, err := os.Create(outfile)  // compile error: no new variables
```
