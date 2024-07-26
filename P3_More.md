#

## 43.范围指针，自动 delete
```c++
#include <iostream>

class Entity {
public:
	Entity() {
		std::cout << "Created Entity" << std::endl;
	}
	~Entity() {
		std::cout << "Destrroyed Entity" << std::endl;
	}
};

class ScopePtr {
private:
	Entity* m_ptr;
public:
	ScopePtr(Entity* p_e)
		: m_ptr(p_e){

	}
	~ScopePtr() {
		delete m_ptr;
	}
};
	

int main() {
	{
		ScopePtr sp(new Entity());
	}

	return 0;
}
```

## 智能指针

### unique_ptr
作用域指针，不可复制，唯一
```c++
#include <iostream>
#include <string>
#include <memory>

class Entity {
public:
	Entity() {
		std::cout << "Created Entity" << std::endl;
	}
	Entity(int val) {
		std::cout << "Created Entity" << val << std::endl;
	}
	~Entity() {
		std::cout << "Destrroyed Entity" << std::endl;
	}
	void Print() {

	}
};

int main() {
	{
		std::unique_ptr<Entity> entity0 = std::make_unique<Entity>();
		entity0->Print();

		std::unique_ptr<Entity> entity1 = std::make_unique<Entity>(10);
		entity1->Print();
	}
	return 0;
}
```

### shared_ptr
```c++
#include <iostream>
#include <string>
#include <memory>

class Entity {
public:
	Entity() {
		std::cout << "Created Entity" << std::endl;
	}
	Entity(int val) {
		std::cout << "Created Entity, " << val << std::endl;
	}
	~Entity() {
		std::cout << "Destrroyed Entity" << std::endl;
	}
	void Print() {

	}
};

int main() {
	{
		std::shared_ptr<Entity> entitya_0;
		std::shared_ptr<Entity> entityb_0;
		{
			std::shared_ptr<Entity> entitya_1 = std::make_shared<Entity>();
			entitya_0 = entitya_1;
			entitya_0->Print();

			std::shared_ptr<Entity> entityb_1 = std::make_shared<Entity>(10);
			entityb_0 = entityb_1;
			entityb_0->Print();
		}
	}
	
	return 0;
}
```

### weak_ptr
```c++
#include <iostream>
#include <string>
#include <memory>

class Entity {
public:
	Entity() {
		std::cout << "Created Entity" << std::endl;
	}
	~Entity() {
		std::cout << "Destrroyed Entity" << std::endl;
	}
	void Print() {

	}
};

int main() {
	{
		std::weak_ptr<Entity> entitya_0;
		std::shared_ptr<Entity> entityb_0;
		{
			std::shared_ptr<Entity> entitya_1 = std::make_shared<Entity>();
			entitya_0 = entitya_1;
			//entitya_0->Print(); // error
			entitya_1->Print();

			std::cout << entitya_0.use_count() << std::endl; // 检查 entitya_1 指向的空间有多少个 shared_ptr 指向
			std::cout << entitya_0.expired() << std::endl; // 检查 entitya_1 指向的空间是否销毁
		}
		std::cout << entitya_0.expired() << std::endl;
	}
	
	return 0;
}
```

## 46. -> 运算符

### 1.
```c++
int main() {
	Entity entity;
	Entity* ptr = &entity;
	ptr->Print();
	(*ptr).Print();
	
	return 0;
}
```

### 2.重载->运算符
```c++
#include <iostream>

class Entity {
public:
	int x;
	void Print() const {
		std::cout << "hello" << std::endl;
	}
};

class ScopedPtr {
private:
	Entity* m_ptr;
public:
	ScopedPtr(Entity* entity)
		: m_ptr(entity) {

	}
	~ScopedPtr() {
		delete m_ptr;
	}
	Entity* operator->() {
		return m_ptr;
	}
};

int main() {
	ScopedPtr ptr(new Entity());
	ptr->Print();
	
	return 0;
}
```

### 3.查看内存布局
```c++
#include <iostream>

class Entity {
public:
	int x, y, z;
};

int main() {
	std::cout << (int) & ((Entity*)nullptr)->y << std::endl; // 0
	std::cout << (int) & ((Entity*)nullptr)->y << std::endl; // 4
	std::cout << (int) & ((Entity*)nullptr)->y << std::endl; // 8
	return 0;
}
```

## 47.Vector

### 1.
```c++
#include <iostream>
#include <vector>

class Vertex {
public:
	int x, y, z;
	Vertex() {

	}
	Vertex(int a, int b, int c)
		: x(a), y(b), z(c) {

	}
};

std::ostream& operator<<(std::ostream& stream, const Vertex& vertex) {
	stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
	return stream;
}

void Func(const std::vector<Vertex>& vertices) {

}

int main() {
	std::vector<Vertex> vertices;
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));

	for (int i = 0; i < vertices.size(); i++)
		std::cout << vertices[i] << std::endl;

	vertices.erase(vertices.begin() + 1);

	for (Vertex& v : vertices)
		std::cout << v << std::endl;

	vertices.clear();
	
	return 0;
}
```

### 2.
```c++
#include <iostream>
#include <vector>

class Vertex {
public:
	int x, y, z;
	Vertex() {

	}
	Vertex(int a, int b, int c)
		: x(a), y(b), z(c) {

	}
	Vertex(const Vertex& vertex)
		: x(vertex.x), y(vertex.y), z(vertex.z) {
		std::cout << "Copied" << std::endl;
	}
};

std::ostream& operator<<(std::ostream& stream, const Vertex& vertex) {
	stream << vertex.x << ", " << vertex.y << ", " << vertex.z;
	return stream;
}

void Func(const std::vector<Vertex>& vertices) {

}

int main() {
	std::vector<Vertex> vertices;
	vertices.reserve(3); // 提前扩容
	vertices.emplace_back(1, 2, 3); // 不需要调用拷贝构造函数
	vertices.emplace_back(4, 5, 6);
	vertices.emplace_back(7, 8, 9);
	
	return 0;
}
```

## 53.Template

### 1.function
```c++
template<typename T>
void Print(T value) {
	std::cout << value << std::endl;
}
```

### 2.class
```c++
#include <iostream>
#include <vector>

template<typename T, int N>
class Array {
private:
	T m_array[N];
public:
	int GetSize() const {
		return N;
	}
};

int main() {
	const int n = 6;
	Array<int, n> array;
	std::cout << array.GetSize() << std::endl;
	
	return 0;
}
```

## 55.宏
```c++
#include <iostream>

#define PR_DEBUG 1

#if PR_DEBUG == 1
#define LOG(x) std::cout << x << std::endl
#else
#define LOG(x)
#endif

int main() {
	LOG("Hello");

	return 0;
}
```

## 58.函数指针，lambda
```c++
#include <iostream>
#include <vector>
#include <functional>

void PrintNums(const std::vector<int>& values, const std::function<float(int)>& func) {
	for (int val : values) {
		 float f = func(val);
		 std::cout << f << std::endl;
	}
}

float Print(int val) {
	std::cout << val << std::endl;
	return 3.14f;
}

int main() {
	std::vector<int> values{ 1, 5, 4, 2, 3 };
	int x = 10;
	PrintNums(values, [&x](int val) -> float{ // x, &x, =, &
		std::cout << val << ", " << x << std::endl;
		return 3.14f;
	});

	PrintNums(values, Print);

	return 0;
}
```
```c++
#include <iostream>
#include <vector>

int main() {
	std::vector<int> values{ 1, 5, 4, 2, 3 };
	std::vector<int>::iterator it = std::find_if(values.begin(), values.end(),
		[](int val) -> bool {
			return val > 3;
		});
	std::cout << *it << std::endl;

	return 0;
}
```

### 61.namespace
```c++
#include <iostream>
#include <string>

namespace apple {
	void Print() {
		std::cout << "Apple" << std::endl;
	}
}

namespace orange {
	void Print() {
		std::cout << "Orange" << std::endl;
	}
}

int main() {
	apple::Print();
	orange::Print();

	return 0;
}
```

## 62.thread
```c++
#include <iostream>
#include <thread>

static bool s_finished = false;

void DoWork() {
	using namespace std::literals::chrono_literals;

	std::cout << "Start thread DoWork, id=" << std::this_thread::get_id() << std::endl;

	while (!s_finished) {
		std::cout << "Working..." << std::endl;
		std::this_thread::sleep_for(1s); // using namespace std::literals::chrono_literals;
	}

}

int main() {
	std::cout << "Start thread id=" << std::this_thread::get_id() << std::endl;

	std::thread worker(DoWork);

	std::cin.get();
	s_finished = true;

	worker.join();
	std::cout << "Finished" << std::endl;

	std::cin.get();

	return 0;
}
```

## 63.计时
```c++
#include <iostream>
#include <thread>
#include <chrono>

class Timer {
public:
	std::chrono::time_point<std::chrono::steady_clock> start, end;
	std::chrono::duration<float> duration;
	Timer() {
		start = std::chrono::high_resolution_clock::now();
	}
	~Timer() {
		end = std::chrono::high_resolution_clock::now();
		duration = end - start;

		float ms = duration.count() * 1000.0f;
		std::cout << "Duration=" << ms << "ms" << std::endl;
	}
};

void TestFunc() {
	Timer timer;

	for (int i = 0; i < 100; i++) {
		std::cout << "hello, cpp." << std::endl;
	}
}

int main() {
	TestFunc();
	return 0;
}
```

## 64.二维数组
```c++
#include <iostream>

int main() {
	int** array2d = new int*[50];
	for (int i = 0; i < 50; i++) {
		array2d[i] = new int[50];
	}
	for (int i = 0; i < 50; i++) {
		delete[] array2d[i];
	}
	delete[] array2d;

	int*** array3d = new int**[50];
	for (int i = 0; i < 50; i++) {
		array3d[i] = new int*[50];
		for (int j = 0; j < 50; j++) {
			array3d[i][j] = new int[50];
		}
	}
	for (int i = 0; i < 50; i++) {
		for (int j = 0; j < 50; j++) {
			delete[] array3d[i][j];
		}
		delete[] array3d[i];
	}
	delete[] array3d;

	return 0;
}
```

## 65.排序
```c++
#include <iostream>
#include <algorithm>
#include <vector>

bool cmp(const int& lhs, const int& rhs) {
	return lhs < rhs;
}

int main() {
	std::vector<int> nums = { 3, 5, 1, 4, 1 , 6};

	// func
	sort(nums.begin(), nums.end(), cmp);

	// lambda
	sort(nums.begin(), nums.end(), 
		[](const int& lhs, const int& rhs) {
			if (lhs == 1)
				return false;
			if (rhs == 1)
				return true;
			return lhs < rhs;
		});

	for (int num : nums)
		std::cout << num << std::endl;

	return 0;
}
```
