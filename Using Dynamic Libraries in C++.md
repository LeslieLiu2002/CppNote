# 库的组成部分
典型的 C++ 库由两部分组成：**Include 目录**（包含 `.h` 头文件，提供函数声明）和 **Lib 目录**（包含预编译的二进制库文件，提供函数定义）。在Lib目录中，`XXdll.lib`是用于动态链接的库文件，但它只包含了一系列指向真正`.dll`文件中函数的指针/引用
# 动态链接的两种常见方法
- **隐式/依赖型加载**：可执行文件在启动时会检查是否有所需的 `.dll` 文件，如果找不到，程序会直接报错弹窗拒绝启动。
- **显式/动态加载**：程序在代码中手动调用系统 API（如 Windows 下的 `LoadLibrary`）在运行时随时加载或卸载 `.dll` 并提取函数指针。
# 配置动态链接的实战演示
- 在 Visual Studio 中，动态链接 GLFW 的过程与静态链接[[Using Libraries in C++ (Static Linking)#Visual Studio 实战配置]]几乎一样，唯一的区别在于链接的 `.lib` 文件名不同。
- **切换库文件**：在 `Linker -> Input -> Additional Dependencies` 中，将上一节课的静态库 `glfw3.lib` 更改为对应动态库的导入库 **`glfw3dll.lib`**。`.lib`文件必须和配套的`.dll`文件一起编译,否则会导致函数地址不匹配
> [!tip] `xx.dll was not found`找不到DLL文件错误
> 将dll文件复制并放置在与生成的可执行文件同一级的目录下。因为程序启动时，系统默认会在应用程序的根目录中搜索所需的动态库

> [!info] 宏定义`__declspec(dllimport)`导入/导出声明
> 在使用DLL时，通常需要在头文件声明函数时使用`__declspec(dllimport)`（例如定义宏`#define GLFWAPI __declspec(dllimport)`来启用它）。此时编译器就会知道这是一个DLL函数，从而直接进行调用。但如果不在函数前加上它，编译器会把它当作普通的外部函数，生成间接调用代码。
