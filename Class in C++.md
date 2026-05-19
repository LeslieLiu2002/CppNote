# Class vs Struct
>[!tip] 本质区别
>不加`private`或者`public`时，Class默认是将成员设置为`private`；Struct默认是将成员设置为`public`

也可以把Class和Struct理解成有private和public的命名空间
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