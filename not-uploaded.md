封装在一个类中

    class GameMemoFinder{
        public:
            GameMemoFinder(DWORD dwProcessId);
            virtual ~GameMemoFinder();
        // 属性
        publlic:
            Bool IsFirst() const{
                return m_bFirst;}
            Bool IsValid() const{
                return m_hProcess != NULL;}
            int GetListCount() const{
                return m_nListCnt;}
            DWORD operator[](int nIndex){
                return m_arList[nIndex];}
        // 操作        
            virtual BOOL FindFirst(DWORD dwValue);
            virtual BOOL FindNext(DWORD dwValue);
            virtual BOOL WriteMemory(DWORD dwAddr, DWORD dwValue);
        // 实现    
        protected:
            virtual BOOL ComparAPage(DWORD dwBaaseAddr. DWORD dwValue);
            DWORD m_arList[1024];
            int m_nListCnt;
            HANDLE m_hProcess;
            BOOL m_bFirst;
    };
***
### 多线程： 共享进程资源，如全局变量，句柄等
> 应用程序被装在到内存后就形成了进程，程序在内存中用线程执行代码。
一般主线程接受用户输入，显示结果，二辅助线程一般处理长时间的工作，如读写文件、访问网络等。这样有专门的线程响应用户指令。

创建辅助线程也需要制定进入点函数，被称为线程函数

    #define WINAPI __stdcall; // 凡是windows系统调用的函数都定义为————stdcall类型
    DWORD WINAPI ThreadName(LPCOID LpParam);
创建新进程的函数Create Thread，返回线程句柄

    #include<stdio.h>
    #include<windows.h>
    DWORD WINAPI SideThread1(LPVOID lpParam){
        //线程函数
        int i = 0;
        while(i < 20){
            printf("I an from a thread, count = %d\n", i++);}
        return 0;
    }
    int main(int argc, char* argv[]){
        HANDLE handleOfNewThread;
        DWORD dwThreadId;
        // 进程创建
        handleOfNewThread = ::CreateThread(
            NULL, NULL, SideThread1, NULL, 0, &dwThreadId);
        //主线程代码和新线程代码同时运行，一般下一行代码会先被执行，主线程若有多行则可能切换执行线程
        printf("Now another thread has been created.ID = %d \n", dwThreadId; 
        ::WaitForSingleObject(handleOfNewThread, INFINITE); // 等待这个新线程执行完成
        CloseHandle(handleOfNewThread);
        return 0;
    }
#### 线程内核对象
每调用CreateThread函数成功，系统就会创建一个线程内核对象。包含
* 线程上下文CONTEXT，即线程自己的一组CPU寄存器，寄存器的值保存在一个CONTEXT结构里
* 使用计数Usage Count, 线程运行时值为1，被被其他进程调用也会加1（CreateThread返回线程内核对象
的句柄，相当于打开一次新创建的内核对象），所以初始值为2。
* 暂停次数Suspend Count， 新线程值为1表示暂停， 需要初始化，然后值改为0表示可被调用，3表示被暂停3次，需要3次ResumeThread
