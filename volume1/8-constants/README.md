# Constants

> The concept of constant, expressed by the `const` keyword, was created to allow the programmer to draw a line between what changes and what doesn't.

## TOC

* Value substitution
* Const pointer
* Function arguments and return values
* Classes
* Mutable: bitwise vs logical const
* ROMability

## Value substitution

The preprocessor can be used to create macros and to substitute values. Because the preprocessor simply does text replacement and has no concept nor facility for type checking, preprocessor value substitution introduces subtle problems that can be avoidad in C++ by using `const` values, which brings value substitution into the domain of the compiler.

```c++
# define BUFSIZE 100
const int bufsize = 100;
```

## Const pointer

```c++
int* const w; // const pointer that points to an integer
int const* v; // pointer that points to a const int
const int* v; // pointer that points to a const int
```

## Function arguments and return values

If you are passing objects _by value_, specifying `const` has no meaning to the client (it means that the passed argument cannot be modified inside the function). If you are returning an object of a user-defined type by value as a `const`, it means the returned value cannot be modified. If you are passing and returning _addresses_, `const` is a promise that the destination of the address will not be changed.

### Passing by const value

You can specify that function arguments are `const` when passing them by value, such as:

```c++
void f1(const int i) {
  i++; // compile-time error
}

void f2(int ic) {
  const int& i = ic;
  i++; // Illegal -- compile-time error
}
```

### Returning by const value

```c++
const int g();
```

You are promising that the original variable (inside the function frame) will not be modified. And again, because you're returning it by value, it's copied so the original value could never be modified via the return value.

At first, this can make the specification of `const` seem meaningless. For built-in types, it doesn't matter, but it becomes important when you're dealing with user-defined types. If a function returns a class object by value as a `const`, the return value of that function cannot be an lvalue (that is, it cannot be assigned to or otherwise modified). Thus, it's important to use `const` when returning an object by value if you want to prevent its use as an lvalue.

### Passing and returning addresses

If you pass or return an address (either a pointer or a reference), it's possible for the client programmer to take it and modify the original value. If you make the pointer or reference a `const`, you prevent this from happening. In fact, whenever you're passing an address into a function, you should make it a `const` if at all possible.

## Classes

You may want to create a local `const` in a class to use inside constant expressions that will be evaluated at compile time. However, the meaning of `const` is different inside classes, so you must understand the options in order to create `const` data members of a class.

You can also make an entire object `const`. But preserving the `const`ness of an object is more complex. he compiler can ensure the `const`ness of a built-in type but it cannot monitor the intricacies of a class. To guarantee this, the `const` member function is introduced: only a `const` member function may be called for a `const` object.

## Mutable" bitwise vs logical const

What if you want to create a `const` member function, but you'd still like to change some of the data in the object? Bitwise const means that every bit in the object is permanent, so a bit image of the object will never change. Logical const means that, although the entire object is conceptually constant, there may be changes on a member-by-member basis.

However, if the compiler is told that an object is `const`, it will jealously guard that object to ensure bitwise constness. To effect logical constness, there are two ways to change a data member from within a const member function:

1. __Casting away constness__: you take `this` and cast it to a pointer to an object of the current type.

2. `mutable` keyword, in the class declaration, to specify that a particular data member may be changed inside a `const` object.

## ROMability

If an object is defined as `const`, it is a candidate to be placed in read-only memory (ROM), which is often an important consideration in embedded systems programming. The object must be bitwise-const rather than logical-const.

## Volatile

The syntax of `volatile` is identical to that for `const`, but `volatile` means "This data may xchange outside the knowledge of the compiler." Somehow, the environment is changing the data (possibly through multitasking, multithreading or interrupts), and `volatile` tells the compiler not to make any assumptions about that data, especially during optimization.
