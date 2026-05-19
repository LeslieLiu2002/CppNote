# 类的隐式转换
```c++
class Entity
{
private:
	int m_Score;
	char* m_Name;
public:
	Entity(int score):m_Score(score),m_Name(nullptr){}
	explicit Entity(const char* name):m_Score(-1),m_Name(name){}
};

class ScopedPtr
{
private:
	Entity* m_Ptr;
public:
	ScopedPtr(Entity* ptr):m_Ptr(ptr){}
	~ScopedPtr(){delete m_Ptr;}
};

Entity e=2; // 可以编译，隐式调用第一个构造函数
Entity e="Leslie"; // 不可以编译，因为第二个构造函数被explicit修饰，不能隐式转换
ScopedPtr e = new Entity(); // 可以编译，先创建一个Entity对象，然后隐式调用构造函数
```
- 当类中有对应构造函数时，可以使用等号+值的方式进行隐式转换
- 使用explicit关键字会取消该构造函数的隐式转换功能
- 其中的`ScopedPtr`类似智能指针[[Smart Pointers in C++]]中的`unique_ptr`