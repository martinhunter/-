## [DATATYPES:](https://docs.microsoft.com/en-us/windows/win32/winprog/windows-data-types)

DWORD:unsigned integer

HDC:A handle to a device context(DC)

## PREFIX:
H:handle
CS:Class Style
WND:Window
M:Message
WM:WindowMessage

### WINAPI: 程序入口

CALLBACK WNDPROC:窗口过程函数，为WNDCLASS的参数
WNDCLASS 1.设计窗口类
RegisterClass(&wndclass)2.注册窗口类
3.创建窗口
4.显示及更新窗口
