> [!info] 重要理解
明白一个重要的点：对于c++而言，文件类型不重要，它们只是用来提供内容，都是编译器的翻译单元！！！首先根据预处理语句进行文本修改(include复制粘帖文件内容，define替换文本内容，if判断用不用该文本)。然后进行编译检查语法错误(例如调用函数时是否存在对应的函数声明)，将处理完的文本编译成单独的机器语言文件，称为对象文件。最后链接，检查所有对象文件的内容，然后对被调用的函数寻找函数定义，如果没找到或者找到多个都会报错。
# 1.预处理
把头文件([[C++ Header Files]])复制粘帖到源文件里，源文件文本替换，源文件文本是否编译([[How the C++ Compiler Works#预处理]])
___
# 2.编译与链接
编译器([[How the C++ Compiler Works]])将源文件转换为机器语言
在vs code需要配置编译任务(task.json)和调试环境(launch.json)
目前建议不要在vscode里开发，在xcode或者VS里开发
> [!info]- task.json模版
> ```json
> {
> 	"version":"2.0.0",
> 	"tasks": [
> 	{
>            "label": "Build C++",
>            "type": "shell",
>            "command": "clang++",
>            "args": [
>                "-std=c++17",
>                "-g",
>                "${file}",
>                "-o",
>                "${fileDirname}/${fileBasenameNoExtension}"
>            ],
>            "group": {
>                "kind": "build",
>                "isDefault": true
>            }
>        }
> 	]
> }
> ```

> [!info]- launch.json模版
>```json
>{
>    "version": "0.2.0",
>    "configurations": [
>        {
>            "name": "Debug C++",
>            "type": "lldb",
>            "request": "launch",
>            "program": "${fileDirname}/${fileBasenameNoExtension}",
>            "args": [],
>            "cwd": "${workspaceFolder}",
>            "preLaunchTask": "Build C++"
>        }
>    ]
>}
>```

> [!tip]+ TIPS
> - 头文件不会被编译，只有cpp源文件会被编译
> - 头文件通过预处理器声明（名为include）被包含进cpp文件

cpp文件会被编译为目标文件(object file)然后通过链接器(linker)[[How the C++ Linker Works]]链接到一起
____
# 3.项目配置（vscode）
## 3.1.创建项目结构
```
my_cpp_project/
├── src/           # 源代码目录
│   └── main.cpp
├── include/       # 头文件目录
├── .vscode/       # VSCode 配置目录
│   ├── tasks.json     # 编译任务配置
│   ├── launch.json    # 调试配置
│   └── c_cpp_properties.json  # 智能提示配置（可选）
└── Makefile       # 编译脚本（可选）
```
## 3.2.配置 c_cpp_properties.json（智能提示）
在项目根目录创建 `.vscode/c_cpp_properties.json`：
```json
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/local/include",
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include"
            ],
            "defines": [],
            "macFrameworkPath": [
            "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "macos-gcc-x64"
        }
    ],
    "version": 4
}
```
## 3.3.使用Makefile配置多文件项目
创建 `Makefile`：
```makefile
CXX = clang++
CXXFLAGS = -std=c++17 -g -Wall
SRCDIR = src
INCDIR = include
SOURCES = $(wildcard $(SRCDIR)/*.cpp)
TARGET = bin/main

$(TARGET): $(SOURCES)
	$(CXX) $(CXXFLAGS) -I$(INCDIR) $^ -o $@

clean:
	rm -f $(TARGET)

.PHONY: clean
```
对应的 `tasks.json`：
```json
{
    "label": "make build",
    "type": "shell",
    "command": "make",
    "group": "build"
}
```