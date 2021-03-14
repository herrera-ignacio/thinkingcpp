# Function Overloading & Default arguments

Calls `f("hello")`, `f("hi", 1)`, and `f("howdy, 2, "c")` can all be calls to the same function or also be calls to three overloaded functions.

## TOC

* Function overloading
* Default arguments

### Function overloading

The idea of an overloaded function is that you use the same function name,  but different argument lists. Thus, for overloading to work the compiler must decorate the function name with the names of the argument types.

```c++
void print(char);
void print(float);
```

### Default arguments

A default argument is a value given in the declaration that the compiler automatically inserts if you don't provide a value in the function call.

```c++
Stash(int size); // Zero quantity, this is not necessary
Stash(int size, int initQuanitity);
Stash(int size, int initQuantity = 0);
```

