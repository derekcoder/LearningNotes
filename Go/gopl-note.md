# The Note of Go Programming Language

# 1 入门

# 2 程序结构

## 2.1 命名

Go语言中的25的关键字

```
break     default      func    interface  select 
case      defer        go      map        struct 
chan      else         goto    package    switch 
const     fallthrough  if      range      type   
continue  for          import  return     var    
```

30多个预定义的名字

```
内建常量：true false iota nil 
内建类型： int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64 uintptr float32 float64 complex128 complex64 bool byte rune string error 
内建函数： make len cap new append copy close delete complex real imag panic recover 
```

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

### 2.3.2 指针

一个变量对应一个存储该变量值的内存空间。一个指针的值是另一个变量的地址。通过指针，我们可以直接读取或更新对应变量的值。

> 不是每一个值都会有一个内存地址，但是每一个变量必然有对应的内存地址。

```
var x = 1   // int
var p = &x  // *int: 指向int类型的指针
// p指针指向变量x

fmt.Println(*p)  // "1"

*p = 2           // 等同于：x = 2
fmt.Println(x)  // "2"
```

任何类型的指针的零值都是nil。如果 p != nil，那么表示p指向某个有效变量。指针之间可以测试是否相等，只有两个指针同时指向同一个变量或全部是nil时才相等。
```
var x, y int
fmt.Println(&x == &x, &x == &y, &x == nil)  // "true false false"
```

```
// v虽然是函数局部变量，但指针p引用了v，所以v变量依然有效，不会被释放
var p = f()  

func f() *int {
    v := 1
    return &v
}
```

### 2.3.3 new函数

表达式new(T)将创建一个T类型的匿名变量，初始化为零值，然后返回变量地址，类型为*T。

```
p := new(int)  // *int, 指向匿名的int变量
fmt.Println(*p)  // "0"
*p = 2  
fmt.Println(*p)  // "2"
```

new是预定义的函数，不是关键字。所以我们可以使用new来作为变量名。
```
func delta(old, new int) int { return new - old }
```

### 2.3.4 变量的生命周期

包一级的变量的生命周期和整个程序的运行周期是一致的。局部变量生命周期则是动态的：从创建开始，到不再被引用为止。

> **编译器会自动选择在栈上还是堆上分配局部变量的存储空间，这个选择并不是由用var还是new创建变量的方式决定的。**

```
var global *int

func f() {
    var x int
    x = 1
    global = &x
}

func g() {
    y := new(int)
    *y = 1
}
```

* f函数里的x变量必须在堆上分配的，因为它在函数退出后然后可以通过一级的global变量访问，虽然它是在函数内部定义的。用Go语言的术语说，这个x局部变量从函数f中逃逸了。
* g函数中的y虽然是用new的方式创建的，但是它只在g函数内部有效。所以编译器可以选择在栈上创建，也可以选择在堆上创建。

## 2.4 赋值

语法如下：
```
变量 = 值
```

```
x = 1                        // 命名变量的赋值
*p = true                    // 通过指针间接赋值
person.name = "bob"          // 结构体字段赋值
count[x] = count[x] * scale  // 数组，slice或map的元素赋值
```

数值变量支持++递增和--递减语句（自增和自减是语言，而不是表达式，因此x = i++之类的表达式是错误的）。
```
v := 1
v++
v--
```

### 2.4.1 元组赋值

```
i, j, k = 2, 3, 5
f, err = os.Open("foo.txt")  // function call returns two values
```

交换值
```
x, y = y, x
a[i], a[j] = a[j], a[i]
```

求最大公约数(GCD: Greatest Common Divisor)
```
func gcd(x, y int) int {
    for y != 0 {
        x, y = y, x%y
    }
    return x
}
```

计算斐波那契数列（Fibonacci）的第N个数
```
func fib(n int) int {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        x, y = y, x+y
    }
    return x
}
```

