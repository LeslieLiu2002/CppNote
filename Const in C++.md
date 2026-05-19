> [!info] `const`对性能和运行不产生影响，它只是程序员间的互相约定，在编译时会检查约定是否正确从而影响编译。
# 修饰简单变量
`const int a；`说明变量a不可修改，是个常量
# 修饰指针
> [!tip] 核心区分方法
> C/C++底层语法是“从右往左读”
- `const int * p`从右往左：`p`是一个变量，`*`指针指向，`int`整型，`const`常量。意思是p是一个变量，这个变量是个指针，指针指向int类型的数据，这个数据是常量。所以`p`可以修改`*p`不可修改
- `int * const p`从右往左：`p`是一个变量，`const`是常量，`*`指针指向，`int`整型。意思是p是一个变量，这个变量是个常量，这个常量是个指针，指针指向int数据类型。所以p不能修改`*p`可以修改
# 修饰类方法
语法：在函数名后添加const
例子：
```c++
class Entity
{
private:
	int m_X,m_y;
public:
	int GetX() const
	{
		return m_X;
	}
};
```
- 被const修饰的函数不能修改成员的值，除非该成员被`mutable`修饰[[Mutable in C++#修补Const修饰类方法]]。
- 保证声明为常量的对象在调用该方法能通过编译，eg：`void PrintEntity(const Entity& e)`和`const Entity e; e.GetX()`。如果不用`const`修饰方法，编译器不知道`e.GetX()`这个方法是否会改变对象`e`，所以不允许通过。