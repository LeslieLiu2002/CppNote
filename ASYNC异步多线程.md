# 使用`std::async`实现异步多线程加载
- 引入 `<future>` 头文件。
- 使用 `std::async` 函数将模型的加载任务调度到后台线程中执行。通过指定 `std::launch::async` 策略，强制程序开启新线程来异步处理这些加载任务。如果省略该参数，编译器可能会默认采用 `std::launch::deferred` 策略（延迟执行），这会导致任务根本不会在后台线程并发执行，而是等到你索要结果时才在当前线程执行。
```c++
std::async(std::launch::async,LoadMesh,&m_Meshes,file);//将下面的函数以及参数传入线程
```
# 处理多线程数据共享冲突（Mutex互斥锁）
- 当多个后台线程同时加载完模型，并试图将模型数据添加（`push_back`）到主线程的同一个 `std::vector` 时，会引发数据竞争（Data Race），导致程序崩溃。
- 解决方案是引入 `std::mutex`（互斥锁）和 `std::lock_guard`。在写入 `vector` 的瞬间加锁，确保同一时刻只有一个线程能向数组中添加数据，添加完后自动解锁。
```c++
std::mutex s_MeshesMutex;
void LoadMesh(std::vector<Ref<Mesh>>* meshes,std::string filepath)// 这里string不使用引用赋值的原因是引用对象有可能会在线程启动时销毁。meshes不用引用的原因是，调用std::async时会拷贝参数成一个临时变量然后再传入LoadMesh
{
	auto mesh=Mesh::Load(filepath);// 从文件加载模型数据
	std::lock_guard<std::mutex> lock(s_MeshesMutex);
	meshes.push_back(mesh);
}
```
# 管理异步任务的生命周期 (`std::future`)
- `std::async` 会返回一个 `std::future` 对象。视频中强调，**必须将这些返回的对象保存起来**（例如存入一个专门的 `vector<std::future<void>>` 中）。如果没有被变量接收或保存，它会在当前语句结束时立即析构。而 **`std::future` 的析构函数默认会阻塞（阻塞等待异步任务完成）**。这会导致你的程序看起来写了并发代码，但实际上依然是串行执行的。视频中通过将它们推入 `m_Futures` 数组来保持它们的生命周期。
```c++
std::vector<std::future<void>> m_Futures;// 在类中定义一个存放std::future的数组，这里使用void是因为std::async调用的函数返回值是void

// 把std::async返回的对象存入数值
m_Futures.push_back(std::async(std::launch::async,LoadMesh,&m_Meshes,file));
```