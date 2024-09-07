# Cap 13

```c++
#pragma once

#include <set>
class Message;
class Folder {
private:
	std::set<Message*> messages;
public:
	void addMsg(Message* msg);
	void remMsg(Message* msg);
};
```
```c++
#include "Folder.h"

void Folder::addMsg(Message* msg) {
	messages.insert(msg);
}

void Folder::remMsg(Message* msg) {
	messages.erase(msg);
}
```

```c++
#pragma once

#include <string>
#include <set>

class Folder;
class Message {
    friend class Folder;
    friend void swap(Message& lhs, Message& rhs);
private:
    std::string contents;
    std::set<Folder*> folders;
    void add_to_Folders(const Message&);
    void remove_from_Folders();
public:
    Message(const std::string& str = std::string(""))
        : contents(str), folders(std::set<Folder*>()) {}
    Message(const Message&); // 拷贝构造函数
    Message& operator=(const Message&); // 拷贝赋值运算符
    ~Message();

    void save(Folder&); // 从给定的Folder集合中添加该message
    void remove(Folder&); // 从给定的Folder集合中删除该message
};
```
```c++
#include "Message.h"
#include "Folder.h"

void Message::add_to_Folders(const Message &msg) {
    // 拷贝函数的辅助函数
    // msg是被拷贝的原对象
    // this指向的对象是拷贝出来的新副本
    for (std::set<Folder*>::iterator it = msg.folders.begin(); it != msg.folders.end(); ++it) {
        (*it)->addMsg(this);
    }
}

void Message::remove_from_Folders() {
    // 将自己所在的folders中删除
    for (std::set<Folder*>::iterator it = folders.begin(); it != folders.end(); ++it) {
        (*it)->remMsg(this);
    }
}

Message::Message(const Message &other)
    : contents(other.contents), folders(other.folders) { // 拷贝构造函数
    add_to_Folders(other);
}

Message& Message::operator=(const Message &other) { // 拷贝赋值运算符
    // 需要考虑自赋值
    contents = other.contents;
    folders = other.folders;
    remove_from_Folders(); // 先删除
    add_to_Folders(other); // 再添加，处理自赋值
    return *this;
}

void swap(Message &lhs, Message &rhs) {
    using std::swap;
    for (Folder* p : lhs.folders) {
        p->remMsg(&lhs);
    }
    for (Folder* p : rhs.folders) {
        p->remMsg(&rhs);
    }

    swap(lhs.contents, rhs.contents);
    swap(lhs.folders, rhs.folders);

    for (Folder* p : lhs.folders) {
        p->addMsg(&lhs);
    }
    for (Folder* p : rhs.folders) {
        p->addMsg(&rhs);
    }
}

Message::~Message() {
    remove_from_Folders();
}

void Message::save(Folder &folder) { // 从给定的Folder集合中添加该message
    folders.insert(&folder); // 更新自己的folders
    folder.addMsg(this); // 在对应folder中添加msg指针
}

void Message::remove(Folder &folder) { // 从给定的Folder集合中删除该message
    folders.erase(&folder); // 更新自己的folders
    folder.remMsg(this); // 在对应folder中删除msg指针
}
```

