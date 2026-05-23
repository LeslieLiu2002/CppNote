# 基础语法
```c++
#include<iostream>
#include<thread>

static bool s_Finished = false;
void DoWork()
{
	using namespace std::literals::chrono_literals;

	std::cout << "Started thread id=" << std::this_thread::get_id() << std::endl;

	while (!s_Finished)
	{
		std::cout << "Working...\n";
		std::this_thread::sleep_for(1s);
	}
}

int main()
{
	std::thread worker(DoWork);// 线程创建并运行
	std::cin.get();// 主线程等到输入并阻塞，此时因为s_Finished没有改变，线程一直工作
	s_Finished = true;// 一旦用户输入，主线程得到资源，重新开始运行，从而改变s_Finished

	worker.join();// 等待线程结束，因为s_Finished改变，所以也结束了
	std::cout << "Finished." << std::endl;
	std::cout << "Started thread id=" << std::this_thread::get_id() << std::endl;
	
	return 0;
}
```
- **创建和启动线程**
	引入 `<thread>` 头文件后，可以通过实例化 `std::thread` 对象来创建一个新线程。只需将一个函数指针（或 Lambda 表达式）作为参数传递给其构造函数，例如 `std::thread worker(DoWork);`。该线程在对象创建完成的瞬间就会**立刻启动**。
- **等待线程结束(`join`)**
	通过调用 `worker.join()`，主线程会被**阻塞（Block）**，直到 `worker` 线程内部的函数完全执行完毕，主线程才会继续往下走。
# 重难点
1. **内存共享**：多线程最强大的地方（同时也是最危险的地方）在于它们**共享同一块内存空间**。
2. **隐形的线程冲突（Race Conditions 预警）**：
3. **忘记 `join` 或 `detach` 导致的崩溃**：如果你创建了一个 `std::thread` 对象，但在该对象脱离作用域（被销毁）之前，你既没有调用 `.join()` 等待它，也没有调用 `.detach()` 让它在后台独立运行，C++ 程序在运行时会直接**抛出异常并崩溃（`std::terminate` 被调用）**。
4. **阻塞与 CPU 占用 (Blocking vs Spinning)**:实际开发中，**后台轮询线程**通常需要搭配 `std::this_thread::sleep_for`（哪怕只睡眠几毫秒）或者使用条件变量（`std::condition_variable`）来降低 CPU 无效消耗。