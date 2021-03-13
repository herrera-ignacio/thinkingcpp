# Introduction to Objects

Object-oriented model provides tools for the programmer to represent elments in the problem space. This representation is general enough that the programmer is not constrained to any particular type of problem. We refer to the __elements in the problem space__ and their __representations in the solution space__ as "__objects__".

## TOC

* Smalltalk characteristics example
* Class
* Interface
* Implementation boundaries
* Access specifiers
* Reusing implementation
  * Composition & Aggregation
  * Inheritance
  * Polymorphism
* Creating and destroying objects
  * Stack
  * Heap
  * C++ and Garbage Collection
* Exception handling
* Analysis and Design
  * Five design phases
  * Five stages of object design
  * Extreme Programming (WIP)


## Smalltalk characteristics example

Alan Kay summarized five basic characteristics of _Smalltalk_, the first successful object-oriented langage ad one of the languages upon which C++ is based.

1. __Everything is an object__.

2. __A program is a bunch of objects telling each other what to do by sending messages__.

3. __Each object has its own memory made up of other objects__.

4. __Every object has a type__.

5. __All objects of a particular type can receive the same message__.

## Class

Objects that are identical except for their state during a program's execution are grouped together into "_classes of objects_".

Creating __Abstract Data Types__ (classes) is a fundamental concept in object-oriented programming. ADTs work almost exactly like built-in types: you can create variables of a type (_objects_ or _instances_) and manipulate those variables. The type defines the instance's characteristics and behaviors.

One of the challenges of object-oriented programming is to create a one-to-one mapping between the elements in the problem space and objects in the solution space.

## Interface

Each object can satisfy only certain requests. The requests you can make of an object are defined by its __interface__, and the __type is what determines the interface__.

The interface establishes _what_ requests you can make for a particular object. However, there must be code somewhere to satisfy that request. This, along with the hidden data, comprises the __implementation__.

## Implementation boundaries

It is helpful to break up the playing field into __class creators__ (those who create new data types) and __client programmers__ (class consumers who use the data types in their applications).

The goal of the _client programmer_ is to collect a toolbox full of classes to use for rapid application development.

The goal of the _class creator_ is to build a class that exposes only what's necessary to the lcient programmer and keeps everything else hidden. This way, the class creator can change the hidden portion at will without worrying about the impact to anyone else.

### Access Specifiers

C++ uses three explicit keywords to set the boundaries in a class:

* `public`: available to everyone.

* `private`: no one can access those definitions except from inside member functions of that type.

* `protected`: acts like private with the exception that an inheriting class has access to protected members.

## Reusing implementation

### Composition and Aggregation.

Simplest way to reuse a class is to just use an object of that class directly, but you can also place an object of that class inside a new class ("_creating a member object_"). Your new class can be made up of any number and type of other objects, in any combination that you need. This concept is called __composition__.

Composition is often referred to as a "_has-a_" relationship, as in "a house has a room". Composition implies a relationship where the child cannot exist independent of the parent.

__Aggregation__ implies a relationship where the child can exist independently of the parent, as in "a teacher has students".

This comes with a great deal of flexibilit because __you can change member objects at runtime, to dynamically change the behavior of your program__.

### Inheritance
arbage Collector
> In the shape example, functions manipulate generic shapes without respect to whether they're circles, squares, triangles, and so on. All shapes can be drawn, erased, and moved, so these functions simply send a message to a shape object. They don't worry about how the object copes with the message.

Such code is unaffected by the addition of new types, and adding new types is the most common way to extend an object-oriented program to handle new situtations.

There problem with attempting to treat derived-type objects as their generic base types, is that the compiler cannot know at compile-time precisely what piece of code will be executed. That's the whole point, when the message is sent, the programmer doesn't want to know what piece of code will be executed. If you don't have to know what piece of code will be executed, then when you add a new subtype, the code it executes can be different without requiring changes to he function call.

> The draw function ca be applied equally to a circle, a square, or a triangle, and the object will execute the proper code depending on its specific type.

#### Polymorphism at compiler level

The compiler cannot make a function call in the traditional sense.

The function call generated by a non-OOP compiler causes what is called __early binding__, meaning that the compiler generates a call to a specific function name, and the linker resolves this call to the absolute address of the code to be executed.

In OOP, the program cannot determine the address of the code until runtime, so it uses the concept of __late binding__. When you send a message to an object, the code being called isn't determined until runtime. The compiler does ensure that the function exists and performs type checking on the arguments and return valuie (unless it is _weakly typed_), but it doesn't know the exact code to execute.

#### Polymorphism in C++

To perform late binding, the C++ compiler inserts a special bit of code in lieu of the absolute code. This code calculates the address of the function body, using information stored in the object. Thus, each object can behave differently according to the contents of that special bit of code. When you send a message to an object, the object actually does figure out what to do with that message.

You state that you want a function to have the flexibility of late-binding properties using the keyword `virtual`. You need to remember to add the `virtual` keyword because, by default, member functions are _not_ dynamically bound. Virtual functions allow you to express the differences in behavior of classes in the same family. Those differences in are what cause polymorphic behavior.

## Creating and destroying objects

Different programming languages use different philosophies regarding where the data for an object is and how its liftetime is controlled.

C++ takes the approach that control of efficiency is the most important issue, so it gives the programmer a choice.

### Stack

The stack is an area in memory that is used directly by the microprocessor to store data during program execution. Variables on the stack are sometimes called __automatic__ or __scoped__ variables. The static storage area is simply a fixed patch of memory that is allocated before the program begins to run.

Using the stack or static storage area places a priority on the speed of storage allocation and release (for maximum runtime speed). However, you sacrifice flexibility because you must know the exact quantity, lifetime, and type of objects _while_ you're writing the program.

If you create an object on the stack or in static storage, the __compiler determines how long the object lasts and can automatically destroy it__.

### Heap

The second approach is to create objects dynamically in a pool of memory called the _heap_.

In this approach you don't know until runtine how many objects you need, what their lifetime is, or what their exact type is. Those decisions are made at the spur of the moment while the program is running. If you need a new object, you simply make it on the heap when you need it, using the `new` keyword. When you're finished with the storage, you must release it using the `delete` keyword.

Because the storage is managed dynamically at runtime, the amount of time required to allocate storage on the heap is significantly longer than the time to create storage on the stack.

> Creating storage on the stack is often a single microprocessor instruction to move the stack pointer down, and another to move it back up.

The dynamic approach makes the generally logical assumption that objects tend to be complicated, so the __extra overhead of finding storage and releasing that storage__ will not have an important impact on the creation of an object. In addition, the __greater flexibility__ is essential to solve general programming problems.

if you create an object on the heap, __the compiler has no knowledge of its lifetime__.

### C++ and Garbage Collection

In C++, the programmer must determine programmatically when to destroy the object, and then perform the destruction using the `delete` keyword. As an alternative, the environment can provide a feature called a __garbage collector__ that automatically discovers when an object is no longer in use and destroys it.

Of course, writing programs using a garbage collector is much more convenient, but it requires that all applications must be able to tolerate the existence of the garbage collector and the __overhead for garbage collection__. This does not meet the design requirements for the C++ language and so __it was not included__, although third-party garbage collectors exist for C++.


## Exception Handling

_Exception handling_ wires error handling directly into the programming language and sometimes even the operating system. __An _Exception_ is an object that is "_thrown_" from the site of error and can be "_caught_" by an appropriate _Exception Handler_.__

In addition, a thrown exception is unlike an error value that's returned from a function or a flag that's set in order to indicate an error condition (_these can be ignored_). An exception __cannot be ignored__ so it's guarenteed to be dealt with at some point.

Finally, exceptions provide a __way to recover reliably__ from a bad situation. Instead of just exiting the program, you are often able to set things right and restore the execution of a program, which produces more robust programs.

> It's worth noting that exception handling isn't an object-oriented feature, altough in object-oriented languages the exception is normally represented with an object.

## Analysis and design

### Five design phases

0. __Make a plan__
  * Mission statement / high concept

1. __What are we making?__
  * Equivalent of _procedural design_: "creating the _requirements analyis_ and _system specification_".
  * Who will use this system?
  * What can those actors do with the system?
  * How does this actor do that with the system?
  * How else might this work if someone else were doing this, or if the same actor had a different objective? (to reveal variatons)
  * What probles might happen whiel doing this with the sytstem? (to reveal exceptions) 
  * Try to discover a full set of use cases for your system: use cases produce requirements specifications by determining all interactions that the user may have with the system.

2. __How will we build it?__
  * Design that describes what classes look like and how they will interact.
  * _Class-Responsibility-Collaboration (CRC) cards_. (Name, responsibilities and collaborations of classes)
  * Create a formal description of your design using UML diagrams.
  * __Object design__.

3. __Build the core__
  * Initial conversion from rough design into a compiling and executing body of code that can be tested and especially that will prove or disprove your architecture.
  * This is not a one-pass process, but rather the beginning of a series of steps that will iteratively build the system.
  * Goal is to find the core of your system architecture that needs to be implemented in order to generate a running system.

4. __Iterate the use cases__
  * Each feature set you add is a small project in itself.
  * The basis of each iteration is a single uise case.
  * A __use case__ is a package of related functionality that you build into the system all at once.
  * You stop iterating when you achieve target functionalkity or an external deadline.
  * Iterative development helps revealing and resolving critical risks early, customers have ample opportunity to change their minds, and project can be steered with more precision.
  * Additional feedback for stakeholders, reducing need for status meetings and increasing confidence and support.

5. __Evolution__ (maintenance)
  * Program goes from good to great.
  * Issues that you didn't really understand in the first pass become clear.
  * Classes can evolve from single-project usage to reusable resources.


### Five stages of object design

Understanding what an object does and what it should look like happens over time.

1. __Object discovery__
  * Initial analysis of a program, looking for external factors and boundaries, duplication of elements in the system, and smallest conceptual units.

2. __Object assembly__
  * Discover that you need tnew members that didn't appear during discovery. The internal needs of the object may require other classes to support it.

3. __System construction__
  * As you learn, you evolve your objects.
  * The need for communication and interconnection with other objects in the system may change the needs of your classes or require new ones.

4. __System extension__
  * As you add new features to a system, you may discover that your previous design doesn't support easy system extension.
  * Reestructure parts of the system.
  
5. __Object reuse__
  * Real stress test for a class.
  * Change a class to adapt to more new programs, class will become clearer until you have a truly reusable type.
  * Don't expect most objects from a system design to be reusable.

### Extreme Programming (XP)

> You can find it chronicled in _Extreme Programming Explained_ by Kent Beck and on the web _www.xprogramming.com_.

XP is both a philosophy about programming work and a set of guidelines to do it. Some of these guidelines are reflected in other recent methodologies. The two most important and distinct contributions are "__write test first__", and "__pair programming__".

#### Write tests first

XP gives equal (or even greater) priority than the code. In fact, you write the tests _before_ you write the code that's being tested. The tests must be executed successfully every time you do an integration of the project.

Writing tests first has two extremly important effects:

1. Forces a clear definition of the interface of a class. The tests are a contract that is enforced by the compiler and the running program.

2. Running tests every time you do a build gives you immediate feedback if you do something wrong. Tests become an extension of the safety net provided by the language.

#### Pair Programming

XP says that code should be written with two people per workstation. And that this should be done in an area with a group of workstations.

The value of pair programming is that one person is actually doing the coding while the other is thinking about it. The thinker keeps the big picture in mind, not only the picture of the problem at hand, but the guidelines of XP. If two people are working, it's less likely that one of them will get away saying, "_I don't want to write tests first_", for example. And if the coder gets stuck, they can swap places. If both of them get stuck, their musings may be overheard by someone else in the work area who can contribute. Working in pairs __keps things flowing and on track__.
