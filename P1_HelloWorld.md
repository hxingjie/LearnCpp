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

## 指针
```c++
#include <iostream>
#define Print(x) std::cout << x << std::endl

int main() {
	int var = 8;
	int* ptr = &var;
	Print(var);

	*ptr = 10;
	Print(var);

    short* buffer = new short[8];
	memset(buffer, 255, sizeof(short)*8); // memset 是操作字节的
	short** ptr_buffer = &buffer;
	delete[] buffer;

	return 0;
}
```

## Enum
```c++
#include <iostream>

enum MyEnum : short {
	var_a, var_b, var_c
};

int main() {
	MyEnum e = var_a;
	if (e == var_a) {
		std::cout << "It's var_a!" << std::endl;
	}
	return 0;
}
```

## Array

### stack 创建数组
```c++
#include <iostream>
int main() {
	int nums[4];
	//const int size = 4;
	//int nums[size];
	for (int i = 0; i < 4; i++) {
		nums[i] = 0;
	}

	int* ptr = nums;
	nums[2] = 2;
	*(ptr + 2) += 1;
	*(int*)((char*)ptr + 8) += 1;

	return 0;
}
```

### heap 创建数组
```c++
#include <iostream>
int main() {
	const int size = 4;
	int* nums = new int[size];
	for (int i = 0; i < size; i++) {
		nums[i] = 0;
	}
	delete[] nums;

	return 0;
}
```

