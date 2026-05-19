> [!tip] 模板就像是**“蓝图”或宏**，它允许你制定一套规则，让编译器为你自动编写代码
# 函数模版
在函数上一行通过 `template<typename T>` 声明模板，`typename`关键字是在告诉编译器用后面的数据类型进行替换，eg：
```c++
template<typename T>// 也可以把typename换成class，但不建议
void Print(T value)
{
	std::cout<<value<<std::endl;
}
```
可以显式指定类型调用（例如 `print<int>(5)`），或者让 C++ 自动推导参数类型（直接调用 `print(5)`）
# 类模版
- 模板不仅适用于函数，还能用于整个类（C++ 标准模板库 STL 就是基于此）。
- **非类型模板参数**：模板参数不仅可以是类型（`typename T`），还可以是具体的值（如 `int N`）。
```C++
template<typename T,int N>
class Array
{
private:
	T m_Array[N];
public:
	int GetN()const{return N;}
};
// 可以这样定义变量
Array<int,5>arr;
```
> [!info] 编译期实例化
> 模板本质上是 C++ 的一种“元编程”——你不是在编写运行时执行的代码，而是在编写**告诉编译器在编译期如何生成代码的代码**
