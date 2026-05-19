> [!info] "Library" 的翻译就 “库”
# 依赖管理理念
- 相比于使用包管理器，作者更推荐将第三方库的二进制文件或源码直接放在项目（Solution）的文件夹中。这样任何人克隆代码后都可以直接编译运行，无需繁琐的环境配置 。
- 对于严肃的大型项目，建议直接从源码编译依赖库；但如果是为了快速开发，使用预编译好的二进制文件（Binaries）会更加方便 。
# 库的组成部分
典型的 C++ 库由两部分组成：**Include 目录**（包含 `.h` 头文件，提供函数声明）和 **Lib 目录**（包含预编译的二进制库文件，提供函数定义）。在Lib目录中，`XX.lib`是用于静态链接的库文件。
# Visual Studio 实战配置
> [!tip] 注意将配置文件Configuration改为自己想要的文件，一般设置为All Configurations；还有把平台改成目标平台(eg:Win32、Win64)
1. **配置头文件**：在项目的 `C/C++ -> General -> Additional Include Directories` 中添加 Include 文件夹的路径。该文件夹包含头文件`#include<头文件名>`或者包含头文件的文件夹`#include<文件夹/头文件名>`[[C++ Header Files#尖括号`< >`和双引号`" "`的区别]]
2. **配置库文件**：在 `Linker -> General -> Additional Library Directories` 中添加 Lib 文件夹的路径。该文件夹一般包含`XX.lib, XX.dll, XXdll.lib`文件。用于告诉链接器去这个文件夹中找函数定义
3. **链接具体库**：在 `Linker -> Input -> Additional Dependencies` 中输入具体的库文件名（如 `glfw3.lib`）。用于告诉链接器在指定文件夹里链接指定文件
> [!tips] 注意事项
> - 在Visual Studio里，使用 Visual Studio 提供的宏（如 `$(SolutionDir)`），将路径设置为相对于解决方案所在的相对路径
> - 许多底层的库（比如 GLFW）是用 C 语言编写的。由于 C++ 支持函数重载，编译器会对函数名进行“名称修饰（Name Mangling）”。如果你直接在 C++ 中==手动声明一个 C 库的函数==，链接器会用 C++ 的修饰规则去寻找它，从而导致找不到该函数。此时必须在声明前加上 `extern "C"`，告诉编译器用 C 语言的规则来处理这个函数名。
