# Hiding the implementation

* Setting limits
* C++ access control (`public`, `private`, and `protected`)
* Friends
* The class

## Setting limits

In a C `struct`, as with most things in C, there are no rules. Client programmers can do anything they want.

There are two reasons for controlling access to members:

1. Keep client programmer's hands off trools they shouldn't touch, tools that are necessary for the internal machinations of the data type, but not part of the interface the client programmer needs to solve their particular problem.

2. Allow library designer to change internal workings of the structure without worrying about how it will affect the client programmer.

## C++ access control specifiers

C++ introduces three new keywords (_access specifiers_) to set the boundaries in a structure: `public`, `private` and `protected`. They are used only ina structure declaration, and they change the boundary for all the declarations that follow them.

* `public` means all member declarations that follow are available to everyone.

* `private` means that no one can access that member except you, the creator of the type, inside function members of that type.

* `protected` acts just like private, with the exception that "_inherited_" structures are granted access to _protected_ members.

## Friends

You can declare a function as `friend` _inside_ the structure declaration to explicitly grant access to a function that isn't a member of the current structure.

## The class

Access control is often referred to as __implementation hiding__. Including functions within structures (often referred to as __encapsulation__) produces a data type with characteristics and behaviors, but access control puts boundaries within that data type, for two important reasons.

1. Establish what the client programmers can and can't use.

2. Separate interface from implementation.

The use of `class` in C++ comes close to being an unnecessary keyword. It's identical to the `struct` keyword in absolutely every way except one: `class` defaults to `private`, whereas `struct` defaults to `public`.

The `class` is the fundamental OOP concept in C++.
