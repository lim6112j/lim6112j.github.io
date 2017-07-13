---
layout: post
comments: true
categories: Development
---
## c++ 기초 정리 (refer [TutorialPoint c++](https://www.tutorialspoint.com/cplusplus/cpp_environment_setup.htm))
![image of c](https://www.visualstudio.com/wp-content/uploads/2016/05/C-4-562x309-OPx.png)
## The ANSI Standard
The ANSI standard is an attempt to ensure that C++ is portable -- that code you write for Microsoft's compiler will compile without errors, using a compiler on a Mac, UNIX, a Windows box, or an Alpha.

The ANSI standard has been stable for a while, and all the major C++ compiler manufacturers support the ANSI standard.

## C++ Compiler:
This is actual C++ compiler, which will be used to compile your source code into final executable program.

Most C++ compilers don't care what extension you give your source code, but if you don't specify otherwise, many will use .cpp by default

Most frequently used and free available compiler is GNU C/C++ compiler, otherwise you can have compilers either from HP or Solaris if you have respective Operating Systems.
```c++
#include <iostream>
using namespace std;

// main() is where program execution begins.

int main() {
   cout << "Hello World"; // prints Hello World
   return 0;
}
```
