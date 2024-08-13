#

## Sales_data
```c++
// Sales_data.h
#ifndef SALES_DATA_H
#define SALES_DATA_H

#include <iostream>
#include <string>
class Sales_data {
	friend Sales_data Add(const Sales_data&, const Sales_data&);
	friend std::ostream& Print(std::ostream&, const Sales_data&);
	friend std::istream& Read(std::istream&, Sales_data&);
private:
	std::string bookID; // id
	unsigned int unitsSold; // 售出数量
	double revenue; // 收入
	double AvgPrice() const;
public:
	Sales_data();
	Sales_data(std::istream &is);
	Sales_data(const std::string &s);
	Sales_data(const std::string &s, unsigned int n, double p);

	std::string Isbn() const;
	Sales_data& Combine(const Sales_data&);
};

Sales_data Add(const Sales_data&, const Sales_data&);
std::ostream& Print(std::ostream&, const Sales_data&);
std::istream& Read(std::istream&, Sales_data&);

#endif
```
```c++
// Sales_data.cpp
#include "Sales_data.h"

Sales_data::Sales_data()
	: bookID(std::string()), unitsSold(0), revenue(0.0) {

}
Sales_data::Sales_data(std::istream& is)
	: bookID(std::string()), unitsSold(0), revenue(0.0) {
	Read(is, *this);
}
Sales_data::Sales_data(const std::string& s)
	: bookID(s), unitsSold(0), revenue(0.0) {

}
Sales_data::Sales_data(const std::string& s, unsigned int n, double p)
	: bookID(s), unitsSold(n), revenue(n*p) {

}
std::string Sales_data::Isbn() const {
	return bookID;
}
Sales_data& Sales_data::Combine(const Sales_data& rhs) {
	unitsSold += rhs.unitsSold;
	revenue += rhs.revenue;
	return *this;
}
double Sales_data::AvgPrice() const {
	if (unitsSold) {
		return revenue / unitsSold;
	} else {
		return 0;
	}
}

Sales_data Add(const Sales_data &lhs, const Sales_data &rhs) {
	Sales_data sum = lhs;
	sum.Combine(rhs);
	return sum;
}
std::ostream& Print(std::ostream &os, const Sales_data &item) {
	os << item.Isbn() << " " << item.unitsSold << " "
	   << item.revenue << " " << item.AvgPrice();
	return os;
}
std::istream& Read(std::istream &is, Sales_data &item) {
	double price = 0.0;
	is >> item.bookID >> item.unitsSold >> price;
	item.revenue = item.unitsSold * price;
	return is;
}
```

