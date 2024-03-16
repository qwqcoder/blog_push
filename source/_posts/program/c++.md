---
title: c++ 语法学习
date: 
tags: 
	- c++
description: 简单记录下c++的语法使用
cover: 
top_img: /img/cover/wyy2.png
---

# 重载

## 一般重载

在C++中可以通过**重载**运算符来改变已有运算符的行为，使其适用于自定义类型。以下是一个简单的例子，演示如何重载加法运算符（+）来实现两个自定义对象的相加：

```cpp
#include <iostream>

class MyNumber {
private:
    int num;

public:
    MyNumber(int n) : num(n) {}

    // 重载加法运算符
    MyNumber operator+(const MyNumber& other) {
        return MyNumber(this->num + other.num);
    }

    // 获取数字的方法
    int getNum() const {
        return num;
    }
};

int main() {
    MyNumber num1(5);
    MyNumber num2(10);

    // 使用重载的加法运算符
    MyNumber sum = num1 + num2;

    std::cout << "Sum: " << sum.getNum() << std::endl;

    return 0;
}
```

在这个例子中，`MyNumber` 类重载了加法运算符 `+`。在 `main()` 函数中，我们创建了两个 `MyNumber` 对象 `num1` 和 `num2`，然后使用重载的加法运算符将它们相加，并将结果赋给 `sum`。最后，我们打印出 `sum` 对象中存储的值。

减法运算符 `-`、乘法运算符 `*` 等类似，**自增运算符比较特殊**。重载运算符时需要考虑类型的合理性和操作的语义，以确保代码的可读性和正确性。

## 自增运算符

C++ 中的自增运算符 `++` 的重载有一些特殊之处。在 C++ 中，自增运算符可以以前缀形式（`++var`）或后缀形式（`var++`）使用，并且可以重载这两种形式。

> 重载前缀形式的自增运算符时，通常返回递增后的对象的引用（或指针）。而重载后缀形式时，则要返回递增前的对象的副本，通常通过一个额外的参数来区分前缀和后缀形式的调用。

以下是一个简单的示例，演示了如何重载前缀和后缀形式的自增运算符：

```cpp
#include <iostream>

class MyNumber {
private:
    int num;

public:
    MyNumber(int n) : num(n) {}

    // 重载前缀形式的自增运算符
    MyNumber& operator++() {
        ++num;
        return *this;
    }

    // 重载后缀形式的自增运算符
    MyNumber operator++(int) {
        MyNumber temp = *this;
        ++num;
        return temp;
    }

    // 获取数字的方法
    int getNum() const {
        return num;
    }
};

int main() {
    MyNumber num(5);

    // 使用前缀形式的自增运算符
    ++num;
    std::cout << "After prefix increment: " << num.getNum() << std::endl;

    // 使用后缀形式的自增运算符
    MyNumber num2 = num++;
    std::cout << "After postfix increment: " << num2.getNum() << std::endl;

    return 0;
}
```

`MyNumber` 类重载了前缀和后缀形式的自增运算符 `++`。在 `main()` 函数中，我们分别使用了前缀和后缀形式的自增运算符，并打印了结果。