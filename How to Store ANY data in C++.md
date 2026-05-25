> [!tip] 少用`std::any`
> 如果你需要在变量中存储多种已知类型，应该优先使用 `std::variant`[[Multiple Types of Data in a Single Variable in C++]]，因为它类型更安全且性能更好。
> 如果你在程序设计中发现自己真的需要一个变量能塞下“全宇宙所有类型”，那你可能需要重新审视并重构你的代码架构设计了。
# 基本概念
- **核心特性**：`std::any` 是 C++17 引入的新特性，允许你在同一个变量中存放几乎任何数据类型（整数、指针、字符串、自定义类等）。其实原理有点像Type punning[[Type Punning in C++]]
- **基础用法**：使用前需 `#include <any>` 并使用支持 C++17 的编译器。声明变量后（如 ==`std::any data;`==），可以直接对其赋任意值。
# 读取数据
- 存入数据很容易，但读取数据需要做额外的工作。
- 必须知道变量当前存储的具体类型，并使用 `std::any_cast<Type>(variable)` 进行强制转换提取，例如 `std::any_cast<std::string>(data)`。
- **指定的类型必须与实际存储的类型完全严格一致**。例如`std::any data="Leslie"`此时存入的是 `const char*`。如果尝试用 `std::string` 去强转，例如`std::string str=std::any_cast<std::string>(data)`，程序并不会自动帮你转换，而是会直接抛出 `std::bad_any_cast` 异常导致崩溃。
# 底层机制
`std::any` 针对小块数据（例如 32 字节以内，依编译器实现如 MSVC 而定）会采用类似 Union 的局部存储（Small Buffer Optimization）；但当数据类型超过该大小时，它会在底层调用 `new` 进行**动态内存分配**（堆分配），从而拖慢程序性能。
> [!tip] 读取时的数据拷贝问题
> 从 `std::any` 提取复杂对象（比如长字符串或大结构体）时，如果直接写 `std::any_cast<std::string>(data)`，会发生一次完整的数据拷贝。由于该函数是对已有内存进行处理(data指向的内存)，所以不会触发ROV[[Function Pointer in C++]]机制，所以无法跳过数据移动和拷贝。此时最正确的优化是改用引用：写成 `std::any_cast<std::string&>(data)`，直接按引用返回，从而省去昂贵的拷贝开销。
- `std::any_cast<T>`的重载
	```c++
	template<class T>
	T* any_cast(any* operand) noexcept;//接收指针参数
	template<class T>
	T any_cast(any& operand);// 接收值或者引用参数
	```
	所以正确使用方法是：
	- `any_cast<std::string>(data)`，值重载，T的实际类型`std::string`，返回类型`std::string`
	- `any_cast<std::string&>(data)`，引用重载，T的实际类型`std::string&`，返回类型`std::string&`
	- `any_cast<std::string>(&data)`，值重载，T的实际类型`std::string`，返回类型`std::string*`