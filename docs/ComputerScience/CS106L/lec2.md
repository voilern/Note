# Lecture 2: Types and Structs

<!-- 
???+ Agenda
    - What is C++
    - Structs bundle data together
    - Code demo
    - Improving our code 
-->

## The Type System
- A type refers to the "category" of a variable  
- C++ comes with built-in types, such as:  
    - int 
    - double 
    - string  
    - bool 
    - size_t  
    
C++ enforces the appropriate use of variable types through its type system, so we say C++ is a **statically typed** language.  

### Static Typing
- Every variable must declare a type 
- Once declared, the type cannot change

### Function Overloading
If we define two functions with the same name but different parameters, such as:  

```cpp
double axolot(int x) {          // ver 1
    return (double) x + 3;
}

double axolot(double x) {       // ver 2
    return x + 3;
}

axolot(2);          // This will use ver 1, returns 5.0
axolot(2.0);        // This will use ver 2, returns 6.0
```

We see it uses different versions of functions with same name according to different input's types, this is called **Function Overloading**.  

We say C++ is a **compiled, statically typed** language. Static typing offers a number of benefits to performance and readability. In particular, it gives the compiler additional information about variables, allowing it to allocate memory for these variables more efficiently. In larger organizations and codebases, it also makes code easier to understand and reason about.  

## Structs
Structs are a way to extend the type system by bundling multiple values together into one object.   

```cpp
struct StanfordID {
    string name;        // These are called fields
    string sunet;       // Each has a name and type
    int idNumber;
};

StanfordID id;          // Initial struct
id.name = "Jacob";      // Access field with "."
id.sunet = "jtrb";
id.idNumber = 6504417;
```

To return multiple values in one function, we can create a struct function like this. This will return a struct, which contains multiple fields.   

```cpp
StanfordID issueNewID() {
    StanfordID id;
    id.name = "Jacob";
    id.sunet = "jtrb";
    id.idNumber = 6504417;
    return id;
}
```

In a more modern way, we can use **Uniform Initialization** to initialize a struct.  

```cpp
StanfordID jrb = {"Jacob", "jtrb", 6504417};    // Order depends on field order in struct. '=' is optional
StanfordID fi {"Fabio", "fibanez", 6504418};
```

A struct bundles **named variables** into a new type.  

### `std::pair`
For these double appeared elements, we can use `std::pair`. The fields of `std::pair` are `first` and `second`.  

```cpp
std::pair<std::string, int> dozen { "Eggs", 12 };
std::string item = dozen.first;
int quantity = dozen.second;
```

`std::pair` is technically not a type, but a **template**. When using pair, we must list the types of `first` and `second` inside the `<>`. Template will be discussed in a later chapter.    

```cpp
template <typename T1, typename T2>     // This is in `utility` header file
struct pair {
    T1 first;
    T2 second;
};
std::pair<std::string, int>

struct pair {                       // std::pair is equal to this struct
    std::string first;
    int second;
}
```

### std: The C++ Standard Library
- Built-in types, functions, and more provided by C++
- You need to `#include` the relevant file  
    - `#include <string> -> std::string`
    - `#include <utility> -> std::pair`
    - `#include <iostream> -> std::cout, std::endl`
- We prefix standard library names with `std::`. Although if we write `using namespace std;` we actually don't have to, but this is considered bad style as it can introduce ambiguity.  
- Without `#include` header file, we can create a template to still use some `std`.

### Code Demo
This is a simple function to solve Quadratic Equations.  

```cpp
#include <utility>
#include <cmath>

std::pair<bool, std::pair<double, double>> solveQuadratic(double a, double b, double c) {
    double delta = b * b - 4 * a * c;
    if (delta >= 0) {
        double sqrt_delta = std::sqrt(delta);
        double x1 = (-b + sqrt_delta) / (2 * a);
        double x2 = (-b - sqrt_delta) / (2 * a);
        return {true, {x1, x2}};
    } else {
        return {false, {0.0, 0.0}};
    }
}
```

## Modern Typing
We have known that C++ is a staticalliy-typed language, the types of every variable, parameter, and function return type must be known at compile time. While this affords us many perks, writing out long type names can become inconvenient. To counteract this, modern C++ offers two mechanisms to make typing easier.  

### Type aliases with `using`
We can create **type aliases** with the `using` keyword, which is identical to a `typedef` in C language, to avoid the hassle of writing a long type name.    

```cpp
std::pair<bool, std::pair<double, double>> solveQuadratic(double a, double b, double c);

// These two implementations are totally equivalent
using Zeros = std:pair<double, double>;
using Solutions = std::pair<bool, Zeros>;
Solutions solveQuadratic(double a, double b, double c);
```

`using` is kind of like a variable for types!  

### Type deduction with `auto`
In other cases, we'd perfer not to have to worry about the types at all. The `auto` keyword tells the compuler to infer a variable, parameter, or function return type from the context in which it occurs.  

```cpp
std::pair<bool, std::pair<double, double>> solveQuadratic(double a, double b, double c);

// These two implementations are totally equivalent
auto result = solveQuadratic(double a, double b, double c);
// result still has type std::pair<bool, std::pair<double, double>>
```

To note, `auto` is still statically typed, try to assign anything other than a `std::pair<bool, std::pair<double, double>>` to `result` is invalid.  

One area often using `auto` is in range-based for loops. For example:  

```cpp
std::vector<int> vec { 1, 2, 3, 4 };
for (auto element : vec) {
    std::cout << element << " ";
}
```

Because we are iterating a `std::vector<int>`, the compiler is smart enough to infer that the type of `element` is `int`.  

The compiler can also infer the return type of a function, so long as it is unambigiously clear what that return type is. In the first of following two examples, the compiler examines the type of the returned object (`20`) to derive the return type of the function `smeagol` as `int`. However, in the second example, the compiler cannot deduce weather the return type is a `std::pair<double, double>` or a self-defined `struct` containing two `double` fields.  

```cpp
auto smeagol() {
    return 20;
}                               // The return type is `int`

auto oops() {
    return { 106.0, 107.0 };
}                               // oops

// We can explicitly specify the return type to fix this issue
```

Meanwhile, you can list the type of a parameter as `auto`. This syntax is exactly identical to creating a function that is templated on the `auto` argument, meaning that the `auto` type will be inferred with whatever type is passed to the function when it is called.  

```cpp
void borges(auto cuento) {
    std::cout << cuento << std::endl;
}
borges(std::string { "el sur" });       // `auto` deduced as `std::string`
```
