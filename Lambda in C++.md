- Lambda 本质上是一种定义**匿名函数**（没有实际函数名、用完即弃的函数）的方式。它允许你在代码的执行点以“内联（inline）”的方式直接编写一个函数体，而不需要在全局或类中去正式声明和定义一个传统的函数。
- 使用场景：任何需要传递“函数指针”或“回调函数”的地方[[Function Pointer in C++#应用场景]]
# 基本语法
- `[captures](parameters) { body }`
- `[]`：**捕获列表（Capture List）**，用于决定 Lambda 内部如何访问其外部作用域的变量。
	这是 Lambda 独有的特性。如果 Lambda 内部需要使用在 Lambda **外部定义的变量**（例如外层函数的一个局部变量 `a`），必须通过捕获列表传进去
	- `[=]`：按**值**（Copy）捕获外部所有变量。只是外部变量的一个副本
	- `[&]`：按**引用**（Reference）捕获外部所有变量。是外部变量的本体
	- `[a]`：仅按值捕获变量 `a`。
	- `[&a]`：仅按引用捕获变量 `a`。
- `()`：**参数列表**，和普通函数的参数一样，比如 `(int value)`。
- `{}`：**函数体**，具体的执行逻辑和返回值。
# 注意点
1. 在多线程或异步编程中使用 Lambda 时，如果**按引用捕获**了局部变量，而该局部变量在 Lambda 真正被执行前已经被销毁（脱离了作用域），就会导致悬空引用（Dangling Reference）引发程序崩溃。
2. Lambda按值捕获变量时（例如 `[a]`），这个副本在 Lambda 函数体内是**只读的（const）**，无法在 Lambda 内写 `a = 10;`。如果想修改，必须在参数列表后加上 `mutable` 关键字，例如 `[a]() mutable { a = 10; }`。
3. 函数指针（Raw Pointers）与 `std::function` 的兼容性问题：
	- 如果 Lambda **有捕获列表**（比如捕获了外部变量），它就**不能**隐式转换为原始的 C 风格函数指针（`void(*)(int)`）[[Function Pointer in C++]]。
	- 为了支持带有捕获状态的 Lambda，接收回调函数的参数类型必须改为 C++11 标准库中的 **`std::function`**（需要 `#include <functional>`）。例如：`void ForEach(const std::vector<int>& data, const std::function<void(int)>& func)`。
	```c++
	#include<functional>
	
	void Print(int value)
	{
		std::cout << value << std::endl;
	}
	
	// void ForEach(const std::vector<int>& data, void (*func)(int))
	void ForEach(const std::vector<int>& data, const std::function<void(int)>& func)
	{
		for (int datum : data) func(datum);
	}
	```