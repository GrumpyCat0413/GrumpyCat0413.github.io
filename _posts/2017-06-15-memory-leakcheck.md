---
layout: post
title: VS2013 内存泄漏检查
---
http://baitai.iteye.com/blog/1020355
```
#define _CRTDBG_MAP_ALLOC

#include <stdlib.h>
#include <crtdgb.h>


void EnableMemleakCheck()
{
  int tmpFlag = _CrtSetDbgFlag(_CRTDGB_PEPORT_FLAG);
  tmpFlag |= _CRTDBG_LEAK_CHECK_DF;
  _CrtSetDbgFlag(tmpFlag);
}
//或者
//_CrtSetDbgFlag(_CRTDBG_LEAK_CHECK_DF | _CRTDBG_ALLOC_MEM_DF);     
// 设置CRT库中的内存泄露检测标记 
int main()
{
  EnableMemLeakCheck();
  //_CrtSetBreakAlloc(3558059);
  
  
  return 0;
}

```
```
编译执行，如果代码中存在内存泄漏，则在程序终止时会输出端口看到以下内容
线程 0x1b24 已退出，返回值为 0 (0x0)。  
Detected memory leaks!  
Dumping objects ->  
{3558059} normal block at 0x0000000007F11480, 62544 bytes long.  
 Data: <        y W]    > CC AD FF AD AA 1B A2 BE 79 9E 57 5D E2 8E FE BE   
{3558049} normal block at 0x0000000007CB98F0, 88 bytes long.  
 Data: <                > 0C 00 00 00 CD CD CD CD CD CD CD CD CD CD CD CD   
{3558025} normal block at 0x0000000007EF1A90, 62544 bytes long.  
 Data: <  FR   >   .u  ?> E2 CB 46 52 DD 11 E1 3E CB C9 93 2E 75 BE 00 3F   
{3558015} normal block at 0x0000000007CBA170, 88 bytes long.  
 Data: <                > 0C 00 00 00 CD CD CD CD CD CD CD CD CD CD CD CD
 
 log中3558059的地方存在内存泄漏，然后将_CrtSetBreakAlloc(3558059)注释取消，
 然后重新编译执行程序，代码会run到内存泄漏的地方就停下来
```

### 例子
```
#define _CRTDBG_MAP_ALLOC
#include "stdlib.h"
#include "crtdbg.h"
 
#include <iostream>
using namespace std;

int* getInt() {
	int* p = (int*)malloc(sizeof(int));
	*p = 100;
	return p;
}
 
char* GetMemory(char *p, int num)
{
    p = (char*)malloc(sizeof(char) * num);
	return p;
}
 
int main(int argc,char** argv)
{
    char *str = NULL;
    char *p = GetMemory(str, 100);
	free(p);
    cout<<"Memory leak test!"<<endl;

	int* pInt = getInt();
	cout << *pInt << endl;
	free(pInt);

    _CrtDumpMemoryLeaks();
    return 0;
}
```
