# Data Abstraction

Libraries are probably the most important way to improve productivity, and one of the primary design goals of C++ is to make library use easier.

* The basic object
* What's an object
* Abstract data typing
* Header file etiquette
* Nested structures
* Global scope resolution

## The basic object

C++ functions can be palced inside `structs` as "member functions".

```c++
//: C04:CppLib.h
// C-like library converted to C++
struct Stash {
  int size; // Size of each space
  int quantity; // Number of storage spaces
  int next; // Next empty space
  // Dynamically allocated array of bytes:
  unsigned char* storage;
  // Functions!
  void initialize(int size);
  void cleanup();
  int add(const void* element);
  void* fetch(int index);
  int count();
  void inflate(int increase);
}; ///:~
```

## What's an object

In C, a `struct` is an agglomeration of data, a way to package data so you can treat it in a clump. The functions that operate on those structures are elsewhere. However, in C++, with functions in the package, the structure becomes a new creature, capable of describing both characteristics (like a C struct does) _and_ behaviors. The concept of an object, a __free-standing, bounded entity that can remember and act__, suggests itself.

In C++, an object is just a variable, and the purest definition is "a region of storage" (this is a more specific way of saying, "an object must have a unique identifier", which in the case of C++ is a unique memory address). It's a place where you can store data, and it's implied that there are also operations that can be performed on this data.

## Abstract data typing

The ability to package data with functions allows you to create a new data type. This is often called __encapsulation__.

The definition of a `struct` creates a new data type. Even though it acts like a real, built-in data type, we refer to it as an __asbtract data type__, perhaps because it allows us to abstract a concept from the problem space into the solution space. In addition, the C++ compiler treats it like a new data type, and if you say a function expects a particular `struct`, the compiler makes sure you pass it to that function. So the same level of type checking happens with abstract data types (sometimes called __user-defined types__) as with built-in types.

## Header file etiquette

When you create a `struct` containing member functions, you are creating a new data type. In general, you want this type to be easily accessible to yourself and others. In addition, you want to separate the interface (declaration) from the implementation (definition of member functions) so that implementation can be changed without forcing a re-compile of the entire system. You achieve this end by putting the declaration for your new type ina header file.

In C++, the use of header files is virtually mandatory for easy program development, and you put very specific information in them: declarations. You can use a library even if you only possess the header file along with the object/library file, you don't need the source code for the cpp file.

The __header is a contract__ between you and the user of your library. The contract describes your data structures, and states the arguments and return values for the function calls. The __compiler enforces the contract__ by requiring you to declare all structures and functions before they are used and, in the case of member functions, before they are defined.

### Multiple-declaration problem

When you put a struct declaration in a header file, it is possible for the file to be included more than once in a complicated program. Iostreams are a good example. If the `cpp` file you are working on uses more than one kind of struct (typically including a header file for each one), you run the risk of including the `<iostream>` header more than once and re-declaraing iostreams.

The compiler considers the redeclarations of a structure (both for `structs` and `classes`) to be an error. To prevent this error when multiple header files are included, you need to build some intelligence into your header files using the preprocessor (standard C++ header files like `<iostream>` already have this "intelligence")

```c++
#ifndef HEADER_FLAG
#define HEADER_FLAG
// declaration here ...
#endif
```

### Namespaces in headers

_Using directives_ (i.e, `using namespace std`) is virtually never seen in a header file (at least, not outside of a scope). The reason is that the using directive eliminates the protection of that particular namespace, and the effect lasts until the end of the current compilation unit. If you put a _using directive_ (outside of a scope) in a header file, it means that this loss of "namespace protection" will occur with any file that includes this header, which often means other header files. Thus, if you start putting _using directives_ in header files, it's very easy to end up "turning off" namespaces practically everywhere, and thereby neutralizing the beneficial effects of namespaces.

## Nested structures

The convenience of taking data and function names out of the global name space extends to structure. You can nest a structure within another structure, and therefore keep associated elements together.

```c++
//: C04:Stack.h
// Nested struct in linked list
#ifndef STACK_H
#define STACK_H
struct Stack {
  struct Link {
    void* data;
    Link* next;
    void initialize(void* dat, Link* nxt);
  }* head;
  void initialize();
  void push(void* dat);
  void* peek();
  void* pop();
  void cleanup();
};
#endif // STACK_H ///:~
```

## Global scope resolution

The scope resolution operator gets you out of situations in which the name the compiler chooses by default (the "nearest" name) isn't what you want. For example, suppose you have a structure with a local identifier `a`, and you want to select a global identifier `a` from inside a member function. The compiler would default to choosing the local one, so you must tell it to do otherwise.

```c++
//: C04:Scoperes.cpp
// Global scope resolution
int a;

void f() {}

struct S {
  int a;
  void f();
};

void S::f() {
  ::f(); // Would be recursive otherwise!
  ::a++; // Select the global a
  a--; // The a at struct scope
}

int main() { S s; f(); } ///:~
```
