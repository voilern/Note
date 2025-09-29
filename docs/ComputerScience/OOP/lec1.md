---
comments: true
---

# Lecture 2: Using Objects

## String

`string` 是 C++ 中的一个类，调用时需要 `#include <string>`，可以像其它类型一样定义、赋值 `string` 变量。  

### Assignment

```cpp
char charr1[20];
char charr2[20] = "jaguar";

string str1;
string str2 = "panther";

charr1 = charr2;    // illegal
str1 = str2;        // legal
```

### Concatenation for string

```cpp
string str3;
str3 = str1 + str2;
str1 += str2;
str1 += "lalala";
```

## File I/O

```cpp
#include <fstream>
using namespace std;

ofstream File1("test1.txt");
File1 << "Hello world!" << endl;

ifstream File2("test2.txt");
string str;
File2 >> str;
```

## Memory Model

- 全局变量
- `extern`
- `static`

## Pointers to Objects


## New & Delete

```cpp
using namespace std;

struct Circle {
    double r;

    // 构造函数
    Circle() {
        r = 10;
        cout << "constructor invoked, r = " << r << endl;
    }

    // 析构函数，在销毁时会自动隐式调用
    ~Circle() {
        cout << "destructor invoked, r = " << r << endl;
    }
};

int main() {
    cout << "\nPart 0:\nCalling new Circle()" << endl;
    Circle* newObj = new Circle();
    cout << "After using new: " << newObj->r << endl;

    delele newObj;              // 隐式调用析构函数
    newObj = nullptr;
    cout << "After using delete" << endl;

    cout << "\nPart 1:\nCalling new Circle()" << endl;
    Circle* anotherObj = new Circle[5];
    for (int i = 0; i < 5; i++) {
        anotherObj[i].r = i;
    }
    cout << "After using new: " << anotherObj->r << endl;

    delete[] anotherObj;        // 隐式调用 5 次析构函数
    anotherObj = nullptr;
    cout << "After using delete" << endl;
}
```

其中，C++ 中在 `delete` 后将变量置为空指针是一种更加安全的写法，否则可能会造成空悬指针（dangling pointer）的问题。  

## References

- 引用（references）是 C++ 中的一种新的数据类型

```cpp
type& refname = name;
char c;           // 单个字符
char* p = &c;     // 指向字符的指针
char& r = c;      // 指向字符的引用
```

- 对于一般变量，创建引用时必须初始化，必须绑定（binding）到具体的对象上
- 可用于参数列表（parameter list）或成员变量（member variable）中

```cpp 
int x = 47;
int& y = x;             // 创建 y 作为 x 的一个引用

cout << "y = " << y;    // y = 47
y = 18;
cout << "x = " << x;    // x = 18
```

- 引用的绑定不可修改，对引用赋值相当于直接对绑定的对象赋值
- 引用的对象必须是左值（`l-value`）
- 指针与引用的对比
- 一些限制：
    - 不能创建引用的引用
    - 不能创建指向引用的指针，但指针的引用是合法的
    - 不能创建引用类型的数组

## const

- `const` 声明一个具有常量，即值不可被修改的变量

```cpp
const int x = 37;
x = 27;                 // illegal
x++;                    // illegal

int y = x;              // legal，将 const 赋值给 non-const
y = x;                  // legal

const int z = y;        // legal
```

- 用 `const` 修饰的常量依旧是变量
- C++ 中 `const` 默认采用内部链接，编译器会尝试避免为常量分配具体的内存空间，常量的值将被存储在符号表（symbol table）中；如果采用 `extern`，会强制编译器为其分配内存空间
- 运行时常量（Run-time const） & 编译时常量（Compile-time const）

```cpp
const int class_size = 12;
int finalGrade[class_size];     // ok

int x;
cin >> x;
const int size = x;
double classAvg[size];          // error
```

### Aggregates & const

```cpp
const int i[] = { 1, 2, 3, 4 };
float f[i[3]];                      // illegal
struct S { int i, j; };
const S s[] = { { 1, 2 }, { 3, 4 } };
double d[s[1].j];                   // illegal
```

### Pointers & const

```cpp
char * const q = "abc";     // q 是 const
*q = 'c';                   // ok
q++;                        // error

const char *p = "ABCD";     // *p 是一个 const char
*p = 'b';                   // error
```

```cpp
string p1("fred");
const string* p = &p1;
string const* p = &p1;      // 以上两种写法等效，即指针指向的是一个 const
string *const p = &p1;      // 指针本身是 const，指针指向的是 string
```

|       |   int i;  |   const int ci = 3;   |
| :---: |   :---:   |   :---:               |
| int * ip; | ip = &i; | ip = &ci; // Error |
| const int *cip | cip = &i; | cip = &ci;   |

### String Literals

字符串字面量（String Literals）：

```cpp
char* s = "Hello, world!";
```

在上面的例子中，`s` 是一个指向字符串常量（string constant）的指针，这种写法等价于 `const char* s`，编译器接受以上代码块中的写法，但会自动视作 `const` 处理。因此 `s` 的值是不可修改的。  