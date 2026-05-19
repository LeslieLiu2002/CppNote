# 浅拷贝
使用赋值运算符时，编译器会先分配一个内存，然后把值简单错报地复制过来，基本数据类型是复制值，指针是复制地址。
# 深拷贝
因为指针只是复制地址，对内存上的内容没有复制，我们引入深拷贝解决指针拷贝值的问题。
```c++
class String
{
private:
	char* m_Buffer;
	unsigned int m_Size;
public:
	String(const String& other):m_Size(other.m_Size)
	{
		m_Buffer=new char[m_Size+1];
		memcpy(m_Buffer,other.m_Buffer,m_Size+1);
	}
	~String()
	{
		delete[] m_Buffer;
	}
};
```
- 示例代码里用拷贝构造函数实现了深拷贝，此时使用`String str=other`就会创建一个在内存上和`other`不一样`str`了，确保新老对象完全独立。
- 拷贝构造函数的标准写法：`类名(const 类名& 引用名)`
- 在开发中，对于类和结构体这样复杂的对象，**永远优先使用**`const Type&`**(常量引用)作为函数形参**
