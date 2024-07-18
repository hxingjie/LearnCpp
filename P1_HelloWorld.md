#

## 编译和连接
```c++
#pragma once

void log(const char* message);
```

```c++
#include <iostream>
#include "log.h"

void log(const char* message) {
    std::cout << message << std::endl;
}
```

```c++
#include <iostream>
#include "log.h"

int main(){
    log("hello, world!");
    
    return 0;
}
```

```shell
g++ hello.cpp log.cpp
./a.out
```

## 变量
```c++
char, short, int, long, long long
unsigned int num;
float num = 3.14f;
double num = 3.14;
bool

int num = 10;
sizeof(int): 4
sizeof(num): 4 
```

## head file
如果不使用头文件
```c++
#include <iostream>
void log(const char* message) { // 实现
    std::cout << message << std::endl;
}
```
```c++
#include <iostream>
void log(const char* message); // 声明
int main(){
    log("hello, world.");
    return 0;
}
```

使用头文件
```c++
#pragma once

void logInit();
void log(const char* message);

/*
#ifndef _LOG_H
#define _LOG_H

void logInit();
void log(const char* message);

#endif
*/

// pragma once 和 ifndef define endif 都是防止该文件在同一个文件中被重复引用
```
```c++
#include <iostream>
#include "log.h"

void logInit() {
    std::cout << "Init Log" << std::endl;
}

void log(const char* message) {
    std::cout << message << std::endl;
}
```
```c++
#include <iostream>
#include "log.h"

int main(){
    logInit();
    log("hello, world.");
    
    return 0;
}
```
