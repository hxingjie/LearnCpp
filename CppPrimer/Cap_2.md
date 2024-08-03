# Cap 2

##
Sales_data.h
```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H

#include <string>
class Sales_data {
public:
	std::string bookID;
	unsigned int unitsSold;
	double revenue;
	Sales_data()
		: bookID(""), unitsSold(0), revenue(0.0) {

	}
};
#endif
```

Sales_data.cpp
```c++

```

Main.cpp
```c++
#include <iostream>

#include "Sales_data.h"

int main() {
    Sales_data data1;
    Sales_data data2;
    
    double tPrice = 0.0;
    std::cin >> data1.bookID >> data1.unitsSold >> tPrice;
    data1.revenue = tPrice * data1.unitsSold;

    std::cin >> data2.bookID >> data2.unitsSold >> tPrice;
    data2.revenue = tPrice * data2.unitsSold;

    if (data1.bookID == data2.bookID) {
        unsigned int unitSum = data1.unitsSold + data2.unitsSold;
        double revenueSum = data1.revenue + data2.revenue;
        std::cout << "Unit Sum: " << unitSum << ", Revenue Sum: " << revenueSum << std::endl;
        if (unitSum > 0)
            std::cout << "Average Price: " << revenueSum / unitSum << std::endl;
        else
            std::cout << "Unit Sum == 0" << std::endl;
    } else {
        std::cout << "Data must refer to the same ISBN" << std::endl;
    }

    return 0;
}
```
