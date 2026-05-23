# 实现
-  栈(Stack)分配：例如`int example[5]`，这种数组在超出其所在代码块的作用域后会自动销毁。在编译器眼里`example`的类型是`int[5]`，栈指针包含偏移量
- 堆(Heap)分配：使用`new`关键字，例如`int* example = new int[5];`。这种数组的生命周期直到你手动调用`delete[] example;`才会结束。在编译器眼里`example`的类型是`int*`
# 查漏补缺
1. 性能问题：尽量在栈上分配数组，在堆上分配时会导致“内存重定向”，cpu需要在内存中跳转寻址[[Stack vs Heap Memory in C++]]
2. 不要用`sizeof`去计算**指针类型**的数组对大小：当在堆上分配数组，或者把数组传入某个函数时，数组会“退化”为指针。此时使用`sizeof`之后得到指针的大小（通常是4或者8字节）。但栈上的数组可以使用`sizeof`计算数组大小：`sizeof(example)/sizeof(int);`。但==最好的方法是自己维护数组大小==
3. 释放堆内存的正确语法：销毁堆上的数组时，必须带上方括号，使用`delete[]`
# std中的array
[[Static Array in C++(std中的array)]]
# 多维度的array
[[Multidimensional Arrays in C++(2D array)]]