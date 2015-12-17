---
layout: page
title: Data Structures - Final Exam Review
keywords: final, exam, review, data structures, data, structures, algorithm, final, exam, review, notes, guide
permalink: /education/data-structures/final-review/
mathjax: true
description: A collection of notes to reference for the Data Structures (CS112)
---

[**Click here to go back to the Data Structures page**](../)

**This page is currently a work in progress**

------------------------------------------------------------------------

## Binary Trees

## AVL Trees

### Huffman Coding

## Hash Table

## Heaps

## Graphs

Graphs are data structures which we can use to establish connections between different nodes (also called vertices). For example, you could think of a graph as a series of airports. Given an airport named A, we can say that A has many different places one could travel to from A. There are also many other airports that may offer flights which are directed into A. These airports and flights represent the nodes and edges of a graph. Where an airport is a node, and a connecting flight between two airports is an edge

The two basic components of graphs are:

- Vertex
- Edge

A vertex is a point in the graph, whereas an edge is what connects two vertices together

There are two types of graphs:

- Directed
- Undirected

These two types of graphs describe which types of edges are present in our graph.

Undirected graphs are graphs which the connections between to vertices flows in both directions. That is you may use the same edge which connects nodes A and B together to travel back and forth between the two vertices.

Directed graphs are graphs which flow in only one direction. Usually represented with an arrow, these graphs you may only travel away from the node you are currently at if there is an arrow pointing out of the current vertiex to another.

The maximum number of edges in a graph is one where every vertex has edges to the other $$n-1$$ vertices.

The maximum number of distinct edges in a graph is:

> $$ e = \frac{n(n-1)}{2} = \frac{n^2}{2} - \frac{n}{2}$$

There are also two types of edges we may have on a graph:

- Weighted
- Unweighted

Basically an unweighted edge means there is number number or _weight_ (hence the name) associated with the edge connecting to vertices.

In a graph with weighted edges you find that each connection between two vertices has a number associated with it. This can tell us things such as the distance between two points if we see our vertices as airports.


### Storing Graphs

There are two different possible ways to store a graph:

- An adjacency matrix
- Linked lists of neighbors

An adjacency matrix is a two dimensional array of size $$n\times n$$ where depending on the graph type we can mark either `true/false`(unweighted) or a `number`(weighted) wherever we find that there is an edge between two vertices.

Example:

We have an undirected graph with vertices A, B, and C. There is a connections from A to B and B to C

|   | A | B | C | 
|:-:|:-:|:-:|:-:|
| A | 0 | 1 | 0 |
| B | 1 | 0 | 1 |
| C | 0 | 1 | 0 |

So we understand what a graph is, but how can we **traverse** these graphs to find which vertices they are connected to one another? Or how can we determine whether two nodes like within the same portion of a graph?

There are two approaches to this:

- Depth-First
- Breadth-First

Each of the traversals are explained below.

### Depth-First Search (DFS)

A depth-first search is where we follow the edges of a vertex until we have either (or both) of the following conditions are satisfied:

1. The vertex we are currently at has no neighbors or connections
2. We have already visited all of the neighbors of the vertex.

The vertex has then been visited and we "step back" to the previous vertex.


The pseudocode for algorithm is as follows

~~~
dfs(Vertex v):

  if v has not been visited:
    mark v as being visited
    for each of vertex w of v's neigbors:
      if w has not been visited:
        dfs(w)
      endif
    endfor
  endif     
~~~

So DFS works great and is very simple because it is a recursive algorithm. But instead of going to the _deepest vertex_, what if we want to visit all of the "shallow" vertices first?


### Breadth-First Search (BFS)

Breadth first search is interesting because it propagates through a graph like a **wave**. It will traverse vertices in the around the same time for all vertices that are a given distance from the root or origin vertex

See below for the BFS code:

~~~
bfs(vertex v):
  Queue q is a new queue
  
  enqueue(v)
  while our q is not empty:
    vertex c = q.dequeue
    for all neighbor vertices w of c:
      if w has not been visited:
        mark w as visited
        enqueue(w) 
    endfor
  endwhile
~~~

### Topological Sort

Topological sort takes an interesting stab at a graph. It will only apply in case where we have a **directed acyclic graph**

**Acyclic** means that the graph does not cycle back upon itself. In other words: it isn't possible to arrive at a vertex from which you've already visited.

The approach here is that we will use DFS to run a topological sort, but the only catch is that when we arrive at a vertex where we "jump out" of the recursion, we will assign that vertex the greatest topological number, $$n$$.

The code for this is very similar to DFS

~~~
topSort():
  topnum = number of vertices in the graph
  for each vertex v in a graph:
    if v is not visited:
      dfs(v, topnum)
    endif
  endfor
  
dfs(v, topnum):
  mark v as visited
  for each neighbor w of v:
    if w is not visited:
      dfs(w, topnum)
    endif
  endfor
  v has the number topnum
  topnum--
~~~

### Connected Path

The connected path algorithm also uses the **depth-first search** in order to determine if there is a path connecting two different vertices in a single section of an undirected graph.

Because in a graph it is possible to have "islands" (two different parts of the graph that have no common vertices) we can simply determine whether two vertices are part of the same island which tells us whether or not they are connected

We will use an array of size $$n$$ where for each vertex we mark whether or not it is part of the island.

pseudocode:

~~~
int[] islands

driver():
  topnum = 0;
  for vertex v in graph g:
    if v has not yet been visited
      topnum++
      dfs(v, topnum)
    endif
  endfor

dfs(v, islandNum):
  if(v is not yet visited):
    islands[v] = islandNum
    mark v as visited
    for each neighbor w of v:
      dfs(w, islandNum)
    endfor
  endif
~~~

Given the pseudocode above all we have to do to do to determine whether two vertices...say `x` and `z` ...are connected is check whether our `island[x] == island[z]`.

### Dijkstra's Algorithm

Dijkstra's algorithm is interesting because it provides us with a way to to not only determine the shortest path between two vertices. But not simple _just_ because between the two vertices. It finds the shortest path  from one vertex to _all_ other vertices.

This requires that the graph be directed or undirected. But it _must_ also be weighted. This means that the edges between adjacent vertices should have weights to them.

The algorithm itself isn't that complex and may utilize some of the previous structures that we've learned about!

Basically what Dijkstra's algorithm does:

1. Start at our initial vertex. We will mark this vertex as having a distance of 0. We should mark it as **visited**.

2. After **1** so we should add all of the vertices to what we call **the fringe**. This **fringe** is extremely important. The data structure with which we implement the fringe will determine this algorithm's running time

3. After adding all vertices to the fringe, we will then **pick the minimum** vertex from the fringe. The minimum vertex is the vertex which has the shortest distance from the root vertex we picked. This vertex is then removed from the fringe. We will call is vertex, `v`.

4. We then use `v` and check all of `v`'s neighbors. If it's neighbor hasn't been visited yet then we need to **check whether or not we should update its distance in the fringe**. The distance that is stored along with the vertex in the fringe should be the the very smallest possible distance. This means that when we check vertex `v` it's possible to have a smaller distance than what is already stored. So if the new calculated distance is smaller, we need to **search and update** inside of our fringe. We can call this the **check** and **update phase**. 

5. If the fringe is not empty at this point, go back to step **2**.


**So then how can we calculate the running time of this algorithm?**

We're going to break down the basic runtime components of this algorithm.

We have: 

- Adding a vertex to the fringe
- Picking the minimum distance vertex from the fringe
- Checking and updating the distance of neighbors.


For the sake of simplification we're going to take that last point and break it up into two different phases. This leaves us **4** total components to the algorithm that we will use to determine the runtime.

- **Adding a Vertex to the Fringe**
- **Picking the minimum distance vertex from the fringe**
- **Checking the distance of the neighbors**
- **Updating the distance of the neighbors**

All but one of these components depends upon the data structure used in the implementation of **the fringe**. So really it is up to our implementation to determine how fast this algorithm can run.

We're going to look at three different fringe implementations and compare the runtimes for each.

- A linked list
- A sorted linked list
- A min heap

See the table below for the runtime for each phase and fringe implementation


| Big-O Run Time | Linked List | Sorted LL | Min Heap |
|:--------------:|:-----------:|:---------:|:--------:|
| Add to Fringe  | $$O(n)$$    | $$O(n^2)$$|$$O(n\cdot logn)$$|
| Pick Minimum   | $$O(n^2)$$  | $$O(n)$$  |$$O(n\cdot logn)$$|
| Checking Neighbor| $$O(n+e)$$  | $$O(n+e)$$|$$O(n+e)$$|
| Updating Neighbor| $$O(e)$$    | $$O(ne)$$ |$$O(e\cdot logn)$$|


So let's go over the runtimes for each step.

**Adding to the fringe**

For each implementation we must add $$n$$ vertices to the fringe. This leaves us with the insertion time of $$n$$ multiplied by the time it takes to insert into a list.

- Linked List
  - $$O(1) \cdot n $$.
  - Runtime is $$O(n)$$
- Sorted Linked List
  - To insert a single on a sorted linked list of size 0 takes maximum of $$1$$ time. the next item takes a max of 1. The next a max of 2... and so on.
  - Series Sum: $$\Sigma_{i=1}^n i = 1 + 2 + 3 + ... + n$$
  - We find that this sums to $$\frac{n\cdot(n + 1)}{2}$$
  - This gives a runtime of $$O(n^2)$$
- Min Heap
  - Takes a maximum of $$log(1)$$ time to insert the first item, $$log(2)$$ for second, and so on...
  - This will give a series sum of $$\Sigma_{i=1}^n log(i) = log(1) + log(2) + log(3) + ... + log(n) = log(n!)$$
  - $$ log(n!) \approx log(n^n) = n\cdot log(n)$$
  - Thus our runtime is $$On\cdot log(n)$$


**Picking The minimum from the fringe**

- Linked List
  - Here our search is on $$n$$ items. We must perform this search a maximum of $$n$$ times.
  - Runtime is $$ O (n^2) $$
- Sorted Linked List
  - This one is pretty simple. It takes $$O(1)$$ time to find the minimum on the sorted linked list (because it is sorted)
  - We perform the search $$n$$ times on the sorted linked list. 
  - This gives a runtime of  $$O(n)$$.
- Min Heap
  - Takes a maximum of $$log(n)$$ time to delete an item. But we can continuously delete from this heap for $$n$$ times.
  - This will give a series sum of $$\Sigma_{i=n}^0 log(i) = log(n) + log(n-1) + log(n-2) + ... + log(1) = log(n!)$$
  - $$ log(n!) \approx log(n^n) = n\cdot log(n)$$
  - Thus our runtime is $$O(n\cdot log(n))$$
  
**Checking if we need to update**

Surpisingly enough, this part of the algorithm runs indepedent of the fringe implementation because it doesn't actually interact with the fringe.

We do a check at $$n$$ vertices and then we can only check for a total of $$e$$ times. 

From this our runtime is simply just $$ O(n+e) $$.

**Updating the Fringe**

- Linked List
  - Here we update a maximum of $$e$$ times at a speed $$O(1)$$ and the item keeps its spot in the list.
  - Runtime is $$ O(e) $$
- Sorted Linked List
  - Here we must perform the update a maximum number of $$e$$ times. But then after updating it takes $$n$$ time to put into the correct list spot.
  - Runtime is $$O(ne)$$.
- Min Heap
  - Takes a maximum of $$log(n)$$ time to delete an item. But we need to update this heap for $$e$$ times.
  - Thus our runtime is $$O(e\cdot log(n))$$
  
From this we can find our individual runtimes for each fringe implementation!

| Algorithm          | Run Time |
|:------------------:|:--------:|
| Linked List        |$$O(n^2)$$|
| Sorted Linked List |$$O(n^2)$$|
| Min Heap           |$$O((n+e)log(n))$$|


## Sorting

### Mergesort

### Quicksort

### Heapsort

#### Building a Heap in Linear ($$O(n)$$) Run Time

When building a heap within an already existing array. You actually don't need to "heapify" the entire array. You can start with the $$\frac{n}{2} - 1$$ element of the array. ($$n$$ is the size of the array.)

Then to build the heap it is simply:

~~~
build_heap(A)
  x = A.length/2 - 1;
  while (x > 0) {
    v = A[x];
    siftDown(x, v);
    x--;
  }
~~~

This will "heapify" the array. We can then sort.

We simply just have to "remove" from the heap and accordingly sift down each time.

~~~
sort(A) {
  int x = A.length - 1;
  while (x > 0) {
    temp = A[x];
    A[x] = A[0];
    A[0] = temp;
    siftDown(0, temp, x);
    x--;
  }
}
~~~

The run time given that at any level the amount of time it would take to sift down is:

$$ S = 2^0\cdot 2h + 2^1 \cdot 2(h-1) + 2^2 \cdot 2(h-2) +... + 2^{h-2}\cdot 2\cdot 2 + 2^{h-1} \cdot 2 $$

So how do we solve this?

Well, If we multiply S by 2...then simply line up all of the terms (the first and last term of S won't line up)

We find that the total $$S = -2^0\cdot2h + 2\cdot 2^1 + ... + 2\cdot2^{h-1} + 2^h\cdot 2$$

This will sum to give us a value of $$2^{h+2} - 4$$

We then find that $$S = 2^{h+2} - 4 - 2h = 4\cdot 2^h - 2h = 2^{h+2} - 4$$

We understand that the value of  $$h$$ is on the order of $$log(n)$$ so that gives us:

> $$ S \approx 4n - 2log(n) - 4 $$

This means that heapify is linear!!


Running time analysis

### Radix Sort