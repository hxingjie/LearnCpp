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

g++ -std=c++17 hello.cpp
```

## 变量
```c++
typedef long long ll;
#define LOG(x) std::cout << x << std::endl

char, short, int, long, long long
unsigned int num;
float num = 3.14f;
double num = 3.14;
bool

int num = 10;
sizeof(int): 4
sizeof(num): 4 
```

变量应该显式初始化

声明与定义分开
```c++
// tmp.h
extern int val;
void func();

// tmp.cpp
#include <iostream>
#include "tmp.h"

int val = 10;
void func() {
    std::cout << val << std::endl;
}

// Main.cpp
#include <iostream>
#include "tmp.h"

extern int val;

int main() {
    func();
    std::cout << val << std::endl;
    val = 15;
    std::cout << val << std::endl;

    return 0;
}
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

## 指针，内存
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

	int n = 4;
	int* nums = new int[n] {0, 1, 2, 3};
	int* nums_c = new int[n];
	memcpy(nums_c, nums, n*sizeof(int));
	delete[] nums;
	delete[] nums_c;
 
	return 0;
}
```
```c++
#include <iostream>

struct Entity {
	int x, y;
};

int main() {
	int numi = 6;
	float numf = * (float*) & numi; // 拷贝内存
	// 0100 0000 1100 0000 0000 0000 0000 0000
	// 1.10.0 0000
	std::cout << numf << std::endl;
	
	// 内存访问
	Entity entity;
	int* ptr = (int*) &entity;
	ptr[0] = 6;
	ptr[1] = 12;
	std::cout << entity.x << ", " << entity.y << std::endl;

	char* creazyPtr = (char*) &entity;
	std::cout << *(int*)(creazyPtr + 4) << std::endl;

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

	const int size = 4;
	int nums[size];

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
	int size = 4;
	int* nums = new int[size];

	for (int i = 0; i < size; i++) {
		nums[i] = 0;
	}
	delete[] nums;

	return 0;
}
```

## string

### 声明
```c++
#include <iostream>

int main() {
	std::string str("hello,world");
	std::string* hstr = new std::string("hello,world");

	const char* cstr = "hello,world.";

	char* carray = new char[13] { 'h', 'e', 'l', 'l', 'o', ',', 'w', 'o', 'r', 'l', 'd', '.'};
	char carray[13] { 'h', 'e', 'l', 'l', 'o', ',', 'w', 'o', 'r', 'l', 'd', '.' };

	return 0;
}
```

### c string
```c++
#include <iostream>
int main() {
	// "hello, world." is const char array

	// const char*
	const char* c_str = "hello, world.";

	int size = strlen(c_str); // 不包括 '\0'

	char c_array[14];
	strcpy(c_array, c_str);

	// char c_array[]
	char c_array[14] = { 'h', 'e', 'l', 'l', 'o', ',', ' ', 'w', 'o', 'r', 'l', 'd', '.', '\0' };
	char c_array[14] = "hello, world.";
	c_array[0] = 'a';

	// other type
	const char* c_str_8 = u8"hello, world.";
	const wchar_t* c_str_w = L"hello, world.";
	const char16_t* c_str_16 = u"hello, world.";
	const char32_t* c_str_32 = U"hello, world.";

	// more lines
	const char* str = "hello\n"
		"world\n"
		"hello\n"
		"cpp\n";

	return 0;
}
```

### c++ string
```c++
#include <iostream>
#include <string>

void PrintStr(const std::string& str) {
	std::cout << str << std::endl;
}

int main() {
	std::string str = "hello, world.";
	std::cout << str << std::endl;
	PrintStr(str);

	// some api
	int size = str.size();
	bool b = str.find("or") != std::string::npos;
	str.push_back('a');
	str.pop_back();

	// 拼接字符串
	str += "hello, cpp."; // += 被重载了
	// 不能 "hello, world." + "hello, cpp."
	// 因为 "hello, world." 和 "hello, cpp." 都是 const char array

	// more lines
	std::string str = "hello\n"
		"world\n"
		"hello\n"
		"cpp\n";

	return 0;
}
```

## string_view
```c++
#include <iostream>
#include <string>

void PrintStr(std::string_view str) {
    std::cout << str << std::endl;
}

int main() {
    // std::string_view: 存储 const char *，和长度，不需要分配内存

    const char* str = "hello, world.";
    std::string_view firstStr(str, 5);
    std::string_view secondStr(str+7, 5);

    PrintStr(firstStr);
    PrintStr(secondStr);

	return 0;
}
```

## 类外的 static
```c++
// static.cpp
// like use private
static int var = 5; // 只属于该.obj文件
static void TestFunc() { // 只属于该.obj文件

}
```

```c++
// static.cpp
int var = 5;
```
```c++
// main.cpp
#include <iostream>
extern int var; // 找到 static.cpp 中的变量
int main() {
	std::cout << var << std::endl;
	return 0;
}
```

## 类内的 static
```c++
#include <iostream>
class Entity {
public:
	static int x, y; // 表示该变量属于类，而不属于实例
	static void Print() { // 表示该方法属于类，而不属于实例
		// static 方法不包含 this 指针，所以不能访问到非静态变量
		std::cout << x << ", " << y << std::endl;
	}
};
int Entity::x; // 必须声明或赋值
int Entity::y = 20;
int main() {
	Entity::x = 10;
	//Entity::y = 20;
	Entity::Print();
	
	return 0;
}
```

## Local Static Var
```c++
#include <iostream>

void Func() {
	static int s_var = 0; // 表示该遍历的生命周期是整个程序的生命周期，作用范围是所在作用域
	std::cout << s_var << std::endl;
	s_var += 1;
}

int main() {
	Func();// 0
	Func();// 1
	Func();// 2
	Func();// 3
	
	return 0;
}
```

### 单例模式 use static
```c++
#include <iostream>

class Singleton {
private:
    float numf = 3.14f;
    Singleton() { // 构造函数私有化，不允许外部实例化该类 
        std::cout << "Construst" << std::endl;
    }
public:
    Singleton(const Singleton& other) = delete;
    static Singleton& GetInstance() {
        static Singleton instance;
        return instance;
    }
    void Func() {
        std::cout << "Hello, World." << std::endl;
        std::cout << "numf: " << numf << std::endl;
    }
};

int main() {
    Singleton& instance = Singleton::GetInstance();
    instance.Func();

	return 0;
}
```

### 单例模式 no static
```c++
#include <iostream>

class Singleton {
private:
	static Singleton* s_Instance;
public:
	static Singleton& Get() {
		return *s_Instance;
	}
	void Hello() {
		std::cout << "hello, world." << std::endl;
	}
};
Singleton* Singleton::s_Instance = nullptr;

int main() {
	Singleton::Get().Hello();
	return 0;
}
```

## const
```c++
// 1.定义常量
const float PI = 3.14f;

// 2.约束指针
const int* ptr = new int; // 不能修改指针指向的内存的值
int* const ptr = new int; // 不能修改指针的指向
const int* const ptr = new int;

// 3.in class
class Entity {
private:
	int m_x, m_y;
	mutable int var; // 允许在const函数中修改该变量
public:
	int GetX() const { // 约束该函数只读
		var = 0;
		return m_x;
	}
	void SetX(int x) {
		m_x = x;
	}
};

void PrintFunc(const Entity& entity) {
	// 声明了const，就不能调用非const函数
	// entity.SetX(1);
}
```
