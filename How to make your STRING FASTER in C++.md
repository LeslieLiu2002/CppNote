# 背景
在 C++ 中，`std::string` 和许多相关的字符串操作（如格式化、获取子串等）往往会在堆（Heap）上分配内存。频繁的堆内存分配（Heap Allocation）是非常缓慢的。在游戏循环或实时应用程序中，这会严重影响性能（例如导致掉帧）。于是在C++17版本，提出了`std::string_view`
# std::string_view
- **原理**：`std::string_view` 本质上只是一个==指向现有字符串内存的指针==（`const char*`）==加上一个大小==（`size`）。它本质上是现有字符串的一个“视图”或“窗口”。
- **优势**：因为它不拥有（own）数据，只观察数据，所以**完全不需要分配新的堆内存**。通过值传递（pass by value）它非常轻量和高效。
```c++
void PrintName(std::string_view name)
{
	std::cout<<name<<std::endl;
}
std::string name = "Leslie Liu"; // breakPoint1
std::string_view firstName(name.c_str(),6);//c_str()的作用是把std::string转换为c风格的字符串，也就是const char*
std::string_view lastName(name.c_str()+7,3);//把头指针向后移动7位，然后获得子字符串"Liu"

PrintName("Leslie"); // breakPoint2
PrintName(firstName);
PrintName(lastName);
```
- 这里全程只会分配一次内存，即在`breakPoint1`处
- `breakPoint2`不会分配内存，相当于参数指针指向的是栈上的字符串`"Leslie"`
- 其他地方都不分配内存，因为它们只是对已有字符串的观察指针
- 如果把`breakPoint1`改为`const char *`，甚至不会出现一次内存分配。
- 如果把函数参数改成`std::string&`或者使用`name.substr()`获取子字符串，它们都会在堆上分配内存创造新字符串
> [!tips] 几个注意点
>- 必须始终保证被观察的原始数据的生命周期长于 `string_view`。
>- `std::string_view` 依赖于其内部的 `size` 变量来确定长度，它所指向的子串末尾**不一定**有 `\0`（空字符结尾）。如果你需要将 `std::string_view` 传递给需要 C 风格字符串（以 `\0` 结尾，如老式 C API 或 `printf` 的 `%s`）的函数，可能会读取越界，读到垃圾内存。
