---
layout: page
title: Data Structures Study Group
keywords: review, data structures, data, structures, algorithm, exam, review, notes, guide, study, group, session
permalink: /education/data-structures/study-group/
mathjax: true
description: Notes and review materials for my CS 112 study group in the Spring of 2017.
---

You can contact me any time via email at `zac.blanco@rutgers.edu`. I will do my best to respond in a timely manner.

## Useful Links

- [Problem Sets](https://www.cs.rutgers.edu/courses/112/classes/spring_2017_venugopal/probs/)
- [Session Feedback Form](https://goo.gl/forms/dNokwIddk9YOgxZg2)


## Session 1 - Big O and Linked Lists

[*Problem Set 1*](https://www.cs.rutgers.edu/courses/112/classes/spring_2017_venugopal/probs/ps1.html)

In this session we went over

- Guidelines & Expectations
  - Always arrive on time
  - Be prepared to participate

Data structures Topics

**Linked Lists**

A computer requires memory every time you create a variable. Simple variables (**primitives**) are special built-in types that require only small amounts of space (usually architecture specific) when the computer creates them.

However when we create objects like _linked list nodes_ the way the information is stored is not the same. 

If you create a class `LLNode<T>` and try to instantiate and then print the class to the console with `System.out.println` you'll find that the computer will print a bunch of gibberish. That's due to the fact that when you instantiate an object, the object in code is actually **referenced by a memory location**. These references to memory locations are called **pointers**. The numbers and letters that look like gibberish are actually the ASCII values which represent the location in memory that the object resided.

The neat thing is that we can create pointers which reference other pointers within objects that are referenced by pointers themselves!

The most basic linked list node is comprised of 2 basic pieces.

- A data field
- A next pointer

In code, this might look like

    public class LLNode {
      int data;
      LLNode next;
      public LLNode(int data, LLNode next){
        this.data = data;
        this.next = next;
      }
    }

Then you could create a **pointer** to a node by the following:

    public static void Main(String[] args) {
      LLNode ptr1 = LLNode(0, null); // This is a variable which *points* to an LLNode
      LLNode ptr2 = LLNode(10, ptr1); // This LLNode set its "next" variable to be ptr1.

      // ptr2.next == ptr1 ==> True
      // ptr2.next.data == ptr1.data ==> True
    }

A linked list is then essentially just a number of these nodes *linked* together. Hence the name **Linked List**

A linked list object typically needs to somehow point to the beginning or **head** of the list. This is done by having a variable within the list list object called `head`.

**Adding to a Linked List**

The simplest way to add to a linked list is to change the reference of the head node to the new node you are trying to add. Then change the reference of the head pointer to the new node. This ensures no data is lost

To add a node pseudo-code:

- Create the new node.
- Set new node.next to the current head
- Set the head pointer to the new node.

The runtime for this operation is $$O(1)$$ because the the size of inputs (number of items in the list) does not play a factor in the algorithm to add a node.


    public static void Main(String[] args) {
      LLNode head = LLNode(0, null); // This is a variable which *points* to an LLNode
      head = addNode(12, head);
      head = addNode(15, head);

      //The list now contains three items
      //15 --> 12 --> 0 
    }

    public static LLNode addNode(int data, LLNode head) {
      LLNode new = new LLNode(12, head);
      return new;
    }

**Traversing a Linked List**

So now we know how to add a bunch of nodes to our linked list. How do we traverse over a linked list?

It's actually quite simple. In order to traverse a linked list, all we need is a basic for/while/do while loop and an understanding of how the list is structured.

First we need to create a temporary node as being equal to the head of the list. Then we set the temporary node equal to its "next" value, so long as the value of "next" is NOT `null`.

Pseudo-code

- Create temp Node
- temp = head
- while temp != null
  - print node (or whatever operation is desired)


**Searching in a Linked List** (Coming Soon)

**Deleting in a Linked List** (Coming Soon)

**Singular Linked Lists v.s. Circular Linked List** (Coming Soon)



