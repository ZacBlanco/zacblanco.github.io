---
tile: Systems Programming (CS214)
layout: page
keywords: systems, programming, cs, computer, science, C, functional, programming, andrew, tjang, rutgers, scarlet, knights
permalink: /education/systems-programming/
---


## Rutgers Computer Science: Systems Programming

Sometimes better know as "Systems" or "CS214". This course aims to teach students (but not limited to):

- how to work with somewhat larger codebases
- Use unix systems
- Use version control
- The C programming language
  - Memory management in C
- Work with groups on a larger project



## Introduction to C
---------------------

_(For Java programmers)_

The main thing that we need realize going from Java to C is that there is no such thing as memory management in C. As a result C has constructs that allow us to manually manage pointers and memory that we'll dive into shortly.

In Java when we want to *dynamically* create objects based on inputs or specific conditions, Java usually takes care of allocating the necessary space to allow us to create out objects on the Memory heap. We don't have to worry about how or where our memory is stored.

In C, things are a bit different. When we want to create an object we need to worry about how much memory that object is going to take up. We also need to figure out when to free up that meory to the system such that we avoid memory leaks.

Now, first let's talk about **pointers**.

A pointer value is a value which points to a location (also typically called an **address**) where a certain value is stored and memory.

Typically for every value we want to store, there will be a corresponding address, or pointer that goes along with our value.

One easy way to think of this is that every variable you want to store has a _left value_ and a _right value_.

```c
int c = 10;
```

We can attempt to represent this value by the following table

| Left Value (Pointer/Address) | Right Value (Value) |
| `0x127a004` (random memory location) | `10` |


Now in C we use the `*` character in order to access the left and right value of a specific variable.

See the code below for an example of accessing a pointer from a value;

```c
#include <stdio.h>
int main() {

        int c = 10; //c == 10
        int* c2 = &c; //c2 == 0xXXXXXXXX (Memory Location)
        int c3 = *c2; // c3 == 10; (Retrieves the value at the pointer)

        printf("Integer (Right Value): %i\n", c);
        printf("Pointer Location (Left Value): %p\n", c2);
        printf("Integer Value (From Pointer): %i\n", c3);
}
```

**Wait, what is the `&` doing??!!1**

The `&` is called **dereferencing** a variable. This takes the value stored in `int c`, and then returns the **memory address** of `int c`.

### Typedef and Structs

C doesn't really have any idea what an object is. We can't define things with functions that belong to them. We also don't have constructors to initialize a bunch of values. Nor do we have a _new_ keyword.

What C _does have_ is this idea of **structs**.

**Structs** are simple structures used to define chunks of data. They're kind of like a Java class, just without any methods or constructors.

C also has the special `typedef` where we can define a name to call our structs by.

Example:

```c
int main() {
  //Example of a struct named BNode.
  typedef struct {

    char* data;
    struct BNode* prev;
    struct BNode* next;

  } BNode;

  BNode myNode;
  myNode.data = NULL;

  char a = 'a';
  char* a2 = &a;

  BNode* mn2 = (BNode*) malloc(sizeof(BNode));;
  mn2->data = a2;

  printf("Val: %s\n", myNode.data);
  printf("Val2: %s\n", mn2->data);
}
```

We can see from the sample above how to access structs data based on whether or not our struct variable is pointer to the struct or a representation of the struct.

Confused about **malloc**? Don't worry that's covered next.



### Malloc and Free

Unlike Java, C doesn't have any memory management so we have to allocate and free memory as we need it. Both functions are important to the structure of a program

The functions `malloc` and `free` are used to allocate and free memory for the program to use.

`malloc` is part of `<stdlib.h>` which will need to be included in your program. What malloc does is assign a number of bytes for your program to use to store new objects.

We call this type of memory **dynamic** because `malloc` only needs to be used when you don't know how many variables need to be created. Inevitably, most programs will need to create variable dynamically.

This is cool because you can preserver and use certain variables or blocks of memory for the entire life of the program. C gives you tons of control.

...but with great power comes great responsibility...

The because malloc lets our program allocate tons of dynamic memory, it can be very easy to write a program that uses all of the available system memory.

But a good software engineer will know that you should probably try to free your memory at a reasonable times, or at least when your program doesn't need the space anymore.

An issue where a program allocates memory but forgets to free it is called a **memory leak**.

To free memory you can use the `free` function on whichever pointer that you are done using.

This will make a system call that let's your computer's operating system know that you're done using the memory space and that it can use that memory for any other program on the system.


#### A Macro called Square

```c
#define square(x) ((x)*(x))
```


For a more in depth intro to C guide please see the the [Intro-to-C-in-C guide which can be found on GitHub]()





























