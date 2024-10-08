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

## 实例化类的两种方式
```c++
#include <iostream>
#include <string>

class Entity {
private:
	std::string m_name;
public:
	Entity()
		: m_name("UnKnown") {

	}
	Entity(const std::string& name)
		: m_name(name) {

	}
	const std::string& GetName() const {
		return m_name;
	}
};

int main() {
	Entity entity_0;
	Entity entity_1("Entity_1");

	Entity* entity_2 = new Entity();
	Entity* entity_3 = new Entity("Entity_3");

	std::cout << entity_0.GetName() << std::endl;
	std::cout << entity_1.GetName() << std::endl;
	std::cout << entity_2->GetName() << std::endl;
	std::cout << entity_3->GetName() << std::endl;

	delete entity_2;
	delete entity_3;

	// new -> delete
	// new [] -> delete[]

	return 0;78
}
```

## 简单日志类
```c++
#include <iostream>

class Log {
public:
	enum Level : short {
		LevelError, LevelWarning, LevelInfo
	};
private:
	Level m_LogLevel = LevelInfo;

public:
	void SetLevel(Level level) {
		m_LogLevel = level;
	}

	void Error(const char* message) {
		if (m_LogLevel >= LevelError)
			std::cout << "[Error]: " << message << std::endl;
	}

	void Warn(const char* message) {
		if (m_LogLevel >= LevelWarning)
			std::cout << "[Warning]: " << message << std::endl;
	}

	void Info(const char* message) {
		if (m_LogLevel >= LevelInfo)
			std::cout << "[Info]: " << message << std::endl;
	}
};

int main() {
	Log log;
	log.SetLevel(Log::LevelWarning);
	log.Error("hello!");
	log.Warn("hello!");
	log.Info("hello!");

	return 0;
}
```

## 构造函数，析构函数
```c+++
#include <iostream>

class Entity {
public:
	float x;
	float y;
	
	// 如果变量是类类型，如果不在初始化列表初始化，就会自动先调用其无参构造函数，导致冗余调用构造函数
	// 所以应该在初始化列表初始化变量
	Entity()
		: x(0.0f), y(0.0f) { // 初始化列表，顺序需要跟声明相同
		x = 0.0f;
		y = 0.0f;
	}

	Entity(float _x, float _y)
		: : x(_x), y(_y) { // 初始化列表，顺序需要跟声明相同
		//this->x = x;
		//this->y = y;
	}

	~Entity() {
		std::cout << "Destroy" << std::endl;
	}

	void Print() {
		std::cout << x << ", " << y << std::endl;
	}
};

class Log {
// 删除构造函数1，设为私有化
private:
	Log() {

	}
public:
	// 删除构造函数2，使用 delete
	Log() = delete;
	static void Print() {

	}1234567890-=0-98 
};

int main() {
	Entity e0;
	e0.Print();

	Entity e1(0, 1);
	e1.Print();

	return 0;
}
```

## 拷贝构造函数
### 示例1
```c++
#include <iostream>

class String {
private:
	char* m_buffer;
	unsigned int m_size;
public:
	String(const char* string) {
		m_size = strlen(string);
		m_buffer = new char[m_size + 1];
		memcpy(m_buffer, string, m_size);
		m_buffer[m_size] = '\0';
	}
	String(const String& other)
		: m_size(other.m_size) {
		std::cout << "String Copy!" << std::endl;
		m_buffer = new char[m_size + 1];
		memcpy(m_buffer, other.m_buffer, m_size);
		m_buffer[m_size] = '\0';
	}
	~String() {
		delete[] m_buffer;
	}
	char& operator[](unsigned int idx) {
		return m_buffer[idx];
	}

	friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string) {
	stream << string.m_buffer;
	return stream;
}

void PrintString(const String& string) {
	String copy = string; // copy 副本
	std::cout << string << std::endl;
}

int main() {
	String string0 = "hello, cpp.";
	String string1 = string0;
	string1[0] = 'a';
	PrintString(string0);
	PrintString(string1);

	return 0;
}
```

### 示例2
```c++
#include <iostream>
#include <string>

class Tmp {
public:
    Tmp() {
        std::cout << "construct" << std::endl;
    }
    Tmp(const Tmp& other) {
        std::cout << "copy construct" << std::endl;
    }
    ~Tmp() {
        std::cout << "destruct" << std::endl;
    }
};

Tmp Func(Tmp& tmp) {
    Tmp t;
    return t;
}

int main() {
    Tmp tmp;
    Tmp t_n = Func(tmp);

	return 0;
}

/*
g++ -fno-elide-constructors hello.cpp
construct Tmp tmp
construct Tmp t
copy construct return t
destruct Tmp t delete
copy construct Tmp t_n
destruct return t delete
destruct Tmp t_n
destruct Tmp tmp
*/
```

## 赋值拷贝函数
```c++
#include <iostream>
struct Tmp {
	int n;
	Tmp() : n(0) {
		std::cout << "Construct" << std::endl;
	}
	~Tmp() {
		std::cout << "Destruct" << std::endl;
	}
	Tmp(const Tmp &other) {
		n = other.n;
		std::cout << "Copy Construct" << std::endl;
	}
	Tmp& operator= (const Tmp& other) {
		n = other.n;
		std::cout << "= Construct" << std::endl;
		return *this;
	}
};

int main() {
	Tmp t1; // "Construct"
	Tmp t2 = t1; // "Copy Construct"
	Tmp t3; // "Construct"
	t3 = t1; // "= Construct"
    return 0;
}
```

## 继承
```c++
#include <iostream>

class Entity {
public:
	float x, y;
	void Move(float xa, float ya) {
		x += xa;
		y += ya;
	}
	void PrintPos() {
		std::cout << x << ", " << y << std::endl;
	}
};

class Player : public Entity {
public:
	const char* name;
	void PrintName() {
		std::cout << name << std::endl;
	}
};

int main() {
	Player player;
	player.name = "Player_0";
	player.x = 0;
	player.y = 0;
	player.Move(1, -1);
	player.PrintPos();
	player.PrintName();

	return 0;
}
```

## 虚函数
虚函数引入的额外开销：
	空间：虚函数表指针，虚函数表
	时间：遍历虚函数表
```c++
#include <iostream>
#include <string>

class Entity {
public:
	virtual std::string GetName() {
		return "Entity";
	}
};

class Player : public Entity {
private:
	std::string m_name;
public:
	Player(const std::string& name) 
		: m_name(name) {

	}
	std::string GetName() override {
		return m_name;
	}
};

void Func(Entity* entity) {
	std::cout << entity->GetName() << std::endl;
}

int main() {
	Entity* e = new Entity();
	Func(e);
	Player* p = new Player("Player_0");
	Func(p);

	return 0;
}
```

## 纯虚函数
```c++
#include <iostream>
#include <string>

class Printable {
public:
	virtual std::string GetClassName() = 0;
};

class Entity : public Printable {
public:
	std::string GetClassName() override {
		return "Entity";
	}
};

class Player : public Entity {
private:
	std::string m_name;
public:
	Player(const std::string& name) 
		: m_name(name) {

	}
	std::string GetClassName() override {
		return m_name;
	}
};

class Tmp : public Printable {
public:
	std::string GetClassName() override {
		return "Tmp";
	}
};

void Print(Printable* obj) {
	std::cout << obj->GetClassName() << std::endl;
}

int main() {
	Entity* e = new Entity();
	Print(e);
	Player* p = new Player("Player_0");
	Print(p);

	Tmp* tmp = new Tmp();
	Print(tmp);

	return 0;
}
```

## 虚构造函数
```c++
#include <iostream>

class Base {
public:
    Base() {
        std::cout << "Base Contructor" << std::endl;
    }
    virtual void Print() { // 虚函数，如果该函数需要在子类中重写
        std::cout << "Base Print" << std::endl;
    }
    virtual ~Base() { // 虚构造函数，如果该类会被继承，必须有虚构造函数
        std::cout << "Base Destructor" << std::endl;
    }
};

class Derived : public Base {
public:
    Derived() {
        std::cout << "Derived Contructor" << std::endl;
    }
    void Print() override {
        std::cout << "Derived Print" << std::endl;
    }
    ~Derived() {
        std::cout << "Derived Destructor" << std::endl;
    }
};

class Derived_n : public Derived {
public:
    Derived_n() {
        std::cout << "Derived_n Contructor" << std::endl;
    }
    void Print() override {
        std::cout << "Derived_n Print" << std::endl;
    }
    ~Derived_n() {
        std::cout << "Derived_n Destructor" << std::endl;
    }
};

int main(){
    Base* base = new Base();
    base->Print();
    delete base;

    std::cout << "----------------------" << std::endl;

    Base* derived = new Derived();
    derived->Print();
    delete derived;

    std::cout << "----------------------" << std::endl;

    Base* derived_n = new Derived_n();
    derived_n->Print();
    delete derived_n;
    
    return 0;
}
```

## 访问修饰符
private: 类内部
protected: 类内部，子类内部
public: 类内部，子类内部，类外部

## explicit
不允许隐式转换调用构造函数

## 友元函数
```c++
// A.h
#pragma once
#include "B.h"
#include <iostream>
class A
{
	friend void B::FuncB(A &a);
private:
	int val;
public:
	A() : val(0) {}
	inline void Print() {
		std::cout << val << std::endl;
	}
	void Print1();
};
```
```c++
// A.cpp
#include "A.h"

void A::Print1() {
	std::cout << val << std::endl;
}
```
```c++
// B.h
#pragma once

class A;
class B
{
public:
	void FuncB(A &a);
};
```
```c++
// B.cpp
#include "B.h"
#include "A.h"

void B::FuncB(A &a) {
	a.val += 1;
	
}
```

## 操作符，操作符重载
```c++
#include <iostream>

class Vector2 {
public:
	float x, y;
	Vector2(float _x, float _y)
		: x(_x), y(_y) {

	}
	Vector2 operator+(const Vector2& other) const {
		return Vector2(x + other.x, y + other.y);
	}
	Vector2 operator*(const Vector2& other) const {
		return Vector2(x * other.x, y * other.y);
	}
	bool operator==(const Vector2& other) const {
		return x == other.x and y == other.y;
	}
	bool operator!=(const Vector2& other) const {
		return ! (*this == other);
	}
};

std::ostream& operator<<(std::ostream& stream, const Vector2& other) {
	stream << other.x << ", " << other.y;
	return stream;
}

int main() {
	Vector2 position(-1.0f, 1.0f);
	Vector2 position1(-1.0f, 1.0f);
	Vector2 position2(1.0f, 1.0f);
	Vector2 speed(1.0f, 1.0f);
	Vector2 powerup(1.0f, 2.0f);

	Vector2 result = position + speed * powerup;
	
	std::cout << (position == position1) << std::endl;
	std::cout << (position != position2) << std::endl;

	return 0;
}
```

## 移动构造函数，移动赋值函数
```c++
#include <iostream>

class Point {
public:
	int* p_data;
	Point(int a) : p_data(new int(a)) {
		std::cout << "construct func" << std::endl;
	}
	Point(Point &&other) noexcept {
		if (this != &other) {
			std::cout << "move construct func" << std::endl;
			p_data = other.p_data;
			other.p_data = nullptr;
		}
	}
	Point& operator=(Point &&other) noexcept {
		if (this != &other) {
			std::cout << "move = func" << std::endl;
			delete p_data;
			p_data = other.p_data;
			other.p_data = nullptr;
		}
		return *this;
	}
};

int main() {
    Point p1(12); // 构造函数
    Point p2 = Point(15); // 构造函数，移动构造函数
	*p1.p_data = 100;
	*p2.p_data = 200;
	Point p3(20); // 构造函数
	p3 = Point(25); // 构造函数，移动赋值函数

	Point p4_t(12); // 构造函数
	Point p4 = std::move(p4_t); // 移动构造函数

	Point p5_t(12); // 构造函数
	Point p5(50); // 构造函数
	p5 = std::move(p5_t); // 移动赋值函数

    return 0;
}
```
g++ -fno-elide-constructors Main.cpp
