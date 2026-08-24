# Two Interesting Algorithmic Intuitions

Hello everyone!

I think most of you are familiar with these two algorithmic techniques:

* Small-to-large merging, which efficiently combines sets or data structures.
* Tree knapsack, whose transitions often take $O(n^2)$ time.

At first glance, these techniques may seem simple to understand and implement. However, their efficiency is surprisingly powerful. Have you ever wondered why?

In this blog, I would like to share some interesting intuitions behind these algorithms—ideas that helped me understand not only how to implement them, but also why their time complexities are so efficient.


---

## 1. Why Is Small-to-Large Merging $O(n \log n)$?

Suppose every element belongs to a set, and we repeatedly merge two sets.

The small-to-large technique says:

**> Always move the elements from the smaller set into the larger set.**

It may seem that an element could be moved many times. However, every time an element is moved, the size of its set at least doubles.

If the current set has size $s$, then the other set has size at least $s$, because the current set is the smaller one. After merging, the new set has size at least: $s+s=2s$

Therefore, the set containing an element grows like:

$$
1 \rightarrow 2 \rightarrow 4 \rightarrow 8 \rightarrow \cdots
$$

Since no set can contain more than $n$ elements, **each element can be moved at most: $O(\log n)$ times**.

There are $n$ elements, so the total complexity is $O(n\log n)$.

This is an amortized analysis. One merge may be expensive, but each element can only participate in an expensive merge logarithmically many times.

### Main intuition

Whenever an object is repeatedly processed, ask:

> Does the amount of data associated with this object at least double after every processing?

If yes, then that object is processed only $O(\log n)$ times.

This idea appears in:

* DSU on tree;
* merging maps or sets;
* maintaining colors in subtrees;
* frequency-table merging;
* “sack” techniques.

### Practice problems

https://cses.fi/problemset/task/1139
https://codeforces.com/problemset/problem/600/E
https://qoj.ac/problem/8890
https://codeforces.com/problemset/problem/1009/F
https://qoj.ac/problem/31
https://codeforces.com/problemset/problem/375/D

---

## 2. Why Is Tree Knapsack Usually $O(n^2)$?

Tree DP often requires merging the DP tables of children.

For example, suppose:

```text
dp[u][k] = the best answer inside the subtree of u
           using exactly k selected vertices
```

When merging the current DP table with a child’s table, we often write:

```cpp
for (int i = 0; i <= current_size; i++) {
    for (int j = 0; j <= child_size; j++) {
        dp_new[i + j] = max(
            dp_new[i + j],
            dp_current[i] + dp_child[j]
        );
    }
}
```

If the two parts have sizes $a$ and $b$, the merge costs approximately: $O(ab)$

Why does the total become $O(n^2)$?

The two loops can be interpreted as choosing two vertices:

* one vertex from the already-processed part;
* one vertex from the child’s subtree.

Consider a pair of vertices $x$ and $y$. They are combined when their two subtrees are merged at their lowest common ancestor.

After that, they will never be considered as belonging to two separate child subtrees again.

Therefore, every pair of vertices is counted at most once.

There are only:

$$
\binom{n}{2}=O(n^2)
$$

pairs of vertices.

Thus, the total complexity of all merges is: $O(n^2)$.

### Another view

Suppose a node has children with subtree sizes:

$$
s_1,s_2,s_3,\ldots
$$

The merge costs are:

$$
s_1s_2
$$

$$
(s_1+s_2)s_3
$$

$$
(s_1+s_2+s_3)s_4
$$

and so on.

Expanding the total gives:

$$
\sum_{i<j}s_is_j
$$

Each term corresponds to a pair of vertices from different child subtrees.

Since there are only $n$ vertices, the total number of such pairs is at most $O(n^2)$.

### Important caveat

The $O(n^2)$ bound assumes that:

* the DP state size of a subtree is proportional to its size;
* merging two tables costs the product of their sizes;
* there are no additional expensive dimensions or transitions.

Still, for the standard tree-knapsack merge, the pair-counting argument is the main intuition.

### Practice Problems

https://codeforces.com/problemset/problem/815/C
https://vjudge.net/contest/844053#overview

---
