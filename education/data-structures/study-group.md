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
  - tmp = tmp.next;

```
    public static void Main(String[] args) {
      LLNode head = LLNode(0, null); // This is a variable which *points* to an LLNode
      head = addNode(12, head);
      head = addNode(15, head);

      //The list now contains three items
      traverse(head);
      //Traversal result: 15 --> 12 --> 0 
    }

    public static LLNode addNode(int data, LLNode head) {
      LLNode new = new LLNode(12, head);
      return new;
    }

    public static void traverse(LLNode head) {
      LLnode tmp = head;
      while (tmp != null) { 
        System.out.print(tmp.data + " --> ");
        tmp = tmp.next;
      }
      System.out.println( "\\" ); //Adds newline and prints '\'
    }
```

Because we have a conditional check inside of a while loop, the runtime for a traversal of a linked list is going to be proportial to the number of items in the list. If we assume the list to be of size $$n$$, then the Big-O runtime of a traversal will be $$O(n)$$.

**Searching in a Linked List**

Fortunately, searching in a linked list is very similar to traversal. Instead of printing out items in the list on each iteration we must check whether or not the item we are currently pointing to with `tmp` is holds the data we're searching for.

A simple search function might look something like

```
//Returns true/false if an item existed in the linked list.
public static Boolean search(int searchData, LLNode head) {
  LLNode tmp = head;
  while (tmp != null) {
    if (tmp.data == searchData) {
      return true;
    }
    tmp = tmp.next;
  }
  return false; //False if we complete the loop
}
```

This is where big-O analysis can become trickier. If we look at the best case scenario, where the item we search for appears *at the very beginning of the list* then the total runtime we can estimate to be $$1$$.

If the item appears in the, say, 14th place in the list, then the total runtime is $$14$$. 

In big-O we must always consider the worst possible scenario in terms of runtime. For searching a list this means we assume the wrost case to be when we have to compare every item in the list to our search parameter. In the case of an arbitrarily sized list of $$n$$ items, the search becomes $$O(n)$$.

**Deleting in a Linked List**

Deleting an item in a linked list is probably going to be the most complex operation that we will cover so far. This is due to the fact that not only does it incorporate the traversal/search from before, but also ends up requiring us to move and change pointers in order to remove an item from the list.

The basic steps to deleting a node in a linked list are:

- Perform a search on the linked list to find the location of the item
- Get pointer of previous node
  - Take set pointer of previous node equal to target node -> next

However, now that we are modying our data structure (the linked list), we need to account for special cases. These are cases which we need to think about in order to write good code. This ensures that your methods will not result in NullPointerExceptions or fail to perform the specified task correctly. Identifying these cases is the first thing that you should think about before writing code.

In the case of deleting a node we have to consider:

- The case where the head is null (empty list)
- The case where the item to be deleted is the first item in the list
- General case, deleting any item after the head. 

The code for this isn't terribly complicated but it builds on the topics covered earlier. Also note that the algorithm that I am describing only deletes the first occurrence of the item. It does not account for duplicates.

    //Returns the new head of the list
    public static LLNode delete(int data, LLNode head) {
      if (head == null) {
        return head;
      } else if (head.data == data) {
        return head.next; //This removes the pointer to head and will end up being cleaned up by the garbage collector.
      }

      //General case
      LLNode curr = head.next; //head is definitely not null.
      LLNode prev = head;
      while( curr != null ) {
        if (curr.data == data) {
          prev.next = curr.next; //Forces prev pointer to curr.next (removes reference to curr)
          break;
        }
        prev = curr; //The order of these instructions is important.
        curr = curr.next;
      }
      return head;

    }

To analyze the runtime we see that we still have to perform a search over the list. In the worst case this results in us having to traverse over the entire list before deleting an item which gives us $$O(n)$$.

**Singular Linked Lists v.s. Circular Linked List**

So far I have only explored singular linked lists in which we mark the end of the list by having the last node in the list have a `next` pointer of `null`.

However this leaves a pointer's worth of information empty that could be utilized by the system to possibly make operations more efficient. A circularly linked list allows us to utilize that pointer memory by pointing it back to the end (or beginning) of the list.

In one implementation of a circularly linked list instead of having a `head` pointer, might replace that with a pointer named `rear` which will always point to the last item in the list. The `rear.next` field of the `rear` pointer would then pointer to the beginning of the list, or the head (but without using an explicit `head` pointe).

In this way, with a single pointer we are able to find the head **and** tail of a list.

- Head ==> `rear.next`
- Tail ==> `rear`

This is quite a simple modification, but it does modify the code that we would use for a traversal. One option to do a traversal of the list is shown below:

    public static void traverse(LLNode rear) {
      LLNode tmp = rear.next; //Actual head

      do {
        System.out.println(curr.data + " --> ");
        tmp = tmp.next;
      } while (tmp != rear.next); //Stop when we get to the front again.
      // Remember there's no null pointers so NPE isn't an issue
    }

This is just one way of  traversing over the list. The runtime of this is no different than that of the normal linked list. However by using a circular linked list this allows us to have $$O(1)$$ when adding to the list at the front **and** the back of the list. 

With a non-circular linked list the time to append to the rear of the list is $$O(n)$$ and $$O(1)$$ for the front. In a circular linked list the time to append to the front and rear is simply $$O(1)$$. This is useful for stacks and queue's which we'll explore soon. 



