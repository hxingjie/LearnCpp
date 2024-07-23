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


