函数指针指向的是**函数指令在内存中的地址**。
> [!tips] 函数返回值是左值还是右值的判断方法
> - **非引用返回类型** → 右值（纯右值）`int func() {int x = 0; return x; }`
> - **左值引用返回类型** → 左值 `int& func() { static int x = 0; return x; }`
> - **右值引用返回类型** → 右值（亡值）`int&& func() { return 42; }  // 不推荐，仅示例`
> 以`T a = func()`为例：
> 但如果编译器有RVO（返回值优化）机制，那么编译器会直接将a的地址隐式传入函数，直接在函数内部进行构造，就算你返回的是一个局部变量(例如`x`)，这个局部变量也是指向这个地址的，所以对`x`的操作都是在对`a`操作，**完全省略拷贝和移动**。
> 如果没有RVO（返回值优化）机制，才会按照上面的返回类型先后判断是否可以使用移动、拷贝构造函数然后调用
# 语法
```c++
auto function =HelloWorld;// 其中HelloWorld是一个函数名，function是函数指针
// function()==HelloWorld()// 加上括号就是在调用函数，它们两的结果一样

// 函数指针的类型声明：返回类型 (*变量名)(参数类型1,参数类型2,...)
int (*func)(int,int);
func=add;//add是两整数相加的函数

// 类型别名简化代码
typedef int (*FuncPtr)(int, int);
using FuncPtr = int (*)(int,int);
```
> [!info] typedef的基本语法
> `typedef 原类型 新别名;`
> 函数指针的原类型是`int (*)(int,int)`，新别名是`FuncPtr`
# 应用场景
函数指针最强大的应用场景之一是**回调函数 (Callback)** 和自**定义迭代操作**。
```c++
void Print(int value)
{
	std::cout << value << std::endl;
}

typedef void (*PrintPtr)(int);
using PrintPtr = void (*)(int); // 也是可行的

void ForEach(const std::vector<int>& data, PrintPtr func)// PrintPtr只是类型，所以还要定义一个形参
{
	for (int datum : data) func(datum);
}
// 或者不使用别名的方法
// void ForEach(const std::vector<int>& data, void (*func)(int))
// {
//  	for (int datum : data) func(datum);
// }

std::vector<int>data{ 1,5,4,3,2 };
ForEach(data, Print);// 1 5 4 3 2
```
