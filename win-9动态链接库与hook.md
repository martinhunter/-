### 9.1 动态链接库（dynamic link library,除了dll，系统中某些exe、控件（*.ocx）也可能是动态链接库）
> 作用：API都是从系统动态链接库中导出。方便更新和重用模块化程序更方便。几个程序同时使用相同函数时，减少内存消耗，因程序代码是共享的。
> 概念： 是应用程序的一个模块，用于导出一些函数和数据供程序中其他模块使用。
1. 是应用程序的一部分，本质上与可执行文件相同，都作为模块被进程加载到自己的空间地址。
2. 在程序编译时不会被插入到可执行文件（exe），在程序运行时整个库的代码才会调入内存
3. 多程序用同一动态链接库时，windows在物理内存中止保留1份库的代码，仅仅通过分页机制将这份代码映射到不同进程中。

入口为DllMain函数
BOOL APIENTRY DllMain(HANDLE hModule,DWORD ul_reason_for_call, LPVOID lpReserved)
DLL定义导出函数和内部函数。
导出函数可被其他模块调用

### 9.2 hook
> **windows程序的运行模式是基于消息驱动，任何线程只要注册窗口类都会有一个消息队列接受用户的输入消息和系统消息**
> 作用：hook应用于各种监测程序中，必须写在动态链接库以便注入其他进程。使用hook来获取特定线程接受或发送的消息。

概念：应用程序可以安装1个子程序（hook函数）以监视特定窗口某种类型的消息，这窗口可以是其他进程创建的。
在消息到达后，目标窗口处理函数处理前，hook允许应用程序截获它进行处理。
hook是一个处理消息的程序段，通过调用相关API函数，将它挂入系统
* hook用来截获系统中的信息流。
* 截获消息后，用于处理消息的子程序叫hook函数，它是应用程序自定义的一个函数，在安装hook时要把这个函数的地址告诉windows
* 系统同一时间可能有多个进程安装hook，多个hook函数组成hook链。所以在处理截获到的消息时，应把消息事件传递下去，以便其他hook有机会处理。
* hook会使系统变慢，它增加了系统对每个消息的处理量。

```C++
// 安装hook，使用SetWindowsHookEx函数
HHook SetWindowsHookEx(
	int idHook,  // 指定hook类型
	HOOKPROC lpfn,  // hook函数的地址。
	HINSTANCE hMod,  // hook函数所在dll的实例句柄。如果是局部的hook，参数为NULL
	DWORD dwThreadId  // 指定要为哪个线程安装hook。如为0，该hook被解释成系统范围内的
);
```
钩子安装后如果有相应的消息发生，windows将调用lpfn参数指向的函数。如idHook值为WH_MOUSE,则目标线程消息队列中有鼠标消息取出时，lpfn指向的函数就会被调用。
* 如果dwThreadId为0，或指定一个有其他进程创建的线程ID，lpfn参数指向的hook函数必须位于dll中。这是因为进程的地址空间是相互隔离的，
发生事件的进程不能调用其他进程地址空间中的hook函数。此时hook函数必须放在dll中，这种hook叫做远程hook
* 如果dwThreadId为进程自身创建的线程ID，不必写入dll，称为局部hook。
* 如果dwThreadId为0，hook函数将关联到系统内的所有线程

```C++
// hook函数的一般形式
// HookProc是应用程序定义的名字。nCode是Hook代码，hook函数用nCode确定任务，其值依赖于Hook类型
LRESULT CALLBACK HookProc(int nCode， WPARAM wParam, LPARAM lParam){
	// 处理消息的代码
	return ::CallNextHookEx(hHook,nCode, wParam, lParam);  // hHook是安装hook时得到的hook句柄（SetWindowsHookEx的返回值）
}
// 卸载hook
BOOL UnhookWindowsHookEx(HHOOK hhk);  // hhk是安装hook时得到的hook句柄（SetWindowsHookEx的返回值）
```

#### 监视键盘输入hook实例
```C++
// create keyhooklib.dll
// 当程序调用GetMessage或PeekMessage函数，并有键盘消息(WM_KEYUP或WM_KEYDOWN)将被处理


```
