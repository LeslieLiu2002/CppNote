# 现代C++计时方案
标准库 `<chrono>` 提供了一套跨平台且高精度的计时方案，推荐在 99% 的场景下默认使用。
- **基本语法**
	- 引入 `<chrono>` 头文件
	- 在代码块开始前调用 `std::chrono::high_resolution_clock::now()` 获取一个时间点（`start`）
	- 在代码块结束后再次调用 `now()` 获取结束时间（`end`）。
	- 通过计算 `end - start`，并使用 `std::chrono::duration` 将差值转换为秒（s）、毫秒（ms）或微秒（us）进行输出。
- **优雅的封装**
	```c++
	struct Timer 
	{
		std::chrono::time_point<std::chrono::steady_clock>start, end;
		std::chrono::duration<float>duration;
		Timer()
		{
			start = std::chrono::high_resolution_clock::now();
		}
		~Timer()
		{
			end= std::chrono::high_resolution_clock::now();
			duration = end - start;
			float ms = duration.count()*1000.0f;
			std::cout << ms << std::endl;
		}
	};
	```
	**用法**：只需在任何想要计时的函数或作用域的开头加上一句 `Timer timer;`，当代码执行离开该作用域时，`Timer` 对象被销毁，自动输出耗时。