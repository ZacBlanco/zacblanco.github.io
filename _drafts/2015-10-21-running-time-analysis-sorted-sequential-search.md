---
layout: post
title: Running Time Analysis for Sorted Sequential Search
permalink: /blog/data-structures/sorted-sequential-search-analysis
author: Zac
---

## Sequential Search on a Sorted List of Numbers

One question that many people struggled with (including myself) on the first data structures midterm involved a problem similar to the following:

Given the following algorithm for sequential search on a sorted list of numbers, derive the average running time for this algorithm on

{% highlight java linenos %}

public boolean search(int[] A, int target) {
  for (int i = 0; i < A.length; i++) {
    if (target == A[i]) return true;
    if (target < A[i]) return false;
  }
  return false;
}

{% endhighlight %}

1. A successful search (one where the item is found).
2. A failed search (where the item is **not** found).