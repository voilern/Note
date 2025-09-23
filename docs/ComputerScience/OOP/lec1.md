# Lecture 1: Introduction

## String
`string` 是 C++ 中的一个类，调用时需要 `#include <string>`，可以像其它类型一样定义、赋值 `string` 变量。  

### Assignment for string

```cpp
char charr1[20];
char charr2[20] = "jaguar";

string str1;
string str2 = "panther";

charr1 = charr2;    // 并非对 cahrr1 进行赋值
str1 = str2;        // 作用即为将 str2 的值赋给 str1
```

### Concatenation for string

```cpp
string str3;
str3 = str1 + str2;
str1 += str2;
str1 += "lalala";
```

### File I/O

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

## Pointers to Objects
