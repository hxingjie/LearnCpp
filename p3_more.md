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

```

### unique_ptr
```c++

```

### unique_ptr
```c++

```

## 
