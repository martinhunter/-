### 4. 图形界面
在VC++6版本中
显示图形界面的入口函数不是`int main(int argc, char* argv[]){code block};`而是

```C++
hInstance = (HINSTANCE) GetModuleHandle(NULL);  // 应用程序的实例句柄（即模块句柄）
int APIENTRY WinMain(HINSTANCE hInstance,  // 本模块的实例句柄
                      HINSTANCE hPrevInstance,  // 2006年后已不再用
                      LPSTR lpCmdLine,  // 命令行参数
                      int nCmdShow){code block}  // 主窗口初始化时的显示方式
```
windows的消息驱动
windows不断向程序发送消息，通知程序用户进行了什么操作，将信息作为参数传递给目标程序中的窗口函数（window Procedure或叫消息处理函数）
窗口函数的函数原形`LRESULT CALLBACK MainWndProc(HWND, UNIT, WPARAM, LPARAM);`

创建窗口的函数：

```C++
LRESULT CALLBACK MainWndProc(HWND, UNIT, WPARAM, LPARAM);
int APIENTRY WinMain(HINSTANCE hInstance,  HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow){
	char szClassName[] = "MainWClass";
	WNDCLASSEX wndclass;  // 用描述主窗口的参数填充WNDCLASSEX结构
	wndclass.cbSize = sizeof(wndclass);
	wndclass.stle = CS_HREDRAW|CS_VREDRAW;  // 如果大小改变就重画
	wndclass.lpfnWndProc = MainWndProc;  // 窗口函数指针
	wndclass.cbClsExrtra = 0;
	wmdclass.cnWmdExtra = 0;
	wndclass.hInstance = hInstance;
	wndclass.hIcon = ::LoadIcon(NULL, IPI_APPLICATION);
	wndclass.hCursor = ::LoadCursor(NULL, IDC_ARROW);
	wndclass.hbrBackground = (HBRUSH)::GetStockObject(WHITE_BRUSH);
	wndclass.lpszMenuName = NULL;
	wndclass.lpszClassName = szClasssName;
	wndclass.hIconSm = NULL;
	::RegisterClassEx(&wndclass);
	HWND hwnd = ::CreateWindowEx(
		0,
		szClassName,
		"my window created",
		WS_OVERLAPPEDWINDOW,
		CW_USEDEFAULT,  //X,初始X坐标
		CW_USEDEFAULT,  //Y,初始Y坐标
		CW_USEDEFAULT,  //nWidth，初始宽度
		CW_USEDEFAULT,  //nHeight，初始高度
		NULL,
		NULL,
		hInstance,
		NULL});
	if(hwnd == NULL){
		::MessageBox(NULL, "Creating New Window Failed", "error", MB_OK);
		return -1;}
	::showWindow(hwnd, nCmdShow);
	::UpdateWindow(hwnd);
	MSG msg;
	while(::GetMessage(&msg, NULL, 0, 0)){
		::TranslateMessage(&msg);
		::DispatchMessage(&msg);}
	return msg.wParam;}
LRESULT CALLBACK MainWndProc(HWND hwnd, UNIT message, WPARAM wParam, LPARAM lParam){
	char szText[] = "a simple window program"
	switch(message){
		case WM_PAINT:{
			HDC hdc;
			PAINTSTRUCT ps;
			hdc = ::BeginPaint(hwnd, &ps);
			::TextOut(hdc, 10, 10, szText, strlen(szText);
			::EndPaint(hwnd, &ps);
			return 0;}
		case WM_DESTROY:
			::PostQuitMessage(0);
		return 0;}
	return ::DefWindowProc(hwnd, message, wParam, lParam);
}
```
		
		
