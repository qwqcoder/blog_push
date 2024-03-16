---
title: c++ 语法学习
tags:
  - c++
description: 简单记录下c++的语法使用
top_img: /img/cover/wyy2.png
abbrlink: ac3a716d
date:
cover:
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

# 构造函数

## 初始化列表

初始化列表是一种用于在对象构造过程中初始化成员变量的特性。它**提供了一种更加有效和灵活的初始化方式，特别是对于类中的成员变量或者继承关系中的基类构造函数。**

> 在传统的构造函数中，成员变量的初始化通常是在构造函数的函数体中进行的。而使用初始化列表，则可以在构造函数的初始化列表中直接初始化成员变量，而不需要在构造函数的函数体中再次赋值。

初始化列表的语法是在构造函数的参数列表后面使用冒号，并列出成员变量及其初始化值，以逗号分隔。例如：

```cpp
class MyClass {
private:
    int x;
    int y;

public:
    // 使用初始化列表初始化成员变量
    MyClass(int a, int b) : x(a), y(b) {
        // 构造函数的函数体
    }
};
```

在上面的示例中，构造函数 `MyClass(int a, int b)` 使用了初始化列表来初始化成员变量 `x` 和 `y`。

**优点**

1. **效率更高：** 使用初始化列表直接在对象构造时初始化成员变量，避免了先构造对象再赋值的过程，提高了效率。
2. **const 成员变量和引用类型成员变量必须使用初始化列表：** 在初始化列表中初始化 const 成员变量和引用类型成员变量是唯一可行的方式。
3. **初始化顺序：** 成员变量在初始化列表中的顺序决定了初始化的顺序，与它们在类中声明的顺序无关。这有助于避免一些潜在的问题。

总之，C++ 的初始化列表是一种强大的特性，可以帮助你更优雅地初始化类的成员变量，并提高代码的效率和可读性。



## 拷贝构造

### 浅拷贝

当你没有显式地定义拷贝构造函数时，编译器会自动生成一个默认的拷贝构造函数。让我们通过一个简单的示例来演示这一点：

```cpp
class MyClass {
private:
    int x;
    int* ptr;

public:
    // 构造函数
    MyClass(int val) : x(val), ptr(new int(val)) {}

    // 成员函数，打印对象的值
    void print() {
        std::cout << "x: " << x << ", ptr: " << *ptr << std::endl;
    }
};

int main() {
    // 创建一个对象并打印其值
    MyClass obj1(5);
    obj1.print();

    // 通过另一个对象初始化一个新对象，并打印其值
    MyClass obj2 = obj1;
    obj2.print();

    return 0;
}
```

示例中，`MyClass` 类有两个成员变量：一个是整型 `x`，另一个是指向整型的指	针 `ptr`。在构造函数中，我们分别为 `x` 和 `ptr` 分配了内存。

即使我们没有显式地定义拷贝构造函数，编译器会自动生成一个默认的拷贝构造函数，它会逐个成员地复制值。在这个示例中，`ptr` 成员是一个指针，编译器会复制指针的值，使得 `obj1` 和 `obj2` 的 `ptr` 指向相同的内存地址，==这可能会导致浅拷贝问题。==

### 深拷贝

> 要解决浅拷贝问题，需要实现一个自定义的拷贝构造函数，以确保对象之间的深度拷贝。在拷贝构造函数中，你需要手动分配新的内存空间，并将原始对象的数据复制到新分配的内存中，而不是简单地复制指针。

修改浅拷贝的示例，并添加一个自定义的拷贝构造函数来解决浅拷贝问题：

```cpp
#include <iostream>

class MyClass {
private:
    int x;
    int* ptr;

public:
    // 构造函数
    MyClass(int val) : x(val), ptr(new int(val)) {}

    // 自定义拷贝构造函数
    MyClass(const MyClass& other) : x(other.x), ptr(new int(*other.ptr)) {}

    // 析构函数
    ~MyClass() {
        delete ptr; // 释放指针指向的内存
    }

    // 成员函数，打印对象的值
    void print() {
        std::cout << "x: " << x << ", ptr: " << *ptr << std::endl;
    }
};

int main() {
    // 创建一个对象并打印其值
    MyClass obj1(5);
    obj1.print();

    // 通过另一个对象初始化一个新对象，并打印其值
    MyClass obj2 = obj1;
    obj2.print();

    return 0;
}
```

在这个示例中，我们添加了一个自定义的拷贝构造函数 `MyClass(const MyClass& other)`。在这个函数中，我们手动分配了新的内存空间，并将 `other` 对象的数据复制到新分配的内存中。这样做就确保了 `obj1` 和 `obj2` 的 `ptr` 指向的是不同的内存地址，从而避免了浅拷贝问题。

**注意**

在自定义拷贝构造函数中，我们进行了深度拷贝，复制了 `ptr` 指向的数据。并且在**析构函数**中，我们释放了 `ptr` 指向的内存。这样做可以确保资源的正确管理，避免内存泄漏。