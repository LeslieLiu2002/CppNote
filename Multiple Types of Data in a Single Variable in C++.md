> [!info] C++17中引入了std::variant，它提供了一种类型安全的方式，允许我们在*同一个变量中存储多种不同类型的数据*
# 什么是`std::variant`
`std::variant` 就像一个类型安全的 `union`（联合体）。==通过引入 `<variant>` 头文件==，你可以声明一个变量并指定它可能包含的所有类型，例如 `std::variant<std::string, int> data;`，这意味着 `data` 可以在某个时刻存储一个字符串，在另一个时刻存储一个整数。
# 赋值和读取
- **赋值**：非常直接，你可以直接将对应类型的值赋给变量，例如 `data = "Cherno";` 或 `data = 2;`。
- **读取 (`std::get`)**：可以使用 `std::get<类型>(变量)` 来获取数据，例如 `std::get<std::string>(data)`。
# 获取当前的数据类型 (`index`)
由于一个变量可能有多种类型，可以使用 `data.index()` 方法来获取当前实际存储的数据类型的索引（基于声明时的类型顺序，索引从 0 开始）。
# 更安全的读取方式 (`std::get_if`)
==如果使用 `std::get` 读取了错误的类型（例如里面存的是 `int` 却硬要作为 `string` 读取），程序会抛出 `std::bad_variant_access` 异常==。为了避免异常，视频推荐使用 `std::get_if<类型>(&变量)`。它接受的是**变量的地址**，如果类型匹配，则返回指向数据的指针；如果不匹配，则返回空指针 (`nullptr`)。
```c++
if (auto value = std::get_if<std::string>(&data)) {
    // 类型匹配成功，通过解引用 *value 使用字符串
    std::cout << *value << std::endl;
}
```
# 内存与底层原理
`std::variant` 和 C 语言中的传统 `union`的差别：`std::variant` 的大小（`sizeof`）通常是它**最大成员的大小加上一点额外的空间**（用于记录当前存储的是哪个类型的索引）。
# 实际应用场景(对比 `std::optional`)
视频最后将 `std::variant` 与上一个视频讲的 `std::optional` [[Optional in C++]]进行了对比。`std::optional` 只能表达“有数据”或“没有数据”，而在读取文件等场景中，如果我们不但想知道是否失败，还想知道**失败的具体原因**（即返回错误码），就可以使用类似 `std::variant<std::string, ErrorCode>` 的形式，让函数要么返回成功读取的字符串，要么返回一个枚举形式的错误码。
> [!tip] `variant` vs 传统 `union` 的取舍
> 虽然传统的 `union` 内存占用更极致（绝对没有额外开销），但传统的 `union` 在处理非平凡类型（比如 `std::string` 等包含复杂构造/析构函数的 C++ 类）时非常麻烦且容易引发未定义行为。
