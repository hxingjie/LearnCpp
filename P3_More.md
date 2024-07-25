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
