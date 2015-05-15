# Go For Java Programmers
本文旨在帮助JAVA开发者迅速掌握 Go语言.

开始用一个很容易能被所有的Java程序员认出的例子来突出特色，然后对GO的框架给出了详细的的描述，最后用一个例子来说明GO结构中没有与 Java直接对应处。

### Hello stack (一个栈的例子)
为了吊起你的胃口，我们用一个麻雀虽小，五脏俱全和符合习惯的例子对应这个Stack.java 程序

```

// 包collection实现了生成栈.
package collection

// 零值栈是一个空栈，准备被使用.
type Stack struct {
    data []interface{}
}

// Push函数将x添加到栈顶.
func (s *Stack) Push(x interface{}) {
    s.data = append(s.data, x)
}

// Pop函数是将栈顶元素移除并返回.
// 在Pop函数执行在空栈时，会被一个运行时的error警示.
func (s *Stack) Pop() interface{} {
    i := len(s.data) - 1
    res := s.data[i]
    s.data[i] = nil  // 避免内存泄露
    s.data = s.data[:i]
    return res
}

// Size函数返回栈中元素的个数
func (s *Stack) Size() int {
    return len(s.data)
}```


* 顶级声明出现之前，直接的评论是文档注释。他们是纯文字。
* 对于声明，你把名字写在类型后面.
* struct 对应Java中的类, 但struct组成不是方法而只能是变量.
* Tinterface{}类型对应Java的 Object. 在GO中它被所有的类型所实现，而不仅仅是引用类型.
* 代码段 (s *Stack) 声明了一个方法，接收者 s 对应Java中的 this.
* 操作符:=声明并初始化了一个变量.它的类型可以从初始化表达式中推导出.

这里是一个的Hello world程序，演示了如何使用collection.Stack的抽象数据类型.

```
package collection_test

import (
    collection "."
    "fmt"
)

func Example() {
    var s collection.Stack
    s.Push("world")
    s.Push("hello, ")
    for s.Size() > 0 {
        fmt.Print(s.Pop())
    }
    fmt.Println()
    //输出: hello, world
}```

## 概念上的差异

* Go的构造器没有类。Go 用 structs 和 interfaces来替代实例化方法，类的继承机制，动态方法查找.也可用于Java使用泛型接口
* Go提供所有类型的指针的值，而不只是对象和数组。对于任何类型 T，有一个相应的指针类型*T表示指针指向类型 T的值。 offers pointers to values of all types, not just objects and arrays.
* Go允许任何类型都有方法而没有装箱的限制 allows methods on any type; no boxing is required. 方法receiver,在Java中对应this可以是直接值或者是指针.
* 数组在Go就是值.当一个数组被当做函数的参数时，这个函数接收到的是数组的拷贝而不是它的指针. 然而在实践中,函数经常使用 slices作为参数; slices引用了基础数组.
* 该语言提供了字符串，一个字符串行为就像一个字节片，但是是不可改变的。
* 该语言中的哈希表被称作maps.该语言提供了独立运行的线程goroutines 和他们之间的通信渠道channels.
* 某些类型(maps, slices, 和 channels)是按引用传递，而不是值。也就是说，传递一个map到函数并而不是拷贝map，如果函数修改了map，将被调用者看到变化。在Java术语来说，可以认为这是一个map的引用.
* Go提供了两种访问级别对应Java的public和包的private.如果它的命名是大写字母开头就是最高级别public，反之就是包的private.
* 作为替换Java中的异常机制, Go采用了类型 error值来表示事件，如文件结尾,和运行时的panics来表示运行时的错误，如数组越界等.
* Go不支持隐式类型转换。混合使用不同类型的操作需要显式转换.
* Go不支持函数重载。在同一范围内的函数和方法必须具有唯一的名称.
* Go使用nil表示无效的指针，类似于Java使用null.


## 句法

### 声明
声明是跟Java是相反的。你在类型后面再写名称，类型声明从左往右更容易读

Go 与Java相对应的

| go | 与java对应的|
| -- | -- |
| var v1 int |int v1; |
| var v3 *int | Integer v2; |
| var v3 string | String v3="" |
| var v4 [10]int | int[] v4=new int[10] |
| var v5 []int | int[] v5; |
| var v6 *struct {a int} | C v6; Given: class C{int a;} |
| var v7 man[string]int | HashMap< String,Integer > v7; |
| var v8 func(a int) int | F v8 // Given : interfacr F{ int f(int a);} |

