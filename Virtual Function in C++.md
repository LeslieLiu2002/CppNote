# 背景/为什么出现
子类拥有父类的成员，因此可以定义一个父类指针来指向子类实例。但此时编译器会偷懒，如果子类重写了父类的一个方法，利用这个指针调用该方法时只会返回父类定义的函数内容。
```c++
class Entity
{
public:
	std::string getName(){return "Entity";}// 基类的方法
};

class Player
{
private:
	std::string m_Name; 
public:
	Player(const std::string& name): m_Name(name){}
	std::string getName(){return m_Name;}//重写的方法
}

Entity* entity = new Player("Leslie");
cout<< entity->getName(); // 输出"Entity"，不是我们想要的"Leslie"
```
# 实现
要让编译器在==程序运行时==检查==真正对象类型==。
使用`virtual - override`关键词，在基类方法前加上`virtual`关键字，在重写方法后加上`override`关键字：
```c++
class Entity
{
public:
	virtual std::string getName(){return "Entity";}// 前加virtual
};

class Player
{
private:
	std::string m_Name; 
public:
	Player(const std::string& name): m_Name(name){}
	std::string getName() override {return m_Name;}//后加override
}
```
如果子类没有重写也不要紧，会返回父类的定义。这是区别纯虚函数的一个方法，纯虚函数必须被子类实现，不然无法编译[[Interface in C++(Pure Virtual Function)#性质]]。
## 虚表(vtable)
- 当类中包含至少一个虚函数时，编译器会为这个类生成一个vtable
- 这个表包含了映射关系，指向当前类应该调用的正确的函数实现地址
- 如果子类重写了虚函数，vtable就会更新对应的指针
## `override`关键字
- 提高可读性
- 编译器防错：如果拼写错了函数名，或者基类中同名的函数没有被标记为`virtual`，编译器就会报错
# 性能代价
- 内存开销：拥有虚函数的对象在内存中会稍大一点，因为它需要额外存储一个指向vtable的指针
- 运行时代价：每次调用虚函数时，程序都要“先查表，再跳转”