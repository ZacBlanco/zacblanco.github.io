---
layout: post
title: Average Running Time Analysis for Sorted Sequential Search
permalink: /blog/data-structures/sorted-sequential-search-analysis
author: Zac
date: 2015-11-6
mathjax: true
---

## Sequential Search on a Sorted List of Numbers
---------

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

--------------------------------

## Analysis for a Sucessful Search
--------------------------------
It's criticial that we first understand what happens in a successful search.

We know in a successful search that the algorithm will make 2 comparisons for every single item, up to the final item, for which there is only 1 comparison (because it must be successful, so we can't return false).

We should also note the probability that an item being the successful is $$ \frac{1}{n} $$.

So given the information that there should have 2 comparisons for $$n$$ items, we can say that the total number of comparisons for any item, $$i$$, the number of comparisons for a sucess at that point will have been:

> $$ 2i - 1 $$

We can explain this expression because for any item that is in the i-th place of the array from this algorithm, then we should have had two comparisons for every item from 1 to $$i -1$$ and before it. So then on the i-th item we only do one comparison and return `true`. This gives us the value $$2i-1$$. Note we could also have said that we do 2 comparisons for the $$i-1$$ items, then add one for the success, which would yield the same answer. $$ 2(i-1) + 1 = 2i - 1 $$.

Then to find the average run time of this we must sum the individual run times for an array of n size. we can use the formula from the last part to help us do this.

Total comparisons to succeed at each spot in the array:<br>

> $$ \Sigma_i^n 2i-1 = 1 + 3 + 5 + ... + (2n - 1) $$

From this we can simplify our expression for a sum of a linear sequence

> Sum of Sequence $$ = n\cdot\frac{(2n - 1) + 1}{2} $$

Now simplifying the above expression we find in fact the time to find all entries in an array of size $$n$$ is actually equal to $$n^2$$.

Then to find the average run time we simply multiply this sum by the probability for each case, which here happens to be $$\frac{1}{n}$$

So now given $$\frac{1}{n} \cdot n^2$$, we find that the average run time is in fact just $$n$$.

**Average Running Time for a Success:** $$n$$.

--------------------------------

## Analysis for a Failed Search
--------------------------------

This analysis is slightly different than above. We should note now, that on a failed search there are actually more places to fail.

We should first note that we can fail at the beginning of the algorithm if our search target is less than the first item in the array. So we $$+2$$ that to our run time. One for checking equality, then one for checking if it's less than the current item (which is must be to fail on the first spot).

Now what about for something normal, failing inside the array? Well, for any item $$i$$ where the search fails in the array, we will have had to do at least 2 searches there. So we can say for the i-th item in the array, we will have done $$2i$$ total comparisons.

However, there is one other catch to this problem. There are actually $$n+1$$ places to fail here. So when we sum we need to take that into account. There are $$n+1$$ places because 

From this we can conclude that the total number of searches must be equal to the sum:

> $$ 2n + \Sigma_i^{n} 2i $$

We can then expand this to get

> $$ 2n + 2 + 4 + 6 + ... + 2n $$

Then using the same formula to find the sum as before we get $$ 2\cdot(n+1)\cdot\frac{n+1 + 1}{n} $$ which can be simplified to

> $$ 2n + (n)\cdot\frac{2n+2}{n} = 


