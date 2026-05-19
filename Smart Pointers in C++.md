它们的作用都是自动释放内存，防止内存泄露。\*所以这些指针都在栈上定义存储
> [!info] 需要包含头文件`#include <memory>`
# std::unique_ptr
## 基本思想
当一个`unique_ptr`死亡时，它的内存就被释放了。此时如果有指针指向这块内存就会得到错误值，因此只能是unique不能复制(防止多指针指向同一内存)
## 语法：
```c++
std::unique_ptr<T> ptr(new T());
```
- 不能使用隐式转换，即`ptr=new T();`因为`unique_ptr`的构造函数被`explicit`修饰
```c++
std::unique_ptr<T> ptr = std::make_unique<T>();
```
- 这是标准写法，用于处理构造函数的异常，从而防止得到空指针
## 用法
- 把它当指针用，即用箭头运算符`->`取`T`的成员
- 不能复制它`std::unique_ptr<T> e = ptr`这会报错，在给函数传参时容易发生。因为它的拷贝构造函数和拷贝赋值都被删除了
# std::shared_ptr
## 基本思想
引入计数器维护有多少指针指向该内存。一旦计数器为0，则释放该内存。用于解决`unique_ptr`的缺点
## 语法
```c++
std::shared_ptr<T> ptr = std::make_shared<T>();//标准语法
std::shared_ptr<T> ptr(new T());// 可以编译，但是不安全
```
- `shared_ptr`需要分配另一个内存块，称为控制块，用于存储引用计数
- 使用new语法需要分配两次内存（对象一次，控制块一次），而使用标准语法会将对象和控制块一起分配
## 用法
- 可以进行复制，当指向同一内存的shared指针都销毁了，这块内存才能被释放。
# std::weak_ptr
## 基本思想
配合shared_ptr使用，它指向shared_ptr时不改变该对象的引用计数。可以检查对象是否存活。不拥有对象所有权，只是旁观者。
## 语法
```c++
std::weak_ptr<T> ptr=ptr_shared;//不用new或者make_weak的方式赋值

ptr.expired();//查询对象是否被销毁，销毁返回true

std::shared_ptr<T> another = ptr.lock();// 用lock()将weak_ptr提升为shared_ptr
```
## 用法
- 没有重载`*`和`->`，所以不能通过`weak_ptr`直接访问对象成员（即不拥有对象所有权）
- 不用`new`或者`std::make_weak<T>`的方式创建指针
- 想要用`weak_ptr`访问数据要先查询对象是否销毁(`expired()`方法)，然后将它临时提升为`shared_ptr`(`lock()`方法)，通过这个指针去访问数据