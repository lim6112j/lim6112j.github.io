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
## Variables
Variables are nothing but **reserved memory locations** to store values. This means that when you create a variable you reserve some space in memory. The sizes of variables might be different from those shown in the above table, depending on the compiler and the computer you are using.

## Variable Type
Variable Type determines the **size and layout of the variable's memory**; the **range of values** that can be stored within that memory; and **the set of operations** that can be applied to the variable.

## Variable Initialize
For definition without an initializer: variables with static storage duration are implicitly initialized with NULL (all bytes have the value 0); the initial value of all other variables is undefined.

## Variable definition VS Declaration
## C++ References vs Pointers
`A reference variable is an alias, that is, another name for an already existing variable. Once a reference is initialized with a variable, either the variable name or the reference name may be used to refer to the variable.`

References are often confused with pointers but three major differences between references and pointers are:

You cannot have NULL references. You must always be able to assume that a reference is connected to a legitimate piece of storage.

Once a reference is initialized to an object, it cannot be changed to refer to another object. Pointers can be pointed to another object at any time.

A reference must be initialized when it is created. Pointers can be initialized at any time.
```c++
const Type& is equivalent to Type const&?
```
## Data Encapsulation VS Data Abstraction
Data encapsulation is a mechanism of bundling the data, and the functions that use them and data abstraction is a mechanism of exposing only the interfaces and hiding the implementation details from the user.

## Abstract Class
A class is made abstract by declaring **at least one** of its functions as pure virtual function. A pure virtual function is specified by placing "= 0" in its declaration as follows:

## c++ template
A template is a blueprint or formula for creating a generic class or a function. 
```c++
template <class type> ret-type func-name(parameter list) {
   // body of function
} 
```
