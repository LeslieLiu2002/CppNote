# 字符串的本质
在C++中，字符串在底层实际上是字符的数组[[Array in C++]]。字符串字面值（string literals）始终存储在只读内存中。所以不要用非常量指针的形式声明字符串并修改，编译器也不会让你修改，eg：`char* name = "Cherno"`；哪怕用非常量数组的形式声明后可以修改，我们也约定俗成不要这样定义，eg：`char name[] = "Cherno"`。因为用非常量数组声明其实是创建了一个新变量，然后把字符串的内容拷贝到这个变量里再修改变量，而没有对原字符串作修改
> [!tip] 双引号括起来的文本本质上是`const char[]`数组（指针）
> 不能写出`"Cherno"+"hello"`的代码，因为`+`没有处理这个数据类型的逻辑。正确的做法是将其中一个转换为`std::string`，因为它重写了`+`的逻辑
# std::string_literals命名空间
这里有很多便捷的处理字符串的方法，使用案例如下：
```c++
using namespace std::string_literals;

// 方法一：快速变化数据类型
const char* str1="Cherno"s+"Hello";//在第一个字符串后加上s，将它转变为std::string
// 方法二：快速换行
const char* str2=R"(line1
line2
line3)";//用R"()"快速换行
// 方法二等价于下面的写法
const char* str3="line1\n"
"line2\n"
"line3";//注意不要在中间加上逗号分号等符号

```
