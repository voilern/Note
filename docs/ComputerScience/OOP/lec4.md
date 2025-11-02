---
comments: true
---

# Lecture 4: Object Interaction

## Constructor and Destructor

### Constructor

我们需要有机制保证对象被创建时有合理的初值，如果没有赋初值，那么对象的状态是不确定的，这时候就需要构造函数（Constructor）。  

- 构造函数名和结构名完全相同，没有返回类型  
- 如果（当且仅当）没有定义构造函数时，编译器会自动生成一个默认构造函数，默认的构造函数没有任何参数  
- 如果一个类具有构造函数，编译器会在创建对象时自动调用构造函数

构造函数的定义一般有以下形式：  

```cpp
struct Y {
    int i;
    float x;
    Y(int a) { i = a; }
};

int main() {
    Y y1[2] = { Y(1), Y(2) };
}
```

这样是没问题的，但如果我们将 `y1` 的长度设置为 3，会有如下报错：  

```bash
voilern@Anulis:~/cpp/OOP/lec4$ g++ lec4_1.cpp -o lec4_1
lec4_1.cpp: In function ‘int main()’:
lec4_1.cpp:11:26: error: could not convert ‘<brace-enclosed initializer list>()’ from ‘<brace-enclosed initializer list’ to ‘Y’
   11 |     Y y1[3] = {Y(1), Y(2)};
      |                          ^
      |                          |
      |                          <brace-enclosed initializer list>
```

这是因为 `Y y1[3]` 会调用默认构造函数，即 `Y y1[3] = { Y(1), Y(2), Y() }`，而我们并没有定义默认的构造函数，所以会报错。

```cpp
struct Y {
    int i;
    float x;
    Y(int a) { i = a; }
    Y() {}
};
```

定义默认构造函数后即解决，事实上有了默认构造函数后，我们甚至可以使用 `Y y1[3]` 而不需要赋初值。  

### Destructor

相应地，在对象的生命周期结束时，我们需要有机制来释放资源，因此我们有析构函数（Destructor）。  

- 析构函数名与结构名完全一致，在之前需要加上 `~`，没有返回类型
- 对象即将结束生命周期时，析构函数被调用
- 析构函数没有参数

RAII（Resource Acquisition Is Initialization）是 C++ 中惯用的一种编程技术，它将必须在使用前获取的资源（例如分配的堆内存、执行线程等）的生命周期绑定到对象的生命周期上（参考：[cpp reference - RAII](https://en.cppreference.com/w/cpp/language/raii.html)）这里不多做介绍。  

## Initialization

此处对 C++ 中的初始化介绍较为简略，可以参考 [CS106L - Lecture 3: Initialization & References](../CS106L/lec3.md) 中的笔记。  

### Non Static Data Member Init (C++ 11)

C++ 11 后，我们可以直接对成员变量初始化。其好处是通过该方式实例化出的对象均具有相同的初始值。  

```cpp
class MyClass {
public:
    int a = 10;                     // Member Init
    double b = 3.14;
    MyClass(int ax) { a = ax; }     // 通过构造函数可以覆盖初始化的默认值
};
```

### Initializer List

事实上，在函数体当中的赋值并非真正意义上的初始化。在构造函数中，我们可以使用初始化列表（Initialization List）来初始化成员变量，而不是在函数体中进行初始化。

```cpp
class Point {
private:
    const float x, y;
    Point(float xa = 0.0, float ya = 0.0) 
        : y(ya), x(xa) {}                   // Initializer list
};

// This is wrong! For `const` we can only do initialization, rather than assignment.
Point(float xa = 0.0, float ya = 0.0) {
    x = xa;
    y = ya;
}
```

- 可以初始化任何类型的数据，包括内置类型和类类型，并且我们无需在构造函数体内执行赋值  
- 初始化按照声明的顺序进行，而并不是按照初始化列表中的顺序
    - 在上面的例子中，`x` 先于 `y` 被声明，因此 `x` 总是会先于 `y` 被初始化
    - 而析构的顺序则与之相反，因此 `y` 会先于 `x` 被析构

对于初始化我们有两种方法，一种是显式的初始化列表（例如 `Student::Student(string s) : name(s) {}`，另一种是隐式的初始化列表 + 赋值（例如 `Student::Student(string s) { name = s; }`），但由于后者还需要一个默认构造函数（如果后者所属的类缺少默认构造函数，将无法编译），因此更推荐写初始化列表，可以避免一些可能存在的问题，也可以提高代码的可读性。  

## Function Overloading

我们可以定义同名但有不同参数列表的函数，编译器将根据调用时的形参决定所调用的函数。

```cpp
void print(char * str, int width);
void print(double d, int width);
void print(long l, int width);
void print(char * s);

print("oop", 3);
print("oop");
print(1999L, 5);
print(1999.0, 5);
```

如果在调用时没有能够完全匹配传入参数的函数，编译器会尝试做自动类型转换，但如果转换时的成本 / 优先级相同，编译器会感到困惑，会发生编译错误。

```cpp
void f(short i);
void f(double d);

// ambiguous!
f('a');
f(2);
f(2L);
```

在上面的例子中没有精确匹配参数的函数，因此编译器会尝试进行类型转换：

- `f('a')`: `char -> short` 是整型提升（Promotion），而 `char -> double` 是标准转换（Conversion），C++ 规定提升的优先级高于标准转换，因此编译器会调用 `f(short i)`  
- `f(2)`: `int -> short` 与 `int -> double` 都是标准转换，两个转换的优先级相同，产生歧义  
- `f(2L)`: `long -> short` 与 `long -> double` 都是标准转换，同样会产生歧义
