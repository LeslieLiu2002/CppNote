将源文件(as translation unit)编译为中间文件(名为对象文件)，编译的最终功能是将所有代码编译为数据或者指令
___
# Files
>[!tip] files have no meaning in c++,they only provide content

- If a c++ project contains many cpp files which include them in each other, the cpp files will be equal to a object file.
- If a c++ project contains many individual cpp files, they will be translated into an object file respectively.
___
# 预处理
预处理阶段，编译器处理所有预处理语句(included: \#include, \#define, \#if - \#endif...)
## \#include语句
编译器在预处理阶段处理include语句时会将include的文件打开然后复制粘帖里面的内容到源文件里
## \#define语句
define语句会将A替换成B `#define A B`
## \#if - \#endif
根据条件选择是否编译该段代码
