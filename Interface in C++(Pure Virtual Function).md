# 实现
```c++
class Entity
{
public:
	virtual void getName() = 0;// 没有函数体，必须用分号;
};
```
# 性质
当给一个类包含了至少一个纯虚函数(pure virtual function)时,这个类就变成了接口(interface)会出现一下性质：
- 该类无法被实例化
- 继承这个类的子类必须实现这个纯虚函数，如果没有实现则无法实例化

> [!info] 接口
> 其他语言里接口可能不是类，但在c++里，接口就是类
