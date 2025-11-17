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

组合（Composition）指利用已存在的对象来构造新的对象，即复用已有的实现。这是一种 "has - a" 的关系。

包含（inclusion）的方式：

- 完全（Fully）包含：对象作为成员变量被直接包含在另一个类中，构造函数和析构函数会被自动调用
- 引用（By reference）包含：成员变量是另一个对象的引用或指针，需要手动初始化和销毁对象

嵌入对象（Embedded Objects）：

- 所有的嵌入对象都必须被初始化，当不提供参数时，默认的构造函数会被调用
- 构造函数可以使用初始化列表，可以给子构造函数传递参数
- 析构函数会被自动调用

以下是一个例子：

```cpp
#include <iostream>
#include <string>

using namespace std;

class Person {
public:
    Person(string n, string a) :
        p_name(n), p_addr(a) {}
    string get_name() { return p_name; }
    string get_address() { return p_addr; }
    void set_address(string new_addr) { p_addr = new_addr; }
    void set_name(string new_name) { p_name = new_name; }
    void print() { cout << "name: " << p_name << " , address: " << p_addr << '\n';}

private:
    string p_name;
    string p_addr;
};

class Currency { ... }; // 实现细节在此省略，认为有一个参数不为空的构造函数

class SavingsAccount {
public:
    // #1 right!
    SavingsAccount(const string& name, const string& address, int cents) :
        m_saver(name, address), m_balance(cents) {}
    // 在初始化列表中中调用成员对象各自的构造函数

    // #2 wrong!
    SavingsAccount(const string& name, const string& address) :
        m_saver(name, address) {}
    // m_balance 没有被初始化

    // #3 not recommended!
    Person m_saver;
    // 在 C++ 11 后，若 Person 类有默认构造函数，是合法的
    // 但将其放在 public 区域破坏了封装性，并不推荐这种写法

    // #4 wrong!
    SavingsAccount(const string& name, const string& address, int cents) {
        // 此时 m_saver 和 m_balance 已经尝试调用默认构造函数以初始化
        m_saver.set_name(name);
        m_saver.set_address(address);
        m_balance.set_amount(cents);
    }

    ~SavingsAccount();
    void print(); { m_saver.print(); m_balance.print(); }

private:
    Person m_saver;
    Currency m_balance;
};

int main() {
    SavingsAccount account1("John", "XXX", 1000); // #1 & #4
    account1.print();

    // SavingsAccount account2("John", "XXX"); // #2
    
    // account2.m_saver.set_address("XXX"); // #3

    return 0;
}
```

在 `#2` 下，构造函数只在初始化列表中显式初始化了 `m_saver`，由于 `m_balance` 没有在初始化列表中，编译器会尝试调用 `m_balance` 的默认构造函数（即无参构造函数 `Currency::Currency()`。我们认为 `Currency` 类中没有提供默认构造函数，则此时编译无法通过，有类似如下的报错：

```bash
error: constructor for 'SavingsAccount' must explicitly initialize the member 'm_balance' which does not have a default constructor
   41 |     SavingsAccount(const string& name, const string& address) :
      |     ^
```

`#4` 中，在执行构造函数的函数体之前，所有成员变量必须已经全部完成初始化，由于 `#4` 的初始化列表为空，编译器会尝试调用默认构造函数，但在此例中 `Person` 与 `Currency` 均没有默认构造函数，程序在进入函数体前即编译失败。

### Public vs. Private

- 通常将嵌入对象声明为 `private`，因为它们是底层实现的一部分
- 可以将嵌入对象作为 `public` 以访问其全部接口，对应上例中的 `#3`

## Inheritance

继承（Inheritance）指从已存在的类中克隆新的类并扩展。这是一种 "is - a" 的关系。

- 定义一个基类（Base class，或称为超类、父类等），用于定义共同属性
- 定义多个派生类（Derived Class，或称为子类），用于继承基类的属性，并添加自己的属性
- 派生类通过 `:` 指定基类，例如 `class Derived : public/protected/private Base` 
    - 默认为 `private`

继承的优点：

- 减少重复代码，提高代码复用率
- 更好的可维护性与可扩展性

以下是一个例子：

```cpp
#include <iostream>

using namespace std;

class Base {
public:
    Base() { i = 10; j = k = 25; cout << "Base c'tor, data: " << i << '\n'; }
    Base(int x) : i(x) { cout << "Base c'tor, data: " << i << '\n'; }
    ~Base() { cout << "Base d'tor, data: " << i << '\n'; }

// private:
    int i;
private:
    int j;
protected:
    int k;
};

class Derived : public Base {
public:
    // 派生类构造函数必须在初始化列表中调用基类的构造函数
    // 如果不写，编译器会尝试调用 Base()
    // 下面的函数体中 i = 20 访问的是 Derived::i
    Derived() : Base(10) { i = 20; cout << "Derived c'tor, data: " << i << '\n'; }
    ~Derived() { cout << "Derived d'tor, data: " << i << '\n'; }

    // 派生类可以访问基类的 protected 成员
    int getProtected() { return k; }

// private:
    int i;      // 隐藏了基类的 public int i
};

int main() {
    cout << "before creating base object" << '\n';
    Base b;
    cout << "finish creating base object\n" << '\n';

    cout << "before creating derived object" << '\n';
    Derived d;
    cout << "finish creating derived object\n" << '\n';
    // 先调用 Base(10)，再调用 Derived()

    cout << "sizeof(int): " << sizeof(int) << '\n';
    cout << "sizeof(Base): " << sizeof(Base) << '\n';
    cout << "sizeof(Derived): " << sizeof(Derived) << '\n';

    // d.Base::i 可以从子类中访问到基类的成员变量
    cout << d.i << " " << d.Base::i << '\n' << endl;
    // cout << d.j << endl;             // private data is hidden
    cout << d.getProtected() << endl;   // k 未被初始化，行为不可控

    return 0;
}
```

运行结果为：

```
before creating base object
Base c'tor, data: 10
finish creating base object

before creating derived object
Base c'tor, data: 10
Derived c'tor, data: 20
finish creating derived object

sizeof(int): 4
sizeof(Base): 12
sizeof(Derived): 16
20 10

1600677166
Derived d'tor, data: 20
Base d'tor, data: 10
Base d'tor, data: 10
```

<!-- 另一个例子： -->

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

protected:
    string m_name;
    string m_ssn;
}

Employee::Employee(const string& name, const string& ssn) :
    m_name(name), m_ssn(ssn) {}

inline const string& Employee::get_name() const {
    return m_name;
}

inline void Employee::print(ostream& out) const {
    out << m_name << endl;
    out << m_ssn << '\n' << endl;
}

inline void Employee::print(ostream& out, const string& msg) const {
    out << msg << endl;
    print(out);
}

class Manager : public Employee { ... }
``` -->