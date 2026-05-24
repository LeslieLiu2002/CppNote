> [!info] 核心思想
> 继承为我们提供了一种方法，可以将多个类之间的所有公共代码放入一个基类中，这样我们就不必重复编写相同的代码
# 如何实现
- 语法：使用冒号`:`后跟访问修饰符（通常是`public`）和基类的名字。
- 示例：
```c++
	class Entity{
	public:
		float X,Y;
		void Move(float xa, float ya){...}
	};
	class Player : public Entity{
	public:
		const char* Name;
		void PrintName(){...}
	}
```
- 在这个例子中，`Player`类不仅拥有自己的`Name`变量和`PrintNode`方法，它还隐式地拥有`Entity`类中的`X`，`Y`变量和`Move`方法。
# 内存中的体现
使用sizeof方法可以打印出类的大小。
例如：`sizeof(Entity)`就返回8个字节；`sizeof(Player)`返回12个字节
> [!tip] sizeof()
> - 它是一个编译时运算符，意味着在编译阶段完成的。它通过查看非静态成员变量、内存对齐、虚函数表指针以及基础关系算出大小。
> - 误区：不是实例化后计算大小，因为是在编译阶段完成的。静态成员变量和成员函数（普通函数）是不计算在内点。
# 虚函数
[[Virtual Function in C++]]
虚函数引入动态分派(dynamic dispatch)的概念，编译器通常通过虚表(vtable)来实现。
虚表(vtable)是一个包含基类中所有虚函数映射的表，以便我们能在==运行时==将它们映射到正确的重写函数(overridden function)
# 接口/纯虚函数
[[Interface in C++(Pure Virtual Function)]]
纯虚函数(interface/pure virtual function)运行我们在基类中定义一个没有实现的函数，然后强制子类实现该函数
# 可见性
[[Visibility in C++]]
可见性会影响继承，即子类能否看见父类的成员
# 虚解构函数
主要用于解决继承了A类的B类，该B类又申请了堆内存的问题。如果不重写解构函数，在里面释放掉内存，那么下面的情况就会造成内存泄漏:`A var=B();`，如果var被销毁，它只会调用A类的解构函数，于是造成内存泄漏。
[[Virtual Destructor in C++]]
# 继承中的转型
假设有继承关系是Base->A->B，其中Base是基类，A和B是派生类
- **向上转型：** 从B到A或者从A到Base就是向上转型。向上转型是安全的，因为派生类一定拥有父类拥有的成员。
- **向下转型：** 从Base到A或者从A到B就是向下转型。向下转型不一定安全，因为有多态的存在，所以父类变量指向的真正对象可能有派生类的成员。
