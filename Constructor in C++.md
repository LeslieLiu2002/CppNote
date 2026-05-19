# 基本语法
```c++
class Entity
{
private:
	std::string m_Name;
public:
	Entity(){m_Name="Unknown";}
	Entity(const std::string& name){m_Name=name;}
};
```
> [!tip] 防止对象实例化
> 有时可能只想要一个包含静态方法[[Static in C++]]的工具类，不希望任何人创建这个类的实例，于是有下面两个方法：
> - 将构造函数放在`private`里(构造函数是默认`public`的)
> - 删除构造函数，`对象名() = delete`
# Constructor Initializer List
[[Member Initializer Lists in C++]]
```c++
class Entity
{
private:
	std::string m_Name;
	int m_Score;
public:
	Entity():m_Name("Unknown"),m_Score(0){}
	Entity(const std::string& name):m_Name(name),m_Score(0){}
};
```
- 成员初始化列表需要/会按照成员被声明的顺序进行初始化
# 拷贝构造函数
[[Copying and Copy Constructor in C++#深拷贝]]