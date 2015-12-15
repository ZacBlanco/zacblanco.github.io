---
layout: page
title: Data Structures - Final Exam Review
keywords: final, exam, review, data structures, data, structures, algorithm, final, exam, review, notes, guide
permalink: /education/data-structures/final-review/
mathjax: true
description: A collection of notes to reference for the Data Structures (CS112)
---

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

## Sorting

### Mergesort

### Quicksort

### Heapsort

### Radix Sort