#

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
