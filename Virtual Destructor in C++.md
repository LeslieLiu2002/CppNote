```c++
class Base
{
public:
	Base() { std::cout << "Base constructor\n"; }
	virtual ~Base() { std::cout << "Base Destructor\n"; }
};
class Derived :public Base
{
public:
	Derived() { m_Array = new int[5]; std::cout << "Derived constructor\n"; }
	~Derived() { delete[] m_Array; std::cout << "Derived Destructor\n"; }
private:
	int* m_Array;
};

Base* poly =new Derived();
delete poly;
```
- **内存泄漏**
	当执行 `delete poly;` 时，如果父类的析构函数不是虚函数（`virtual`），C++ 编译器会根据指针的静态类型（也就是父类 `Base*`）来决定调用哪个析构函数。它根本不知道底层其实是个子类对象。此时在创建poly时又申请了堆内存，因为只执行父类的解构函数，就造成了内存泄漏。
-  **虚析构函数与普通虚函数的区别**
	- 普通虚函数（如 `virtual void print()`）是覆盖（Override）机制，即子类的实现替换父类的实现，**最终只执行一个**。
	- 但虚析构函数是**追加**机制。把父类析构函数设为 `virtual`，并不是让子类析构去替换父类析构，而是告诉编译器：“在销毁时，请**顺着继承链先执行最底层子类的析构，然后再逐级向上执行父类的析构**。”两者都会被调用。例如本例的输出就是`Derived Destructor Base Destructor`，即**先执行了子类的解构函数然后再执行父类的解构函数** 
	- 所以不需要在声明解构函数时加`override`关键字
> [!tips] C++ 面向对象编程的铁律
> - **牢记黄金法则**：**只要你的类打算被别人继承（即作为基类使用），你就必须（100%需要）将它的析构函数声明为 `virtual`。**
> - **例外**：如果你确信这个类绝对不会被用作父类（比如一些简单的数学结构体或者使用了 `final` 关键字的类），为了极致的性能（省去虚函数表 V-Table 的开销），你可以不使用虚析构函数。
