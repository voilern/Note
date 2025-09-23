---
comments: true
---

# Lecture 3: Initialization & References

## Initialization
Initialization means provides initial values at the time of consturction. To initialize a variable, there are typically three methods:  

- Direct initialization
- Uniform initialization
- Structured Binding

### Direct initialization
Direct initialization relies on the use of a `=` or a parenthsis `()`.  

```cpp
#include <iostream>

int main() {
    int numone = 12.0;
    int numtwo(12.0);

    std::cout << numone << " " << numtwo << std::endl;
    return 0;
}
```

In the above examples, we've declared `numone` and `numtwo` as `int`. However, we initialized them using the value `12.0`, which is actually a `double`. But if you use direct initializing, the compiler will not produce any error. Given that C++ is a statically-typed language, when the mis-match occurs when the variable and the value, this would be considered as a bug. This is known as a **narrowing conversion**, where the compiler will implicitly cast the value assigned to a variable to the type of the variable.  

### Uniform initialization (C++ 11)
Firstly let us see the following program:  

```cpp
#include <iostream>

int main() {
    int numone{12.0};       // Notice the curly braces
    float numtwo{12.0};

    std::cout << numone << " " << numtwo << std::endl;
    return 0;
}
```

While compiling the above program, the compiler would return the following error message:  

```
.\lec3.cpp: In function 'int main()':
.\lec3.cpp:18:16: error: narrowing conversion of '1.2e+1' from  
double' to 'int' [-Wnarrowing]
   18 |     int numone{12.0};
      |
```

This is because **Uniform initialization**, which enforces appropriate variables' constructions to their types. To fix the error, you just need to modify the initialization `int numone{12.0}` to `int numone{12}`.    

Uniform initialization is a consistent, type-safe syntax that uses `{}` to initialize objects in C++. It resolves many of the problems that come with direct initialization, especially the type safety issue associated with narrowing conversion. More importantly, Uniform initialization is ubiquitous, which means it works for all types in C++, even including those that you'll define, like structs, maps and custom classes.  

In order to use Uniform initialization, we need to wrap the values that we want to construct our variable with within `{}` brackets. Special attention need to be paid to the values that are passed into the brace-enclosed initializer. Uniform initialization enforces type-safety in C++ in that if the types of the values passed in which will be used in variable construction differ from those that we expect, the compiler will throw an error. To be clear, C++ is a typed language, but certain features of the language enforce type-safety more than others.  

Now we learn why `struct` demands specific order when initializing: naturally uniform initialization enforces you to provide values in the order in which they are declared within the `struct`.  

There are two more examples to demonstrate the use of Uniform initialization in some types:  

```cpp
#include <iostream>
#include <map>
#include <vector>

int main() {
    // Uniform initialization of a map
    std::map<std::string, int> ages{
        {"Alfa", 25},
        {"Bravo", 30},
        {"Charlie", 35}
    };

    std::cout << "Alfa's age: " << ages["Alfa"] << std::endl;
    std::cout << "Bravo's age: " << ages.at(Bravo) << std::endl;

    // Uniform initialization of a vector
    std::vector<int> numbers(1, 2, 3, 4, 26, 5);

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

As for its disadvantage, overloading conflicts with uniform initialization.  

### Structured Binding (C++ 17)
Structured binding is a method of initializing variables from data structures with size known at compile-time. For example:  

```cpp
#include <iostream>
#include <tuple>
#include <string>

std::tuple<std::string, std::string, std::string> getClassInfo() {
    std::string className = "CS106L";
    std::string buildingName = "260-113";
    std::string language = "C++";
    return {className, buildingName, language};     // This use uniform initialization
}

int main() {
    auto [ className, buildingName, language] = getClassInfo();
    // The above line is equivalent to the following lines
    // auto classInfo = getClassInfo();
    // std::string className = std::get<0>(classInfo);
    // std::string buildingName = std::get<1>(classInfo);
    // std::string language = std::get<2>(classInfo);
    std::cout << "Come to " << buildingName << "and join us for " << className << "to learn " << language << "!" << std::endl;
    return 0;
}
```

In the above example, we introduced the `std::tuple<...>` data structure. At compile time, the size of the tuple is known, so we can unpack the values in the tuple into variables, as we can see in the `auto [ className, buildingName, language] = getClassInfo();`.  

The syntax for structured binding is:  

```cpp
auto [var1, var2, ..., varN] = expression;
```

To note, we have to use `auto` type identifier here because `var1`, `var2`, ect. are possibly in different types, so the compiler here does something heavy to deduce the types of each unpacked variable.  

As for the size-unknown data structure, such as `std::vector`, structured binding would not work.  

## References
A reference is an **alias** to an already-existing thing in memory. A reference in C++ is denoted using ampersand (`&`) character. Here's a simple example:  

```cpp
int main() {
    int x = 26;
    int& r = x;
    return 0;
}
```

In the above example, `x` is declared and a reference `r` to `x` is created. So what this means is that the variable `r` points to the same underlying memory as `x`, so any changes made to `r` will also be reflected on `x`.  

In virtue of reference, we could pass a variable from a calling function into another function and have it be modified. This is called **Pass by Reference**. Relevantly we have **Pass by Value**.  

```cpp
void squareN(int& N) {
    N *= N;
}

int main() {
    int num = 5;
    squareN(num);   // num = 25 now, without `&` the value of num would not change
    return 0;
}
```

Here is a classic reference-copy bug example:  

```cpp
#include <iostream>
#include <math.h>
#include <vector>

void shift(std::vector<std::pair<int, int>> &nums) {
    for (auto [num1, num2] : nums) {
        num1++;
        num2++;
    }
}
```

In the above example we implemented a function `shift` aiming to modify the datas of `std::vector<...> &nums`, instead, executing the function would do nothing to `nums`. Why? The `auto [num1, num2]` would copy one element `std::pair<int, int>` from the vector `nums`, and disassemble the copy into the newly constructed local variable that only works in the function `num1` and `num2`, so all the following operations are just modifying the two copies, rather than the data in `nums` expected. To resolve the bug, what we need is to become variables in loop as reference, thus `num1` and `num2` would become alias of original datas, for appending `&` to `auto`.  

```cpp
for (auto& [num1, num2] : nums) {
    num1++;
    num2++;
}
```

## L-value & R-value
Before starting this section, let's see these two examples:  

```cpp
int foo() {
    return 2;
}

int main() {
    foo() = 2;
    return 0;
}
```

Try to use `g++` to compile the code above, you will get: 

```
.\lec3.cpp: In function 'int main()':
.\lec3.cpp:49:8: error: lvalue required as left operand of assignment
   49 |     foo() = 2;
      |     ~~~^~
```

Another exmaple is:  

```cpp
int& foo() {
    return 2;
}
```

Also, you will get the follwing error message while compiling the code:  

```
.\lec3.cpp: In function 'int& foo()':
.\lec3.cpp:77:12: error: cannot bind non-const lvalue reference of type 'int&' to an rvalue of type 'int'
   77 |     return 2;
      |            ^
```

Both the examples return some error message mentions `lvalue` and `rvalue`. So what do `lvalue` and `rvalue` mean in C++?  

A simplified definition of `lvalues` and `rvalues`:  

- An **lvalue** (locator value) represents an object that occupies some identifiable location in memory (i.e has an address).  
- An **rvalue** (right-hand value) is defined by exclusion, by saying that every expression is either an lvalue or an rvalue. Therefore, from the definition of lvalue above, an rvalue is an expression that **does not** represent an object occupying some identifiable location in memory.

A simple understanding for `lvalue` and `rvalue`: variables with its names are usually `lvalues`, instead are usually `rvalues`.  

The terms as defined above may appear vague. Assuming that we have an integer variable defined and assigned to:  

```cpp
int vat;
var = 4;
```

An assignment expects an `lvalue` as its left operand, and `var` is an `lvalue`, because it is an object with an identifiable memory location. On the other hand, the followings are invalid:  

```cpp
4 = var;        // ERROR
(var + 1) = 4;  // ERROR
```

Neither the constant `4` nor the expression `var + 1` are `lvalues`, which means them `rvalues`. They are not `lvalues` because both are temporary results of expressions, which don't have an identifiable memory location (i.e. they can just reside in some temporary register for the duration of the computation). Therefore, assigning to them makes no semantic sense.  

Returning to the first code snippet, it is clear what the error message means: `foo` returns a temporary value which is an `rvalue`, attempting to assign to it is an error. As for the second code snippet, the function is defined as `int& foo()`, which is expected to return a `lvalue`, however,  `2` is an `rvalue`. so the error occurs.  

Another example from CS106L:  

```cpp
#include <stdio.h>
#include <cmath>
#include <iostream>

int square(int& num) {
    return std::pow(num, 2);
}

int main() {
    int lvalue = 2;
    auto four = square(lvalue);
    auto fouragain = sqaure(2);
    std::cout << four << std::endl;
    return 0;
}
```

Compiling the code will generate the following error message:  

```
.\lec3.cpp: In function 'int main()':
.\lec3.cpp:66:29: error: cannot bind non-const lvalue reference of type 'int&' to an rvalue of type 'int'
   66 |     auto fouragain = square(2); // error: cannot bind non-const lvalue reference of type 'int&' to an rvalue of type 'int'
      |                             ^
.\lec3.cpp:59:17: note:   initializing argument 1 of 'int square(int&)'
   59 | int square(int& num) {
      |            ~~~~~^~~
```

The error occurs at `auto foutagain = square(2)`, because for the function `int sqaure(int& num)`, it expects an `non-const lvalue reference` as parameter, but `2` is an `rvalue`.  

There are still lots of features relating to `lvalue` and `rvalue`, if interested, you can refer to the following blogs.    

- [Understanding lvalues and rvalues in C and C++](https://eli.thegreenplace.net/2011/12/15/understanding-lvalues-and-rvalues-in-c-and-c){target="_blank"}  
- [（上文的译文）理解 C/C++ 中的左值和右值](https://nettee.github.io/posts/2018/Understanding-lvalues-and-rvalues-in-C-and-C/){target="_blank"}  

## Const
A simple definition of `const`: A qualifier for objects that declares they cannot be modified.  

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec{ 1, 2, 3 };                // a normal vector
    const std::vector<int> const_vec{ 1, 2, 3 };    // a const vector
    std::vector<int>& ref_vec{ vec };               // a reference to 'vec'
    const std::vector<int>& const_ref{ vec };       // a const reference

    // `push_back` is a function to add a new element in the tail of the container (vector in this example)
    vec.push_back(3);           // This is okay
    const_vec.push_back(3);     // Error!
    ref_vec.push_back(3);       // This is okay
    const_ref.push_back(3);     // Error!
    return 0;
}
```

From the definition we can explain the errors above. `const_vec` is a `const vector`, once initialized, you cannot call any functions that causes modification to it, such as `push_back`. `const_ref` is a `const reference`, though the source vector `vec` is not a `const vector`, `const_ref` is just a `read-only` alias to it. Furthermore, you can't declare a `non-const reference` to a `const` variable.  

```cpp
int main() {
    const std::vector<int> const_vec{ 1, 2, 3 };
    std::vector<int>& bad_ref{ const_vet };                         // Bad!

    const std::vector<int> another_const_vec{ 1, 2, 3 };
    const std::vector<int>& good_ref{ another_const_vec };          // Good!
}
```

`const` is a way to ensure that you can't modify a variable.  