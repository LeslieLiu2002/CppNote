# Class vs Struct
>[!tip] 本质区别
>不加`private`或者`public`时，Class默认是将成员设置为`private`；Struct默认是将成员设置为`public`

也可以把Class和Struct理解成有private和public的命名空间
# 类的声明和实现
下面演示带上命名空间的情况。当然可以直接去掉命名空间，因为Class本身就是个命名空间。
在`MyClass.h`文件中声明类
```c++
#include <string>  // 注意点1
namespace MyNamespace {
    class MyClass {
    public:
        std::string doSomething();
    };
}
```
在`MyClass.cpp`文件中实现类
```c++
#include "MyClass.h"
namespace MyNamespace { // 注意点2
    void MyClass::doSomething() {
        std::cout << "Doing something." << std::endl;
    }
}
// 这样也行,直接用全名定义
std::string Mynamespace::MyClass::doSomething()
{
	return "Doing something.";
}
```
- **注意点1**：一定不要把`#include`语句放在自定义的命名空间里，不然就相当于给原有命名空间外套了一层新命名空间，会导致无法找到功能的实现（因为实现没有被包裹在这个命名空间里）。
- **注意点2**：和注意点1相呼应地，头文件和源文件里关于该类的内容都要放在命名空间里，不然编译器无法将它们对应起来。
# Static in Class and Struct
[[Static in C++#类内Static]]
# Constructor
[[Constructor in C++]]
语法：`对象名(){}`
# Destructor
[[Destructor in C++]]
语法：`～对象名(){}`
# Inheritance
继承允许我们创建一个类层级结构。定义一个基类存放通用的、共享的功能；创建从基类派生出的子类，它会获得==基类所有特性和行为==，并可在此基础上添加新功能或修改已有功能
[[Inheritance in C++]]
# 类中的const
[[Const in C++#修饰类方法]]
# 创建/实例化对象
- Stack: `Entity e("Cherno");`
- Heap: `Entity* e = new Entity();`。绝对不推荐用malloc创建对象，因为它负责分配内存但不调用对象的构造函数，且返回的`void*`无类型指针。也可以指定内存地址给new，然后在这个内存上创建对象`new(有类型指针) 对象名()`。
- 用`{}`代替`()`也可以
# 类的隐式转换
[[Implicit Conversion and the Explicit Keyword in C++]]
回答为什么`ScopedPtr e = new Entity();`可以编译？其中ScopedPtr和Entity不是同一个类。