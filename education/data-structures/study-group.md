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

## Outline

- [Session 1 - Big-O and Linked Lists](#session-1)
- [Session 2 - Linked Lists, Stacks, Queues, Generics and Exceptions](#session-2)

<div id="session-1"> </div>

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

<div id="session-2"> </div>

## Session 2 - Common Elements, Linked Lists, Stacks, Queues, Generics and Exceptions

### Merging Common Elements of Two Sorted Linked Lists

This comes from problem number 6 of problem set 2.

Imagine 2 sorted linked lists

    1st List: [2] -> [4] -> [8]
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]

Imagine we want to create a list of all the common elements.

- **The naive approach**: Simply loop over the first list and loop over all of the items of the 2nd list. Compare each to find the common nodes. This forces us to get $$O(nm)$$ runtime.
  - This takes a long amount of time and we can certainly improve upon our algorithm. However this might be the simplest to implement.

**The better approach**: Let's use the fact that the lists are pre-sorted to our advantage.

We know that if we're looking for an item _from the first list_ and _within the 2nd list_ that if we find an item in the 2nd list which is greater than the item we're looking for, then we can immediately stop our search for that item because if we didn't find it already we know that it won't exist later on in the list due to the fact that the lists are sorted.

So let's take our lists and look for the value `2`. We use a single pointer on each list.

    1st LOOP
    1st List: [2] -> [4] -> [8]
               ^ 
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
               ^
    2 is not equal to 1. Increment the 2nd list pointer.

    
    2nd LOOP
    1st List: [2] -> [4] -> [8]
               ^ 
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                      ^
    3 is NOT equal to 2, BUT 3 is greater than 2, so we increment the list 1 pointer because we now know that 2 cannot exist in list 2 due to the sorted order property.

Okay so now we get the idea so let's try to look for `4`. We increment the list 1 pointer from the last iteration.

    3rd LOOP
    1st List: [2] -> [4] -> [8]
                      ^ 
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                      ^
    We see 3 is not equal to 4. 3 is also less than 4. Increment list 2 pointer.

    4th LOOP
    1st List: [2] -> [4] -> [8]
                      ^ 
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                             ^
    We see that 4 and 4 ARE EQUAL. This means we add 4 to our new linked list. We also increment both pointers since we found a match.

    5th LOOP
    1st List: [2] -> [4] -> [8]
                             ^
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                                    ^
    6 and 8 are not equal. 6 is still less than 8. Increment list 2 pointer.

    6th LOOP
    1st List: [2] -> [4] -> [8]
                             ^
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                                           ^
    We see that 8 and 8 ARE EQUAL. Add 8 to the new common elements list. Increment both pointers.

    7th LOOP
    1st List: [2] -> [4] -> [8]
                                  ^
    2nd List: [1] -> [3] -> [4] -> [6] -> [8] -> [16]
                                                  ^
    We find that list 1 pointer is set to null, so we can't possibly have any more matches. break out of the loop and return the list of common elements.


So that's a basic example of running through the algorithm. The runtime is $$O(m + n)$$ which is far, far better than $$O(mn)$$. I encourage you to try to write to code to solve this problem. It is very similar to a merge on two sorted arrays that is done in mergesort.

    


### Linked List Review

We're going to now assume you have a basic understanding of pointers and the operations on linked lists.

As a quick review

- **Pointer**: A variable which doesn't contain a primitive value like an `int`, `double`, or `float`, but rather a number which represents a location in a computer's main memory.
- **Node**: An item within a linked list that contains data and a pointer to the next item in the list.
- **Linked List**: An object which contains a node that points to the head of the linked list. Operations that may be performed:
  - **Add**: Adds an item to the linked list
  - **Delete**: Deletes an item from the linked list
  - **Search/Exists**: Returns whether or not an item exists
  - **Traverse**: Loops over all elements within the list by using pointers.

There are more but those are probably the most basic.

### Stacks

Stacks are probably the first _real_ data structure that you encounter in. Linked lists aren't inherently that useful unless you utilize the properties of a stack (or queue).

So to understand the properties of a stack, first imagine a stack of cafeteria trays. A cafeteria will constantly be cleaning these trays and placing them on top of one another. The customers (or students) who use the cafeteria will need trays. When they go to the stack of clean trays to grab one for their food, typically the first tray to be taken will be the one on the top of all of the other trays. The one below that will then be the top tray. It will be taken, then the next one and so on.

So as you can imagine as trays get cleaned and put on top the first ones to be taken will be the last trays to be put on the _stack_. This is exactly how the **stack** operates. It has the property of **LIFO** (Last-In First-Out). This means that the most recent item to be put on a stack is the first one to be put on.

Stacks can be implemented as linked lists. They really only contain two or three new operations which aren't that hard to implement. They are:

- `push()`
- `pop()`
- `peek()`

### **`push()`**

The push operations adds a new item to the *stack*. This not only adds the new item but changes the head as well. It's pretty much exactly the same as the $$O(1)$$ add that we did before for normal linked lists.

### **`pop()`**

Slightly more advanced but still incredibly simple, the `pop()` method will remove an item from the stack. However it doesn't just remove any item. It will remove whatever item is currently at the head of the linked list. So imagine the following stack:

```
5   <-- HEAD
4
77
82
4
0

pop() ---> returns 5
// The stack now becomes:
4   <-- HEAD
77
82
4
0

pop() --> returns 4
77   <-- HEAD
82
4
0
```

### **`peek()`**

`peek()` will return the same element as pop(), however instead of removing it from the stack, all of the items remain in tact. The stack is not modified but the node value at the top of the stack will be returned. Example:

```
1   <-- HEAD
2
3
4
5

peek() --> returns 1
// Stack is
1
2
3
4
5 
```

So essentially a **stack** is a data structure which can be implemented as a linked list that has the **LIFO property** described above. The methods `push`, `pop`, and `peek` are all available and satisfy the LIFO behavior.


### Queues!

*Fun Fact*: Queue is one of the few words in the English language that has 4 vowels in a row.

A queue is another type of data structure which is very similar to the stack in the regard that it can be implemented as a linked list. However instead of having `push()` and `pop()` we have `enqueue()` and `dequeue()`

The difference between a stack and a queue is that **a stack has the LIFO property** and **a queue has the FIFO property**. FIFO stands for First-In First-Out. You can find an example of queues every single day whenever you visit a store or stand in a line. Every time you get in line at the grocery store you are part of a queue! Think about it. The first person to get in line to check out at a grocery store is always going to be the first one to get out of that line. As more people get in they have to wait while the people in front of them check out. This satisfies the property of FIFO which is why it is an example of a queue.

### `enqueue()`

The `enqueue()` operation of a queue is similar to a stack's push. You can enqueue the new items on the the front of list and simply just change the lists front pointer. It is is also possible to implement the queue operations (as well as stacks) with circular or doubly linked lists, however the actual implementation will vary depending on the type of underlying list structure.

### `dequeue()`

The `dequeue()` operation is analogous to the stack's `pop()`. It will return the next item to be deleted from the queue. In a null terminated queue where we always add to the front, this operation takes $$O(n)$$ time because we have to traverse the list all the way to the end to delete and return the last item. 

You might think that it might be able to be fast if you use a circular linked list but the problem of reassigning the `rear` pointer still requires us to traverse the entire list again to find what the 2nd to last item is so we still have $$O(n)$$ run time to dequeue.

### `peek()`

The `peek()` operation in the queue performs the same type of action as in the stack, except that instead of simply returning the first item we return the last, or the item that would have been dequeued had `dequeue()` been called. This operation takes $$O(n)$$ on a null-terminated linked list but actually only requires $$O(1)$$ on circular linked list.


### Generics

Generics are a special construct of the Java programming language that allows you to make object-agnostic classes that you can pass parameters to when instantiating the object. If that just sounded like a bunch of jargon don't worry I'll explain in more lay terms.

Basically when you have a linked list class, you might want to store different types of data in each node. Maybe in one list you want `int` while in the other one you want a `float`, a `double`, or maybe even a `boolean`. But if we didn't have generics you would need to create a new class with the same method implementations for every type! Now obviously that's just silly, take lots of time and effort, and is simply not good programming practice. Copy and pasting code is never a good idea.

So the developers of Java language came up with the idea of **generics** these allow you to define classes where you are allowed to pass different objects as parameters and use them as data.

Take the following linked list node class as an example:

```
public class Node<T extends Comparable<T>>  {
  
  public static void main(String[] args) {
    System.out.println("hello world");
    
    Node<Integer> a = new Node<Integer>(12, null);
    Node<Integer> b = new Node<Integer>(15, null);
    Node<Integer> c = new Node<Integer>(12, null);
    System.out.println(a.equals(b));
    System.out.println(a.equals(c));
    
    Node<Double> d = new Node<Double>(12.0, null);
    Node<Integer> e = new Node<Integer>(15, null);
    Node<Integer> f = new Node<Integer>(12, null);
    System.out.println(d.equals(e));
    System.out.println(d.equals(f));
  }

  // By having 'extends Comparable<T>` means we can use the compareTo method
  public T data;
  public Node<T> next;
  public Node(T data, Node<T> next) {
    this.data = data;
    this.next = next;
  }

  public boolean equals(Node<T> other) {
    return this.data.equals(other.data);
  }

}
```


So now when you define a new `Node` object you can specify the type and utilize it within other types of objects without having to rewrite the methods.

### Exceptions

Exceptions are special types of errors. Exceptions are used in order to notify a user about the reason why a program may have crashed or when an invalid instruction was being attempted to be executed. Two examples of common exceptions which might occur are `NullPointerException` this arises from trying to perform operations on a pointer which has a null value. Another example of an exception that you might see is `ArithmeticException` which is thrown when something like dividing by zero occurs. It is an operation that would normally just shut down the program and cause it to end. Another one might be `IndexOutOfBoundsException` when accessing array indices

However language developers are smart and we know that shutting down the program immediately upon an error is not really the best thing to do so they invented this `try-catch` statement in order to catch exceptions and attempt to handle them.

See below for an example

```
public static void main(String[] args) {


//Linear search with try/catch

int[] nums = {1, 4, 9, 16, 25, 36, 49, 64, 81, 100, 121, 144, 169, 196, 225}

int ind = 0
//search for 255
try {
  while (nums[ind] != 255) { //eventually causes index out of bounds
    nums++; 
  }

} catch (IndexOutOfBoundsException e) {
  ind = -1; // Handle by doing some specified behavior
}
//Continue program.
//...
}
```






