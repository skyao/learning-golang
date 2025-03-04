---
title: "函数调用"
linkTitle: "函数调用"
weight: 20
date: 2025-02-27
description: >
  Golang 中的函数调用
---

### 理解Go语言中的函数调用

https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-function-call/

#### 调用惯例

调用惯例是调用方和被调用方对于参数和返回值传递的约定。

- c

	 当我们在 x86_64 的机器上使用 C 语言中调用函数时，参数都是通过寄存器和栈传递的，其中：

	- 六个以及六个以下的参数会按照顺序分别使用 edi、esi、edx、ecx、r8d 和 r9d 六个寄存器传递；
	- 六个以上的参数会使用栈传递，函数的参数会以从右到左的顺序依次存入栈中；

	而函数的返回值是通过 eax 寄存器进行传递的，由于只使用一个寄存器存储返回值，所以 C 语言的函数不能同时返回多个值。

- Go 语言使用栈传递参数和接收返回值，所以它只需要在栈上多分配一些内存就可以返回多个值。

思考:

C 语言和 Go 语言在设计函数的调用惯例时选择也不同的实现。C 语言同时使用寄存器和栈传递参数，使用 eax 寄存器传递返回值；而 Go 语言使用栈传递参数和返回值。我们可以对比一下这两种设计的优点和缺点：

- C 语言的方式能够极大地减少函数调用的额外开销，但是也增加了实现的复杂度；
	- CPU 访问栈的开销比访问寄存器高几十倍[3](https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-function-call/#fn:3)；
	- 需要单独处理函数参数过多的情况；
- Go 语言的方式能够降低实现的复杂度并支持多返回值，但是牺牲了函数调用的性能；
	- 不需要考虑超过寄存器数量的参数应该如何传递；
	- 不需要考虑不同架构上的寄存器差异；
	- 函数入参和出参的内存空间需要在栈上进行分配；

Go 语言使用栈作为参数和返回值传递的方法是综合考虑后的设计，选择这种设计意味着编译器会更加简单、更容易维护。

### 参数传递

除了函数的调用惯例之外，Go 语言在传递参数时是传值还是传引用也是一个有趣的问题，这个问题影响的是当我们在函数中对入参进行修改时会不会影响调用方看到的数据。我们先来介绍一下传值和传引用两者的区别：

- 传值：函数调用时会对参数进行拷贝，被调用方和调用方两者持有不相关的两份数据；
- 传引用：函数调用时会传递参数的指针，被调用方和调用方两者持有相同的数据，任意一方做出的修改都会影响另一方。

不同语言会选择不同的方式传递参数，Go 语言选择了传值的方式，**无论是传递基本类型、结构体还是指针，都会对传递的参数进行拷贝**。

>  备注：Go的方式和Java是类似的。Java也是传值，如果是对象也传递对象句柄的值）。

总结：

- Go 语言中对于整型和数组类型的参数都是值传递的
	- 如果数组很大，传值方式（拷贝）会对性能造成比较大的影响
- 传递结构体时：会对结构体中的全部内容进行拷贝；
- 传递结构体指针时：会对结构体指针进行拷贝；

Go 语言在传递参数时其实使用的就是传值的方式，接收方收到参数时会对这些参数进行复制；

### 摘要：函数调用的过程

> 摘录自 [函数——go世界中的一等公民](https://segmentfault.com/a/1190000023340324) “函数调用的过程” 一节

在go语言中，每一个goroutine持有一个连续栈，栈基础大小为2kb，当栈大小超过预分配大小后，会触发栈扩容，也就是分配一个大小为当前栈2倍的新栈，并且将原来的栈拷贝到新的栈上。使用连续栈而不是分段栈的目的是，利用局部性优势提升执行速度，原理是CPU读取地址时会将相邻的内存读取到访问速度比内存快的多级cache中，地址连续性越好，L1、L2、L3 cache命中率越高，速度也就越快。

在go中，和其他一些语言有所不同，函数的返回值、参数都是由被caller保存。每次函数调用时，会在caller的栈中压入函数返回值列表、参数列表、函数返回时的PC地址，然后更改bp和pc为新函数，执行新函数，执行完之后将变量存到caller的栈空间中，利用栈空间中保存的返回地址和caller的栈基地址，恢复pc和sp回到caller的执行过程。 

对于栈变量的访问是通过bp+offset的方式来访问，而对于在堆上分配的变量来说，就是通过地址来访问。在go中，变量被分配到堆上还是被分配到栈上是由编译器在编译时根据逃逸分析决定的，不可以更改，只能利用规则尽量让变量被分配到栈上，因为局部性优势，栈空间的内存访问速度快于堆空间访问。