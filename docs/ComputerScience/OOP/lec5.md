---
comments: true
---

# Lecture 5: Inline, Composition, and Inheritance

## Inline

### Inline Functions

在 C++ 中，我们可以使用 `inline` 关键字来定义内联函数（inline functions）。内联函数是一种特殊的函数，编译器会在调用函数的地方直接展开函数体（类似于预处理器宏），而不是通过函数调用的方式，从而消除函数调用的开销。

```cpp
inline int f(int i) {
    return 2 * i;
}

int main() {
    int a = 4;
    int b = f(a);
}
```

在上面的例子中，我们定义了一个简单的内联函数 `inline int f(int i)`，编译器会将这个函数在原地展开，变成：

```cpp
int main() {
    int a = 4;
    int b = 2 * a;
}
```

- 内联函数的优点是可以减少函数调用的开销，提高程序的运行效率；缺点是会增加代码的长度，因为函数体会被复制到调用函数的地方，这是典型的以空间换时间
- 相比于宏定义，内联函数会进行参数类型检查，而宏定义不会
- 通常我们要求内联函数的主体放在头文件中

下面是一个类型检查的例子：

=== "例：类型检查"
    ```cpp
    #include <iostream>

    #define f1(a) (a) + (a)

    inline int f2(int i) {
        return 2 * i;
    }

    int main() {
        double a = 4.5;
        std::cout << f1(a) << std::endl;
        std::cout << f2(a) << std::endl;
        return 0;
    }
    ```

    上例的输出结果为：

    ```bash
    9
    8
    ```

下面是宏在特殊情况下的例子：

=== "例：宏的特殊情况"
    ```cpp
    #include <iostream>

    using namespace std;

    #define unsafe(i) \
        (i >= 0) ? (i) : -(i)

    inline int safe(int i) {
        return (i >= 0) ? (i) : -(i);
    }

    int main() {
        int j = -4;
        int ans = unsafe(j++);  // (j++ >= 0) ? (j++) : -(j++); 因此 j++ 执行 2 次
        cout << ans << endl;
        cout << j << endl;

        j = -4;
        ans = safe(j++);
        cout << ans << endl;
        cout << j << endl;
        return 0;
    }
    ```

    该例中，宏 `unsafe()` 与内联函数 `safe()` 的定义完全一致，但结果会有所差别。在调用宏时其会将参数完全替换，而内联函数则类似于函数，因此其结果为：

    ```cpp
    3
    -2
    4
    -3
    ```

### Inline may not in-line

`inline` 关键字只是对编译器的建议（hint），编译器不一定会采纳。若内联函数太大 / 调用自身（即进行了递归），则编译器会将其视作普通的函数处理。

### Inline inside classes

在类中定义的函数会被自动内联。

### Access functions

存取函数（或访问函数，Access functions）指获取或修改类的私有成员变量的成员函数。通常会被定义在类内部，因此会被编译器自动内联。

总的来说，对于内联函数的编写有以下建议：

- inline：
    - Small functions，即 2 - 3 行的小型函数
    - 频繁调用的函数，例如在循环中调用到的函数
- not inline：
    - Large functions
    - Recursive functions

## Composition

组合（Composition）指利用已存在的对象来构造新的对象，即复用已有的实现。

包含（inclusion）的方式：

- 完全（Fully）包含：构造函数和析构函数会被自动调用
- 引用（By reference）包含：需要手动初始化和销毁对象

嵌入对象（Embedded Objects）：

- 所有的嵌入对象都必须被初始化，当不提供参数时，默认的构造函数会被调用
- 构造函数可以使用初始化列表，可以给子构造函数传递参数
- 析构函数会被自动调用


<!-- ```cpp
#include <iostream>
#include <string>

using namespace std;

class Employee {
public:
    Employee(const string& name, const string& ssn);
    const string& get_name() const;
    void print(ostream& out) const;
    void print(ostream& out, const string& msg) const;
private:
    string m_name;
    string m_ssn;
}


inline const string& Employee::get_name() const {
    return m_name;
}

inline void Employee::print(ostream& out) const {
    out << m_name << endl;
    out << m_ssn << '\n' << endl;
}
``` -->