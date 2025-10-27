---
comments: true
---

# Lecture 3: Class

## Class

以面向过程的思想，对于一个点及其移动操作，我们可以写出如下代码：  

```cpp
typedef struct point {
    int x;
    int y;
} Point;

Point a;
a.x = 1;
a.y = 2;

void print (const Point* p) {
    printf("%d %d\n", p->x, p->y);
}
print(&a);

void move(Point* p, int dx, int dy) {
    p->x += dx;
    p->y += dy;
}
```

以面向对象的思想，我们将以上的数据与操作抽象并封装为一个类（`class`）：

```cpp
class Point {
private:
    int x, y;                   // 通常将类的属性设为 private（但不是绝对的）

public:
    void init(int ix, int iy);
    void move(int dx, int dy);
    void print() const;
}

void Point::init(int ix, int iy) {
    x = ix;
    y = iy;
}

void Point::move(int dx, int dy) {
    x += dx;
    y += dy;
}

void Point::print() const {
    std::cout << x << ' ' << y << std::endl;
}
```

`init()` 在 C++ 中更标准的做法是使用构造函数（Constructor），构造函数是一个与类同名且没有返回值的成员函数，在创建对象时必须提供初始值并自动调用。

```cpp
class Point {
public:
    Point(int ix, int iy);
}

Point::Point(int ix, int iy) {
    x = ix;
    y = iy;
}

// Use member initializer list to init
Point::Point(int ix, int iy) : x(ix), y(iy) {}
```

### `::` resolver

- <Class Name\>::<function name\>
- ::<function name\>：当前的函数不属于任一个具体的类，而是全局作用域下的一个函数

```cpp
void S::f() {
    ::f();      // 尝试调用全局的 f
    f();        // 编译器会优先从当前类的作用域中尝试调用 f，等价于 S::f();
    ::a++;      // 全局变量 a++
    a--;        // 当前类中的 a--，等价于 this->a--
}
```

### stash

stash 实现了一个容器（Container）：

- 容器是可用于存放其它对象的对象
- 对于大多数容器，最一般的接口是 `put()` 和 `get()`
- stash 是一个可以存放对象并动态扩容的容器

对于 stash 而言：

- 无类型容器
- 存放相同类型的对象
- `add()`, `fetch()`
- 需要时动态扩容

### `this`

`this` 是所有成员函数的隐式参数，以 stash 的一个成员函数为例：

```cpp
void Stash::initialize(int size)
// 可以将上一行理解为：
void Stash::initialize(Stash* this, int size)
```

在一个类的成员函数中，可以使用 `this` 作为指向当前类的指针，它是每个成员函数自然含有的局部变量，不能由自己定义。  

## Object

在 C++ 中，所有的一切都可以看作是对象。Objects = Attributes (Data) + Services (Operation) 在 C++ 中，对象只是一个变量，更加纯粹的定义是“存储区域”（region of storage），之前的结构体变量也只是 C++ 中的对象。

### OOP Characteristics

1. Everything is an object.
2. A program is a bunch of objects telling each other what to do by sending messages.
3. Each object has its own memory made up of other objects.
4. Every object has a type.
5. All objects of a particular type can receive the same messages.

### Compile unit

- 在 C++ 中，一个 `.cpp` 文件是一个编译单元，编译器会将 `.cpp` 文件编译成 `.obj` 文件
- 链接器会将所有的 `.obj` 文件链接成一个可执行文件
- 如果要在多个 `.cpp` 文件中共享信息，可以使用 `.h` 头文件

### The header files

- `#include` 会将 `.h` 文件的内容直接复制到 `.cpp` 文件中，一般来说有两种：
    - `#include "xx.h"`：一般会在当前目录下寻找文件
    - `#include <xx.h>`：一般会在特定的目录下（取决于编译环境）寻找文件
    - `#include <xx>`：等效于 `#include <xx.h>`
- 以下是一个标准的头文件结构，里面的声明仅出现一次，这样可以避免头文件内容被多次包含，从而导致编译失败的问题

```cpp
#ifndef HEADER_FLAG
#define HEADER_FLAG
// Your declarations here...
#endif  // HEADER_FLAG

#pragma once // Also works
```

## Makefile

本课程对 Makefile 不做过多展开，笔者对 Makefile 也没有十分了解，在此就不多花篇幅了。也许可供参考的部分资料：

<!-- - [Makefile Tutorial](https://makefiletutorial.com/){target="_blank"}
- [Makefile 教程（上面网站的中文翻译）](https://gavinliu6.github.io/Makefile-Tutorial-zh-CN/#/){target="_blank"} -->
- [GNU Make Manual](https://www.gnu.org/software/make/manual/make.html){target="_blank"}  
- [CSDIY 中提到的 GNU Make 文档](https://seisman.github.io/how-to-write-makefile/overview.html){target="_blank"}  