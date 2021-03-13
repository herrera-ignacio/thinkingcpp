# The C in C++

## TOC

* Creating functions
* Using C function library
* Creating libraries with the librarian
* Controlling execution
* Operators
  * Precedence
* Data Types
  * Built in
  * Specifiers
* Introduction to pointers
* Introduction to C++ references
* Pointers and references as modifiers
* Scoping
* Specifying storage allocation
  * Global
  * Static
  * Extern
  * Linkage
  * Constants
  * Volatile
* Operators
  * Assignment
  * Mathematical operators
  * Preprocessor macros
  * Relational operators
  * Logical operators
  * Bitwise operators
  * Shift operators
  * Unary operators
  * Ternary operator
  * Casting operators
  * Explicit casts (`static_cast`, `const_cast`, `reinterpret_cast`, `dynamic_cast`)
  * `sizeof`
* Composite type creation
  * `typedef`
  * `struct`
  * `enum`
  * `union`
  * `arrays`
* Make: managing separate compilation

## Creating functions

Standard C and C++ use a feature called __function prototyping__. With function prototyping, you must use a description of the types of arguments when declaring and defining a function. This description is the "prototype".

When the function is called, the compiler uses the prototype to ensure that the proper arguments are passed in and the return value is treated correctly.

```c++
int translate(float x, float y, float z);
int translate(float, float, float);
```

When you don't know how many arguments or what type of arguments you will have, you can use a __variable argument list__.  This "uncertain argument list" is represented by _ellipses_ (`...`).

## Using C function library

All functions in your local C function library are available while you are programming in C++.

Use the `#include` directive to include the header file containing the function prototype. Now you can call the function in the same way as you would do in C. The linker searchs the Standard Library by default, so that's all you need to do.

## Creating libraries with the librarian

Each librarian has its own commands, but the general idea is that if you fant to create a library, make a header file containing the function prototypes for all the functions in your library. Put this header file somewhere in the preprocessor's search path, either in local directory (so it can be found by `#include "header"`) or in the include directory (so it can be found by `#include <header>`). Now take all the object modules and hand them to the librarian along with a name of the finished library (most librarians require a common extension, such as `.lib` or `.a`). Place the finished library where the other libraries reside so the linker can find it.

When you use your library, you will have to add something to the command line so the linker knows to search library for the functions you call. This vary from system to system.

## Controlling execution

C++ uses all C's execution statements. These include `if-else`, `while`, `do-while`, `for`, and `switch`. C++ also allows the infamous `goto`.

### True and falsx = 47
&x = 0065FE00
p = 0065FE00Pointers Pointers and references as modifiers

There's 

### `break` and `continue` keywords.

Inside the body of any looping constructs, you can control the flow of the loop using `break` and `continue` keywords.

`break` quits the loop without executing the rest of the statements in the loop.

`continue` stops the execution of the current interation and goes back to the beginning of the loop to begin a new iteration.

### Using and misusing `goto`

`goto` is often dismissed as poor programming style, and most of the time it is. Anytime you use `goto`, look at your code and see if there's another way to do it. On rare ocassions, you may discover `goto` can solve a problem that can't be solved otherwise.

```c++
//: C03:gotoKeyword.cpp
// The infamous goto is supported in C++
#include <iostream>
using namespace std;

int main() {
  long val = 0;

  for(int i = 1; i < 1000; i++) {
    for(int j = 1; j < 100; j += 10) {
      val = i * j;
      if(val > 47000)
        goto bottom;
        // Break would only go to the outer 'for'
    }
  }

  bottom: // A label
  cout << val << endl;
```

The alternative would be to set a `Boolean` that is tested in the outer `for` loop and then do a break from the inner for loop. However, if you have several levels of `for` or `while` this could get awkward.

## Operators

You can think of operators as a special type of function (_operator overloading_ treats operators precisely that way). An operator takes one or more arguments and produces a new value.

### Precedence

Operator precedence defines the order in which an expression evaluates when several different operators are present.

The easist to remember is that multiplication and division happen before addition and substraction. After that, if an expression isn't transparent to you, it probably won't be for anyone reading the code, so you should use parentheses to make the order of evaluation explicit.

## Data Types

_Data Types_ define the way you use storage (memory) in the programs you write. By specifying a data type, you tell the compiler how to create a particular piece of storage, and also how to manipulate that storage.

Data types can be built-in or abstract. A built-in data type is one that the compiler intrinsically understands, one that is wired directly into the compiler. In contrast, a user-defined data type is one that you or another programmer create as a class. These are commonly referred as abstract data types (ADT). The compiler knows how to handle built-in types when it starts up, it "learns" how to handle ADT by reading header files containing class declarations.

### Basic built-in types

C and C++ have __four basic built-in data types__, described here for binary-based machines.

* `char` is for character storage and uses a minimum of 8 bits (one byte) of storage, altough it may be larger.

* `int` stores integral number and uses a minimum of 2 bytes of storage.

* `float` and `double` types store floating-point numbers, usually in IEEE floating-point format.
  * `float` for single precision floating point.
  * `double` for double precision floating point.


#### bool, true, & false

Standard C++ `bool` type can have two states expressed by built-in constants `true` (which converts to an integral one) and `false` (which converts to an integral zero). All three names are keywords.

In addition, some language elements have been adapted:

* `&& || !`: take `bool` arguments and produce `bool` results.
* `< > <= >= == !=`: produce `bool` results.
* `if, for, while, do`: conditional expressions convert to `bool` values.
* `?:` first operand converts to `bool` value.

#### Specifiers

Specifiers modify the meanings of the basic built-in types and expand them to a much larger set. There are four specifiers: `long`, `short`, `signed` and `unsigned`.

`long` and `short` modify the maximum and minimum values that a data type will hold. A plain `int` must be at least the size of a `short`. The size hierarchy for integral types is `short int`, `int`, `long int`. All the sizes could conveivably be the same, as long as they satisfy the minimum/maximum requirements.

The size hierarchy for floating point numbers is: `float`, `double` and `long double`. There are no short floating-point numbers.

The `signed` and `unsigned` specifiers tell the compiler how to use the sign bit with integral types and characters (floating-point numbers always contain a sign). An `unsigned` number does not keep track of the sign and thus has an extra bit available, so it can store positive numbers twice as large as the positive numbers that can be stored in a `signed` number. `signed` is the defualt and is only necessary with `char`, which may or may not default to `signed`. By specifying `signed char`, you force the sign bit to be used.

## Introduction to pointers

Whenever you run a program, it is first loaded into computer's memory. Thus, all elements of your program are located somewhere in memory, which is typically laid out as a sequential series of memory locations (the size of each space depends on the architecture of the particular machine and is usually called the machine's _word size_). Each space can be uniquely distinguished from all other spaces by its _address_.

Every element of your program has an address.

The `&` operator in C and C++ __will tell you the address of an element__. All you do is precede the identifier name with `&`, and it will produce the address of that identifier.

You can store addresses inside other variables for later use. C and C++ have a special type of variable that holds an address, called a __pointer__.

The operator that defines a pointer is `*`. The compiler knows that it isn't a multiplication because of the context in which it is used.

When you define a pointer, you must specify the type of variable it points to.

```c++
int* ip; // points to an int variable
```

One difference from other data types, is that for example with int you can do:

```c++
int a, b, c
```

But the following expression for pointers is invalid:

```c++
int* ipa, ipb, ipc;
```

In this case, only `ipa` is a pointer, but `ipb` and `ipc` are ordinary ints.

### Why?

Why do you want to modify one variable using another variable as a proxy?

1. To change "outside objects" from within a function. This is perhaps the most basic use of pointers.

2. To achieve many other clever programming techniques.

#### Modifying the outside object

Ordinarily, when you pass an argument to a function, a copy of that argument is made inside the function, this is referred to as __pass-by-value__.

Let's look at the following example:

```c++
//: C03:PassByValue.cpp
#include <iostream>
using namespace std;

void f(int a) {
  cout << "a = " << a << endl;
  a = 5;
  cout << "a = " << a << endl;
}

int main() {
  int x = 47;
  cout << "x = " << x << endl;
  f(x);
  cout << "x = " << x << endl;
} ///:~
```

In `f()`, `a` is a _local variable_, so it exists only for the duration of the function call to `f()`. Because it's a function argument, the value of `a` is intialized by the arguments that are passed when the function is called. In `main()`, the argument is `x`, which has a value of `47`, so this value is copied into `a` when `f()` is called.

When you run the program you'll see:

```
x = 47
a = 47
a = 5
x = 47
```

When you're inside `f()`, `x` is the _outside objcet_, and changing the local variable does not affect the outside object, since they are two separate locations in storage. This is where pointers come in handy. If we pass a _pointer_ into a function instead of an ordinary value, we are actually passing an alias to the outside object, enabling the function to modify that outside object.

```c++
//: C03:PassAddress.cpp
#include <iostream>
using namespace std;

void f(int* p) {
  cout << "p = " << p << endl;
  cout << "*p = " << *p << endl;
  *p = 5;
  cout << "p = " << p << endl;
}

int main() {
  int x = 47;
  cout << "x = " << x << endl;
  cout << "&x = " << &x << endl;
  f(&x);
  cout << "x = " << x << endl;
} ///:~
```

Now the output is:

```
x = 47You can se
C++ adds an additional way to pass an address into a function. This is __pass-by-reference__.

The basic idea is the same as with pointers. The difference between references and pointers is that _calling_ a function that takes references is cleaner, syntactically, than calling a function that takes pointers.

```c++
//: C03:PassReference.cpp
#include <iostream>
using namespace std;

void f(int& r) {
  cout << "r = " << r << endl;
  cout << "&r = " << &r << endl;
  r = 5;
  cout << "r = " << r << endl;
}

int main() {
  int x = 47;
  cout << "x = " << x << endl;
  cout << "&x = " << &x << endl;
  f(x); // Looks like pass-by-value,
  // is actually pass by reference
  cout << "x = " << x << endl;
} ///:~
```

And the output is:

```
x = 47
&x = 0065FE00
r = 47
&r = 0065FE00
r = 5
x = 5
```

Even though this looks like an ordinary pass-by-value, the effect of the reference is that it actually takes the address and passes it in, rather than making a copy of the value.

So you can see that pass-by-reference allows a function to modify the outside object, just like passing a pointer does (you can also observe that the reference obscures the fact that an address is being passed).

## Scoping

Scoping rules tell you where a variable is valid, where it is created, and where it gets destroyed. The scope of a variable extends from the point where it is defined to the first closing brace that matches the closest opening brance before the variable was defined. That is, a scope is defined by its "nearest" set of braces.

A variable can be used only when inside its scope. Scopes can be nested, indicated by matched pairs of brances inside other marched pairs of braces. Nesting means that you can access a variable in a scope that encloses the scope you are in.

## Specifying storage allocation

### Global variables  for(int x = 0; x < 10; x++)
  func();al variable lasts until the program ends.

If the existence of a global variable in one file is declared using the `extern` keyword in another file, the data is available for use by the second file.

### Local variables

Local variables occur within a scope, they are "local" to a function. Often called _automatic_ because they automatically come into being when the scope is entered and automatically go away when the scope closes. The keyword `auto` makes this explicit, but local variables default to `auto` so it is never necessary to declare something as `auto`.

### Register variables

`register` keyword tells the compiler "Make accesses to this variable as fast as possible". Increasing the access speed is implementation dependent, but, as the name suggests, it is often done by placing the variable ina re gister. It is a hint to the compiler but not a guarantee.

There are restrictions. You cannot take or compute the address of a `register` variable. A `register` variable can be declared only within a block. You can, hwoever, use a `register` variable as a formal argument in a function.

In general, you shouldn't try to second-guess the compiler's optimizer, since it will probably do a better job that you can. Thus, the `register` keyword is best avoided.

### Static variables

`static` keyword has several distinct meanings. Normally, variables defined local to a function disappear at the end of the function scope. When you cann the function again, storage for the variables is created as new and the values are re-initialized. If you want a value to be extant throughout the lif of a program, you can define a function's local variable to be `static` and give it an initial value. The initialization is performed only the first time the function is called, and the data retains its value between function calls. This way, a function can "remember" some piece of information between function calls.

The beauty of a `static` variable is that it is unavailable outside the scope of the function, so it can't be inadvertently changed.

```c++
//: C03:Static.cpp
// Using a static variable in a function
#include <iostream>
using namespace std;

void func() {
  static int i = 0;
  cout << "i = " << ++i << endl;
}

int main() {
  for(int x = 0; x < 10; x++)
    func();
} ///:~
```

### Extern variables

`extern` keyword tells the compiler that a variable or a function exists, even if the compiler hasn't yet seen it in the file currently being compiled. This variable/function may be defined in another file or further down in the current file.

### Linkage

In an executing program, an identifier is represented by storage in memory that holds a variable or a compiled function body. Linkage describes the storage as it is seen by the linker. There are two types of linkage: _internal linkage_ and _external linkage_.

Internal linkage means that storage is created to represent the identifier only for the file being compiled. Other files may use the same identifier name with internal linkage, or for a global variable, and no conflicts will be found by the linker. Internal linkage is specified by the keyword `static` in C and C++.

External linkage means that a single piece of storage is created to represent te identifier for all files being compiled. The storage is created once, and the linker must resolve all other references to that storage.e Global variables and function names have external linkage. These are accessed from other files by declaring them with the keyword `extern`. Variables defined outside all functions (with the exception of `const` in C++) and function definitions defualt to external linkage. You can specificially force them to have internal linkage using the `static` keyword. You can explicitly state than an identifier has external linkage by defining it with `extern` keyword.

Automatic (local) variables exist only temporarily, on the stack, while a function is being called. The linker doesn't know about automatic variables, and so these have no linkage.

### Constants

C++ introduces the concept of a named constant that is just like a variable, except that its value cannot be changed. The modifier `const` tells the compiler that a name represents a constant.

A `const` has a scope, just like a regular variable, so you can "hide" it inside a function.

In C++, a `const` must always have an initialization value.


### Volatile

Whereas the qualifier `const` tells the compiler "This never changes" (which allows the compiler to perform extra optimizations), the qualifier `volatile` tells the compiler "You never know when this will change" and prevents the compiler from performing any optimizations based on the stability of that variable.

## Operators

https://en.wikipedia.org/wiki/Operators_in_C_and_C%2B%2B

## Composite Type Creation

C and C++ provide tools that allow you to compose more sophisticated data types from the fundamental data types. The most important of these is `struct`, which is the foundation for `class` in C++. However, the simplest way is simply to alias a name to another name via `typedef`.

### `typedef`

```c++
typedef existing-type-description alias-name

typedef unsigned long ulong; // now you can use ulong
```

### `struct`

`struct` is a way to collect a group of variables into a structure, and then you can create many instances of this "new" type of variable.

```c++
struct Structure1 {
  char c;
  int i;
  float f;
  double d;
};

int main() {
  struct Structure s1, s2;
  s1.c = 'a'
  // ...
}
```

#### Using typedef with struct

```c++
typedef struct {
  /// ...
} MyStructure;
```

### `enum`

An enumarted data type is a way of attaching names to numers, thereby giving more meaning to anyone reading the code.

```c++
enum ShapeType {
  circle,
  square,
  rectangle
};

int main() {
  ShapeType shape = circle;
  // ...
  switch(shape) {
    case circle: /* circle stuff */ break;
  }
}

enum ShapeTypeV2 {
  circle = 10,
  square = 20,
  rectangle = 50
}
```

### `union`

Sometimes a program will handle different types of data using the same variable.

A `union` piles all the data into a single space. It figures out the amount of space necessary for the largest item you've put in the `union` and makes that the size of the `union`. Use a `union` to save memory.

```c++
//: C03:Union.cpp
// The size and simple use of a union
#include <iostream>
using namespace std;

union Packed { // Declaration similar to a class
  char i;
  short j;
  int k;
  long l;
  float f;
  double d;
  // The union will be the size of a
  // double, since that's the largest element
}; // Semicolon ends a union, like a struct

int main() {
  cout << "sizeof(Packed) = "
  << sizeof(Packed) << endl;
  Packed x;
  x.i = 'c';
  cout << x.i << endl;
  x.d = 3.14159;
  cout << x.d << endl;
} ///:~
```

### Arrays

Arrays allow you to clump a lot of variables together, one right after the other, under a single identifier name.

Array access is extremly fast. However, if you index fast the end of the array, there is no safety net. The other drawback is that you must define the size of the array at compile time.

The C++ `vector` provides an array-like object that automatically resizes itself, so it is usually a much better solution if your array size cannot be known at compile time.

#### Pointers and arrays

The identifier of an array is unlike the identifiers for odinary variables. When you give the name of an array (`a` or `&a` yield the same result), without square brackets, you get the starting address of the array. So one way to look at the array identifier is as a read-only pointer to the beginning of the array.

And although we can't change the array identifier to point somewhere else, we _can_ create a another pointer and use that to move around in the array. In fact, the square-bracket syntax works with regular pointers as well.

```c++
//: C03:PointersAndBrackets.cpp
int main() {
  int a[10];
  int* ip = a;
  for(int i = 0; i < 10; i++)
    ip[i] = i * 10;
} ///:~
```

The fact that naming an array produces its starting address turns out to be quite important when you want to pass an array to a function. If you declare an array as a function argument, what you're really declaring is a pointer.

The following two functions have the same argument lists:

```c++
void func1(int a[], int size) {
  for(int i = 0; i < size; i++)
  a[i] = i * i - i;
}
void func2(int* a, int size) {
  for(int i = 0; i < size; i++)
  a[i] = i * i + i;
}
```

## Make: managing separate compilation

When using _separate compilation_, you need some way to automatically compile each file and to tell the linker to build all the pieces into an executable file. Most compilers allow you to do this with a single command-line statement.

```
g++ SourceFile1.cpp SourceFile2.cpp
```

The problem with this approach is that the compiler will first compile each individual file, regardless of whether that file needs to be rebuilt or not. With many files in a project, it can become prohibitive to recompile everything if you've changed only a single file.

The solution to this problem, developed on Unix but available everywhere in some form, is a program called __make__. The __make__ utility manages all the individual files in a project by following the instructions in a text called a __makefile__. When you edit some of the files in a project and type `make`, the __make__ program follows the guidelines the _makefile_ to compare the dates of the source code files to the dates on the corresponding target files, and if a source code file date is more recent than its target file, __make__ invokes the compiler on the source code file. __make__ only recompiles the source code files that were changed, and any other source-code files that are affected by the modified files. This way, you don't have to re-compile all the files in your project every time you make a change, nor do you have to check to see that everything was built properly. The _makefile_ contains all the commands to put your project together.

### Make activities

When you type `make`, the _make_ program looks in the current directory for a file named _makefile,. This file lists dependencies between source code files. _make_ looks at the dates on files and if a dependent file has an older date than a file it depends on, _make_ executes the _rule_ given after the dependency.

As a simple example:

```makefile
# A comment
hello.exe: hello.cpp
  mycompiler hello.cpp
```

This says that `hello.exe` (target) depends on `hello.cpp`. When `hello.cpp` has a newer date than `hello.exe`, _make_ executes the "rule" `mycompiler hello.cpp". The rules are not restricted to being calls to the compiler, you can call any program you want from within _make_.

### Macros

A _makefile_ may contain __macros__. Those allow convenient string replacement.

```makefile
CPP = g++
hello.exe: hello.cpp
  $(CPP) hello.cpp
```

### Suffix Rules

It becomes tedious to tell _make_ how to invoke the compiler for every single cpp file in your project if it is the same basic process each time. You can abbreviate actions, as long as they depend on file name suffixes.

Suffix rule is the way to teach _make_ how to convert a file with one type of extension into a file with another type of extension. After this, you only need to tell _make_ which files depend on which other files.

The suffix rule tells _make_ that it doesn't need explicit rules to build everything, but instead it can figure out how to build things based on their file extension. In the following example, it says "To build a file that ends in `.exe` from one that ends in `.cpp`, invoke the following command":

```makefile
CPP = mycompiler
.SUFFIXES: .exe. cpp
.cpp.exe:
  $(CPP) $<
```

### Default targets

After the macros and suffix rules, _make_ looks for the first "target" ina file, and builds that, unless you specify differently.

```makefile
CPP = mycompiler
.SUFFIXES: .exe. cpp
.cpp.exe:
  $(CPP) $<

target1.exe:
target2.exe:
```

If you just type `make`, then `target1.exe` will be built (using the default suffix rule). To build `target2.exe` you'd have to explicitly say `make target2.exe`. This becomes tedious, so you normally create a default "dummy" target that depends on all the rest of the targets:

```makefile
CPP = mycompiler
.SUFFIXES: .exe. cpp
.cpp.exe:
  $(CPP) $<

all: target1.exe target2.exe
```

In addition, you can have other non-default target lists that do other things, for example, you could set it up so that typing `make debug` rebuilds all your files with debugging wired in.
