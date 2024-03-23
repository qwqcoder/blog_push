---
title: Java 语法学习
tags:
  - Java
description: 简单记录下Java的语法使用
top_img: /img/cover/wyy2.png
abbrlink: 80bdef1f
date:
cover:
---

# 输入输出

+ `Scanner`, `System.out.printf()`

+ `BufferedReader`, `BufferedWriter`

  **注意: **`BufferedWriter` 需要手动刷新缓冲区

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) throws Exception {
        /*Scanner输入*/
        Scanner sc = new Scanner(System.in);
        String str = sc.next();  // 读入下一个字符串
        int x = sc.nextInt();  // 读入下一个整数
        float y = sc.nextFloat();  // 读入下一个单精度浮点数
        double z = sc.nextDouble();  // 读入下一个双精度浮点数
        String line = sc.nextLine();  // 读入下一行
        
        /*BufferedReader输入*/
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        System.out.println(str);
        
        /*printf()输出*/
        System.out.println(123);  // 输出整数 + 换行
        System.out.println("Hello World");  // 输出字符串 + 换行
        System.out.print(123);  // 输出整数
        System.out.print("yxc\n");  // 输出字符串
        System.out.printf("%04d %.2f\n", 4, 123.456D);  // 格式化输出，float与double都用%f输出
        
        /*BufferedWriter输出*/
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        bw.write("Hello World\n");
        bw.flush();  // 需要手动刷新缓冲区
    }
}
```





# 数组

## 一维数组

+ 定义

```java
int[] a = new int[10];
int[] b; // 未初始化，需要在后续代码中为其分配内存空间
// 上面两行代码可以简写为如下代码
int[] a = new int[10], b;
```

+ 初始化

```java
int[] a = {0, 1, 2};        // 含有3个元素的数组，元素分别是0, 1, 2
int[] b = new int[3];       // 含有3个元素的数组，元素的值均为0
char[] d = {'a', 'b', 'c'}; // 字符数组的初始化, 字符数组默认初始化为 \u0000 (不可见字符)
```

## 多维数组

+ 定义

```java
int[][] a = new int[3][4]; // 大小为3的数组，每个元素是含有4个整数的数组。
int[][][] b = new int[10][20][30]; // 将所有元素的初值为0
// 大小为10的数组，它的每个元素是含有20个数组的数组
// 这些数组的元素是含有30个整数的数组
```

+ 初始化

```java
int[][] a = {           // 三个元素，每个元素都是大小为4的数组
            {0, 1, 2, 3},       // 第1行的初始值
            {4, 5, 6, 7},       // 第2行的初始值
            {8, 9, 10, 11}      // 第3行的初始值
        };


for (int i = 0; i < 4; i ++ )  // 将第一行全部变成0
    a[0][i] = 0;

for (int i = 0; i < 3; i ++ ) {  // 输出二维数组
    for (int j = 0; j < 4; j ++ ) {
        System.out.printf("%d ", a[i][j]);
    }
    System.out.println();
}
```



## 常用 **API**

+ 属性: `length`, 返回数组长度
+ `Arrays.sort()`, 数组排序
+ `Arrays.fill(int[] a, int val)`, 填充数组
+ `Arrays.toString`, 将数组转为字符串
+ `Arrays.deepToString`, 将多维数组转为字符串
+ 数组不可扩容
+ 使用 `Arrays` 需要 `import java.util.Arrays`





# 字符串

+ 每个常用字符都对应一个 `-128 ~ 127` 的数字，二者之间可以相互转化。注意：目前负数没有与之对应的字符。

+ 常用ASCII值：`'A'- 'Z'` 是65 ~ 90，`'a' - 'z'` 是 `97 - 122`，`0 - 9` 是 

  `48 - 57`。
  字符可以参与运算，运算时会将其当做整数

## String类

+ 初始化

```java
String a = "Hello World";
String b = "My name is ";
String x = b;  // 存储到了相同地址
String c = b + "yxc";  // String可以通过加号拼接
String d = "My age is " + 18;  // int会被隐式转化成字符串"18"
String str = String.format("My age is %d", 18);  // 格式化字符串，类似于C++中的sprintf
String money_str = "123.45";
double money = Double.parseDouble(money_str);  // String转double
```

> 在Java中，String 类是不可变的，这意味着一旦创建了一个字符串对象，它的内容就不能被修改。执行 `a += "World";` 这样的字符串拼接操作时，实际上会创建一个新的字符串对象，其中包含了原始字符串 "Hello " 和新字符串 "World" 的连接结果。

+ 访问

```java
String str = "Hello World";
for (int i = 0; i < str.length(); i ++ ) {
    System.out.print(str.charAt(i));  // 只能读取，不能写入
}
```

+ 常用 API

1. `length()`：返回长度
   
2. `split(String regex)`：分割字符串

3. `indexOf(char c)`、`indexOf(String str)`、`lastIndexOf(char c)`、`lastIndexOf(String str)`：查找，找不到返回 `-1`

4. `equals()`：判断两个字符串是否相等，注意不能直接用 `==`

5. `compareTo()`：判断两个字符串的字典序大小，负数表示小于，`0` 表示相等，正数表示大于

6. `startsWith()`：判断是否以某个前缀开头

7. `endsWith()`：判断是否以某个后缀结尾

8. `trim()`：去掉首尾的空白字符

9. `toLowerCase()`：全部用小写字符

10. `toUpperCase()`：全部用大写字符

11. `replace(char oldChar, char newChar)`：替换字符

12. `replace(String oldRegex, String newRegex)`：替换字符串

13. `substring(int beginIndex, int endIndex)`：

    返回 `[beginIndex, endIndex)` 中的子串

14. `toCharArray()`：将字符串转化成字符数组



+ 输入输出

1. `Scanner.next()`
2. `Scanner.nextLine()`

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str1 = sc.next();  // 输入字符串，遇到空格、回车等空白字符时停止输入
        String str2 = sc.nextLine();  // 输入一整行字符串，遇到空格不会停止输入，遇到回车才会停止

        System.out.println(str1);  // 可以直接输出
        System.out.printf("%s\n", str2);  // 也可以格式化输出，用 %s 表示字符串
    }
}
```



+ `StringBuilder`, `StringBuffer`

1. `String` 无法修改, 如果想要修改字符串, 可以使用 `StringBuilder`, `StringBuffer`

2. `StringBuffer` 线程安全，速度较慢；`StringBuilder` 线程不安全，速度较快。

```java
StringBuilder sb = new StringBuilder("Hello ");  // 初始化
sb.append("World");  // 拼接字符串
System.out.println(sb);

for (int i = 0; i < sb.length(); i ++ ) {
    sb.setCharAt(i, (char)(sb.charAt(i) + 1));  // 读取和写入字符
}

System.out.println(sb);
```

> 常使用  `reverse()` 翻转字符串



# 函数

+ 参数传递

1. 值传递
2. 引用传递

+ 值传递

  八大基本数据类型和 `String` 类型等采用值传递。

  **将实参的初始值拷贝给形参**。此时，对形参的改动不会影响实参的初始值。

```java
public class Main {
    private static void f(int x) {
        x = 5;
    }

    public static void main(String[] args) {
        int x = 10;
        f(x);
        System.out.println(x);
    }
}
```

+ 引用传递

  除 `String` 以外的数据类型的对象，例如数组、`StringBuilder` 等采用引用传递。

  将实参的引用（**地址**）传给形参，通过引用找到变量的真正地址，然后对地址中的值修改。所以此时对形参的修改会影响实参的初始值。

```java
import java.util.Arrays;

public class Main {
    private static void f1(int[] a) {
        for (int i = 0, j = a.length - 1; i < j; i ++, j -- ) {
            int t = a[i];
            a[i] = a[j];
            a[j] = t;
        }
    }

    private static void f2(StringBuilder sb) {
        sb.append("Hello World");
    }

    public static void main(String[] args) {
        int[] a = {1, 2, 3, 4, 5};
        f1(a);
        System.out.println(Arrays.toString(a));

        StringBuilder sb = new StringBuilder("");
        f2(sb);
        System.out.println(sb);
    }
}
```

# 接口

## 类的定义

 类的定义

+ `public`: 所有对象均可以访问

+ `private`: 只有本类内部可以访问

+ `protected`：同一个包或者子类中可以访问

+ 不添加修饰符：在同一个包中可以访问

+ 静态（带static修饰符）成员变量/函数与普通成员变量/函数的区别：

  所有static成员变量/函数在类中只有一份，被所有类的对象共享；
  所有普通成员变量/函数在类的每个对象中都有独立的一份；

> 静态函数中只能调用静态函数/变量；普通函数中既可以调用普通函数/变量，也可以调用静态函数/变量。

```java
class Point {
    private int x;
    private int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void setX(int x) {
        this.x = x;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public String toString() {
        return String.format("(%d, %d)", x, y);
    }
}
```



## 类的继承

每个类只能继承一个父类

```java
class ColorPoint extends Point {
    private String color;

    public ColorPoint(int x, int y, String color) {
        super(x, y);
        this.color = color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public String toString() {
        return String.format("(%d, %d, %s)", super.getX(), super.getY(), this.color);
    }
}
```

## 多态

```java
public class Main {
    public static void main(String[] args) {
        Point point = new Point(3, 4);
        Point colorPoint = new ColorPoint(1, 2, "red");

        // 多态，同一个类的实例，调用相同的函数，运行结果不同
        System.out.println(point.toString());
        System.out.println(colorPoint.toString());
    }
}
```



## 接口

`interface` 与 `class` 类似。主要用来定义类中所需包含的函数。

**接口也可以继承其他接口，一个类可以实现多个接口。**

+ 定义

接口不添加修饰符时, 默认为 `public`

```java
interface Role {
    public void greet();
    public void move();
    public int getSpeed();
}
```

+ 继承

接口可以继承多个接口

```java
interface Hero extends Role {
    public void attack();
}
```

+ 实现

每个类可以实现多个接口

```java
class Zeus implements Hero {
    private final String name = "Zeus";
    public void attack() {
        System.out.println(name + ": Attack!");
    }

    public void greet() {
        System.out.println(name + ": Hi!");
    }

    public void move() {
        System.out.println(name + ": Move!");
    }

    public int getSpeed() {
        return 10;
    }
}
```

+ 多态

```java
class Athena implements Hero {
    private final String name = "Athena";
    public void attack() {
        System.out.println(name + ": Attack!!!");
    }

    public void greet() {
        System.out.println(name + ": Hi!!!");
    }

    public void move() {
        System.out.println(name + ": Move!!!");
    }

    public int getSpeed() {
        return 10;
    }
}

public class Main {
    public static void main(String[] args) {
        Hero[] heros = {new Zeus(), new Athena()};
        for (Hero hero: heros) {
            hero.greet();
        }
    }
}
```

