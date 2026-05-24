```c++
std::tuple<std::string, int>CreatePerson()
{
	return {"Leslie",22};
}
auto[name,age]=CreatePerson();// Structured Binding结构化绑定
std<<cout<<name<<" "<<age;//name对应string，age对应int
```
- 只在C++17及以上版本可用，修改版本：右键进入项目属性 (Properties) -> C/C++ -> Language，将 **Language Standard** 明确设置为 **C++17**
- 避免滥用，如果返回的数据仅在一次函数调用里使用，那可以使用结构化绑定避免污染命名空间，如果返回的数据被频繁使用，用struct定义结构体会更好