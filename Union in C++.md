> [!info] Union最本质的区别
> 内部的**所有成员共享同一块内存空间**。一个 Union 的总大小等于它内部**最大成员**的大小。每次只能用其中一个身份来解释这块内存。Union 最强大也是最常见的用法——**为同一块内存提供不同的访问别名**
```c++
struct Vector2
{
	float x, y;
};
struct Vector4
{
	union 
	{
		struct
		{
			float x, y, z, w;
		};
		struct
		{
			Vector2 a,b;
		};
	};
};
Vector4 vector = {1.0f,2.0f,3.0f,4.0f};
```
因为共享最大内存，同时`float x,y,z,w`和`Vector2 a,b`在内存上又是对齐的，因此`x, y`的内存完全等价于`a`，而`z, w`的内存完全等价于`b`。
- 该例子中有大量**匿名**`struct`和`union`（即没有给它们起名）。这样做的好处是可以将匿名区域内的变量提升到父类的作用域中，可以直接用`vector.x`或`vector.a`访问。如果起了名字，访问变量就会多一些嵌套（例如`vector.myUnion.myStruct.x`）。
- **限制条件**：Union 不能无所不能。它内部不能包含带有**虚函数 (virtual methods)** 的类，在某些较早的 C++ 标准中，甚至不能包含带有自定义构造函数/析构函数的复杂对象（尽管现代 C++ 对此有所放宽，但手动管理它们的生命周期会非常麻烦）。
> [!tip] Union
> Union 最好只用来包装**纯数据 (POD - Plain Old Data)**，比如数学向量、颜色值 (RGB / XYZ)、或者底层字节流。
