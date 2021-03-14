# Intialization & Cleanup

A large segment of C bugs occur when the programmer forgets to initialzie or clean up a variable. In C++, the concept of initialization and cleanup is essential for easy library use and to eliminate the many subtle bugs that occur when the client programmer forgets to perform these activities.

## TOC

* Guaranteed initialization with the constructor
* Guaranteed cleanup with the destructuor

## Guaranteed initialization with the constructor

In C++, initialization is too important to leave to the client programmer. The class designer can guarantee initialization of every object by providing a special function called the `constructor`. If a class has a constructor, the compiler automatically calls that constructor at the point an object is created, before client programmers can get their hands on the object.

The name of the constructor is the same as the name of the class.

```c++
class X {
  int i;
  public:
    X(); // Constructor
};
```

## Guaranteed cleanup with the destructuor

With libraries, just "letting go" of an object once you're done with it is not safe. What if it modifies some piece of hardware, or puts something on the screen, or allocates storage on the heap? If you just forget about it, your object never achieves closure upon its exit from this world. In C++, cleanup is as important as initializsation and is therefore guaranteed with the `destructor`.

The `destructor` is distinguished from the constructor by a leading tilde `~`. In addition, the `destructor` never has any arguments because destruction never needs any options.

```c++
class Y {
  public:
    ~Y();
}
```
