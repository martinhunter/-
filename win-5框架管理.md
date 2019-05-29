> 直接使用 **API函数** 写效率过低且容易出错。
MFC(Microsoft Fundation Classes,Microsoft基础类库）是对API函数的简单封装，简化开发过程
开发者的MFC类库设计过程

### 运行时类信息（CRuntimeClass类）
> 在程序运行过程中辨别对象是否属于特定类的技术叫动态类型识别（Runtime Type Information，RTTI）
1.用于函数识别其参数类型。2.用于针对对象所属类编写特定目的的代码。

框架程序使用RTTI管理应用程序，用户可以设计自己的类，框架程序可在程序执行过程知道这个类的类名、大小等信息，并能创建类对象。

#### 动态类型识别和动态创建
给类使用唯一标识（同进程ID一样）。
> 类的静态成员（python中的类属性、类方法？）不属于（实例）对象的一部分，而是类的一部分。定义类时，编译器已经为类的静态成员分配内存了，不论多少实例，静态成员只有一份。因此，给每个类安排1个静态成员变量，它的内存地址即是类的唯一标识。
```C++
class RecoClass{
	public:
		const int* GetRuntimeClass(){
			return &classAddress;
			}
		stacic const int classAddress;
};
const int RecoClass::classAddress = 1;  // 初始化一个任意值，其他类创建的静态成员也可以是相同值，因为它们地址值不同。
