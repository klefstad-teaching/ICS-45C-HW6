# ICS 45C Homework 6

Class inheritance with a Picture of 2D Shapes

## Getting the assignment

1. Accept the assignment by clicking the link in Canvas
2. Once you accept the invite, you will reach a page that says "You're ready to go"
3. Click the URL from that page that looks similar to this: `https://github.com/klefstad-teaching/ics-45c-hw6-<YourGitHubUsername>`. It may take a bit of time for the repo to be ready.
4. Click the green `<> Code` Button on the top right, and then click the middle tab `SSH`
5. Copy that link
6. Go to your hub and go into the ICS45C folder, and open up the terminal and type in the following command:
```bash
git clone git@github.com:klefstad-teaching/ics-45c-hw6-<YourGitHubUserName>.git HW6
```
7. Go to VSCode and open up the `HW6` Folder
## Directory Structure

```bash
├── CMakeLists.txt
├── CMakePresets.json
├── gtest
│   ├── gtestmain.cpp
│   ├── picture_gtests.cpp
│   ├── shape_gtests.cpp
└── src
    ├── alloc.cpp
    ├── alloc.hpp
    ├── circle.cpp
    ├── circle.hpp
    ├── picture.cpp
    ├── picture.hpp
    ├── rectangle.cpp
    ├── rectangle.hpp
    ├── shape.cpp
    ├── shape.hpp
    ├── square.cpp
    ├── square.hpp
    ├── standard_main.cpp
    ├── triangle.cpp
    └── triangle.hpp
```

## Overview and Objectives

Homework 6 uses the C++ concepts of **class hierarchy** with **inheritance** and **polymorphism** implemented by **virtual functions**, to build a class hierarchy of 2D geometric `Shapes` collected to form a `Picture`, which is a **linked list** of `Shape` pointers. The base class named `Shape` is extended by derived classes `Circle`, `Rectangle`, and `Triangle`. Base class `Shape` contains virtual functions to print the names of the `Shapes`, calculate their areas, and draw them with simple character graphics. Each derived class inherits these virtual functions from the base class `Shape` and **overrides** them as needed to calculate the area and (for example, to calculate the area with the correct function for its specific shape).

Next, a class `Picture` holds a linked list of pointers to `Shapes`. Class `Picture` has functions to add new `Shapes` to the list, print their names, draw them, and calculate the sum of their total area.

**Relation to Course Objectives:  Implementing class inheritance; gaining proficiency with linked lists**

Homework 6 demonstrates the object-oriented programming principles of class inheritance and virtual functions to build a modular, reusable, and extensible framework, by defining a base class and deriving subclasses from it, using inheritance and polymorphism.

Good design of the virtual functions, with code that can handle objects of different types in a uniform manner, achieves **polymorphism**, allowing objects of different classes to be treated as if they were of the same type. You also learn how to use **dynamic binding** through these virtual functions, to ensure that the correct function is called at runtime based on the dynamic type of object being referenced.

Homework 6 uses a linked list, for additional practice to gain proficiency in using them. Because `Shapes` must be kept in the same order that they are added to a picture, they must be added to the **end** of the list. A `tail` pointer is needed to allow efficient  addition of new elements to the end of the linked list.

Homework 6 gives more practice in memory management, in the key principle of programming in C++ of remembering ***who owns the storage that eventually must be freed***. Thinking is required to implement correct constructors, assignment, and destructors.

## Organization of Classes and Files

> Be sure to use **exactly** the names given in these instructions and supplied on this repo for **files**, **functions**, and **classes**, because the autograder will be expecting exactly those names.

Homework 6 implements a class hierarchy with

- a base class `Shape`
- 3 classes derived directly from `Shape`:  class `Circle`, class `Rectangle`, and class `Triangle`
- class `Square` derived from the concrete class `Rectangle`

Class `Picture` contains a collection of `Shapes`, stored in a linked list implemented using struct `ListNode`.

Each Shape class is written in its own `.hpp/.cpp` files, provided in this repo.

> ⚠️ Be sure to use include guards (also known as [header guards](https://www.learncpp.com/cpp-tutorial/header-guards/)) in all header files that you submit! Use the format  "`NAME_HPP`" so, for example, `square.hpp` should be "`SQUARE_HPP`".


![class hierarchy chart](/assets/0-1.png)


## 1 Base Class Shape

Define an abstract base class `Shape` in the files `shape.hpp` and `shape.cpp`, with the data members and functions given in the `shape.hpp` screenshot below.

    **Do not give default parameters to the constructor parameters!**

shape.hpp screenshot

![shape.hpp screenshot](/assets/1-1.png)

## 2 Derived classes

## 2.1 Classes derived from Shape:  Circle, Rectangle, Triangle

Three classes, `Circle`, `Rectangle`, and `Triangle`, are derived directly from the abstract base class Shape. These derived classes are made concrete by providing implementations for all the pure virtual functions introduced in your abstract base class `Shape`, in the files `circle.hpp/.cpp`, `rectangle.hpp/.cpp`, and `triangle.hpp/.cpp`.

- Each `.hpp` file containing a specific shape must `#include` the `.hpp` file for the class it is immediately derived from.  
- Each `.cpp` file must `#include` its own `.hpp` file, as well as `<iostream>`.
   - `circle.cpp` also `#include <numbers>`, for `pi`
   - `triangle.cpp` also `#include <algorithm>` for `std::max`

For example, class `Triangle` is declared in file `triangle.hpp` which `#include shape.hpp`.  Class `Triangle` methods are defined in file `triangle.cpp` which `#include triangle.hpp`. (Generally, don’t `#include .cpp` files, and don’t directly compile `.hpp` files. Instead, directly compile `.cpp` files and link them together to create executable files.)

Each of these derived classes has the data members and functions similar to those shown in the screenshot below for class Circle, but with these data members specific to its own dimensions:

 - `Circle:  radius`
 - `Rectangle:  width, height` ( width is drawn horizontally, and height vertically, and **order matters** in the drawing algorithm)
 - `Triangle:  base, height`
 - `Square:  side` (derived from `Rectangle`, so use `width` and `height`)

Methods for the `area()` for each derived class must be implemented correctly, using the appropriate calculation for the area of a Circle vs Rectangle vs Triangle.

< Note: For `Circle`, use the value `std::numbers::pi` as an approximation for ***𝞹***.        

circle.hpp screenshot

![circle.hpp screenshot](/assets/2-1-1.png)

Methods for `draw()` are provided in the screenshots below. Please use them so that your output matches the autograder’s expectations.

> **Be sure to reproduce the single space in the drawing algorithms.**

Circle draw() screenshot

![Circle draw() screenshot](/assets/2-1-2.png)

Rectangle draw() screenshot

![Rectangle draw() screenshot](/assets/2-1-3.png)

Triangle draw() screenshot

![Triangle draw() screenshot](/assets/2-1-4.png)

## 2.2 Class derived from Rectangle: Square

Derive a fourth class, `Square`, from `Rectangle`, in the files `square.hpp/.cpp`.

Because `Square` is derived from a concrete class, rather than an abstract base class, it can inherit method definitions. We want **`class Square`** to inherit definitions of `area()` and `draw()` from `Rectangle`—which means you must not define an `area()` or `draw()` method in class `Square`.

`Square`’s constructor should take only the parameters:  `center`,  `name`, and a `side`. It must call the `Rectangle` constructor, passing in the appropriate parameters.

## 3 class Picture and struct ListNode

## 3.1 Class Picture

Class `Picture`, in the files `picture.hpp/.cpp`,  is implemented as a linked list of `Shape` pointers. `picture.hpp` only `#includes shape.hpp`; it knows nothing about any other kinds of `Shapes`, like `Circle`, `Rectangle`, `Triangle`, or `Square`.

Like Homework 5’s class `String`, class `Picture` has a data member named `head`, which is a pointer to a `ListNode`. Add another data member named `tail`, which is also a `ListNode` pointer, so that you can efficiently add a new `Shape` to the end of the linked list, while keeping the `Shapes` in the same order in which they were added.

![Example of Picture Linked List](/assets/3-1-1.png)

Class Picture has the following methods:

 - A default constructor that creates an empty linked list
 - `add(const Shape& shape)`
 - `print_all(ostream& out)const`
 - **Polymorphic** methods that operate on a `Picture`:
    - `void draw_all(ostream& out)` draws the `Shapes` in this `Picture` in the order that the `Shapes` were added to this `Picture`
    - `double total_area() const` returns the sum of areas of all the `Shapes` in this `Picture`
 - The destructor must clean up your `Picture` when it dies, freeing all memory that it owns
 - `swap(Picture& other)` swaps data in this picture with `other`
 - valid copy and move constructor
 - valid copy and move assignment

## 3.2 struct ListNode

This linked list implementation will have far fewer helper functions than in Homework 5, so `ListNode` here will be a `struct` nested in the private part of class `Picture`.

`ListNode` has the data members `Shape* shape` and `ListNode* next`,  both `public`. You may add a few static helper functions as necessary. Since `ListNode` is a plain data struct, you can create and initialize a new ListNode similar to how we did in Homework 5 like this:  

     `ListNode* p = new ListNode{new_shape_ptr, new_next};`


picture.hpp screenshot

![picture.hpp screenshot](/assets/3-1-2.png)

## 4 standard_main.cpp

A `main()` function in a file named `standard_main.cpp`

 - declares a `Picture`,
 - adds a bunch of `Shapes` to it,
 - prints them all **in the order they were added**,
 - draws them all in the order they were added, and
 - prints their total area.

See [Steps of Development](#steps-of-development) next for the incremental approach to develop this program. Some important information may be found only in the Steps.

Screenshot of standard_main.cpp

![Screenshot of standard_main.cpp](/assets/3-1-3.png)


## Steps of Development

***Hofstadter’s Law: It always takes longer than you expect, even when you take into account Hofstadter's Law.***

![Hofstadters law graph](/assets/4-1.png)

First, build a framework composed of class `Shape` and class `Picture`. Populate the framework with **only one** derived class to start. When you get that subset of functionality working correctly, then extend the framework with the rest of the specific shape classes, **one at a time**.

### Build a framework with class Picture, abstract base class Shape, and one concrete derived class Circle

1. Write a first draft of class `Shape` with constructor, destructor, data members, and `print()`. As usual, postpone writing any destructors until the final steps.
2. Write a first draft of class `Picture`, with
    a. nested struct `ListNode`,
    b. the data members `head` and `tail`,
    c. the constructor,
    d. `add()`, and
    e. `print_all()`.
3. Write a first draft of class `Circle` derived from `Shape`
    a. Add a data member `radius` of type `int`.
    b. Implement the constructor.
    c. Inherit the `print()` method from `Shape`.
    d. Define the copy constructor for `Circle` to be the default, but keep it `protected`.
    e. Write a definition of `clone()` that returns a new `Circle` copy constructed from this `Circle`.
    f. Be sure to make class `Circle` concrete, which means giving definitions for all virtual functions that are not fully implemented yet. These include `draw()` and `area()`. For this first draft, have `draw()` do nothing and have `area()` return `0.0`.
4. Write a first draft of `main()` in `standard_main.cpp` following the screenshot, but comment out all the shapes except for `Circle` and comment out the lines of code that make calls to methods you have not yet defined, such as `draw_all()` and `total_area()`:
    a. Declare a `Picture`, named collage
    b. add a `Circle` to the `Picture`,
    c. call `print_all()` on the `Picture`.
5. Get all this working, using the sanitizers and/or `valgrind` regularly to help detect memory errors.  At this stage, ignore memory leaks, but fix any other memory errors.
6. Add the method `draw_all()` to `Picture` and the method `draw()` to `Circle`. Extend `main()` to call `draw_all()`. Test it, get it working.
7. Add `total_area()` to `Picture` and `area()` to `Circle`. Extend `main()` to call `total_area()`. Test it, get it all working.
    `Total area = 1256.64`  **// example for format only, NOT value**

## Extend the framework with more derived classes, one at a time

8. Now fully implement the next derived class, `Rectangle`. (Notice that `Rectangle` has two dimensions, `width` and `height`, where `width` is drawn horizontally, and `height` vertically, and **order matters** in the drawing algorithm.) Test it, get it all working.
9. Repeat Step 8 for class `Triangle`.
10. Now, derive`Square` from class `Rectangle`, inheriting its methods.
11. Modify the `print()` method in class `Shape` to also print the area of this `Shape` in the format below. Notice the effect of this one change!
    `Circle_1 at (0, 0) area = 12.5664` **// example for format only, not value**
12. Modify `print_all()` to also call `draw()` for this Shape, after it calls `print()`.
13. Finally, write all necessary destructors. As usual, cross your fingers and pray. 🙏 At this stage, you must ensure you have no memory leaks and no other memory errors. **Remember, whoever owns the storage must eventually delete it.**
    
Screenshot of sample output of standard_main

![Screenshot of sample output of standard_main](/assets/4-2.png)

## How to Submit and Grade the programs

In **GradeScope** for **Homework 6**, submit from GitHub the files detailed above:
 - alloc.hpp
 - alloc.cpp
 - shape.hpp
 - shape.cpp
 - circle.hpp
 - circle.cpp
 - triangle.hpp
 - triangle.cpp
 - rectangle.hpp
 - rectangle.cpp
 - square.hpp
 - square.cpp
 - picture.hpp
 - picture.cpp
 - standard_main.cpp

> ⚠️ **Be sure your `#includes` and `using` match exactly what is specified**, for the autograder to run your submission with our own test programs. **Be sure you have added include guards on every `.hpp` file.**

## Grading criteria

The autograder’s test program will check that areas and total area are computed correctly, and that `Shapes` are drawn correctly and in the right order.

Points are allotted for
 - Preventing memory leaks. Major points are deducted for mismatched new and `delete` and other memory errors.
 - Passing tests of functional correctness.

*We may adjust grades manually when warranted. For example, a submission that attempts to defraud the autograder points by not implementing the requirements may be given a 0.*

## Build Instructions

```bash
# Produce the `build` folder with the presets provided for the homework:
cmake --preset default

# Build all targets at once:
cmake --build build

# Build only standard_main.cpp:
cmake --build build --target picture_main

# Build shape gtests:
cmake --build build --target shape_gtests

# Build picture gtests:
cmake --build build --target picture_gtests
```

To run the above targets after compiling them:

```bash
./build/picture_main         # Runs the 'main' function from src/standard_main.cpp
./build/shape_gtests         # Runs the 'shape' gtests
./build/picture_gtests       # Runs the 'picture' gtests
```

NOTE : If you want to also use valgrind to check your code, use these instructions:

```bash
# Produce the `build_valgrind` folder with the presets provided for the homework:
cmake --preset valgrind

# Build all targets at once:
cmake --build build_valgrind

# Build only picture_main.cpp:
cmake --build build_valgrind --target picture_main

# Build shape gtests:
cmake --build build_valgrind --target shape_gtests

# Build picture gtests:
cmake --build build_valgrind --target picture_gtests

```

Then run the above targets with:

```bash
./build_valgrind/picture_main         # Runs the 'main' function from src/standard_main.cpp
./build_valgrind/shape_gtests         # Runs the 'shape' gtests
./build_valgrind/picture_gtests       # Runs the 'picture' gtests
```

Once you have run the code above and it either produces the output you expected or passes
all provided tests, congratulations! You are now ready to [submit](#submission) your homework!

## Submission

As with previous submissions, submit via `GitHub` by the following steps:

1. `git add .`
2. `git commit -a -m "Commit Message Here"`
3. `git push --set-upstream origin main`

to push your changes to your private GitHub repository, and then submit `hw6` to `Gradescope`.

## Credit

All code moved from [ICS45c](https://github.com/RayKlefstad/ICS45c/tree/hw6)
