#

## 类的基础概念
```c++
#include <iostream>

#define Print(x) std::cout << x << std::endl

class Player {
public:
	int x, y;
	int speed;

	void Move(int xa, int ya) {
		x += xa * speed;
		y += ya * speed;
	}
};

int main() {
	Player player;
	player.x = 0;
	player.y = 0;
	player.speed = 2;
	player.Move(1, -1);

	Print(player.x);
	Print(player.y);

	return 0;
}
```

## 简单日志类
```c++
#include <iostream>

class Log {
public:
	const int LogLevelError = 0;
	const int LogLevelWarning = 1;
	const int LogLevelInfo = 2;
private:
	int m_LogLevel = LogLevelInfo;

public:
	void SetLevel(int level) {
		m_LogLevel = level;
	}

	void Error(const char* message) {
		if (m_LogLevel >= LogLevelError)
			std::cout << "[Error]: " << message << std::endl;
	}

	void Warn(const char* message) {
		if (m_LogLevel >= LogLevelWarning)
			std::cout << "[Warning]: " << message << std::endl;
	}

	void Info(const char* message) {
		if (m_LogLevel >= LogLevelInfo)
			std::cout << "[Info]: " << message << std::endl;
	}
};

int main() {
	Log log;
	log.SetLevel(log.LogLevelWarning);
	log.Error("hello!");
	log.Warn("hello!");
	log.Info("hello!");

	return 0;
}
```

## 类外的 static
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

```c++
// static.cpp
// like use private
static int var = 5; // 只属于该.obj文件

static void TestFunc() { // 只属于该.obj文件

}
```

## 类内的 static
```c++
#include <iostream>
class Entity {
public:
	static int x, y;
	static void Print() {
		// static 方法不包含 this 指针，所以不能访问到非静态变量
		std::cout << x << ", " << y << std::endl;
	}
};
int Entity::x;
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
	static int s_var = 0; // 生命周期是整个程序的生命周期，作用范围是所在作用域
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
public:
	static Singleton& Get() {
		static Singleton instance;
		return instance;
	}
	void Hello() {
		std::cout << "hello, world." << std::endl;
	}
};

int main() {
	Singleton::Get().Hello();
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

