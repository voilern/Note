# Discussion

!!! abstract
    本部分用以记录陈越老师班级每周的讨论题。

## Week 2

**1-1** When it would be optimal to prefer Red-black trees over AVL trees?

**1-2** Please design an algorithm to adjust the structure of a red-black tree from top-down during an insertion.  That is, after inserting the node, we should no longer have to make any rotation.

**1-3** When to prefer B+ trees over B-trees?

## Week 3

**1-1** How to balance/select the precision or recall during relevance measurement? Please make your point by discussing some examples.

**1-2** What are the disadvantages of inverted file index technology?

## Week 5

**1-1** **Self-adjusting data structures**

In typical applications of data structures, it is not a single operation that is performed, but rather a sequence of operations, and the relevant complexity measure is not the time taken by one operation but the total time of a sequence.

If we are content with a data structure that is efficient in only an amortized sense, there is another way to obtain efficiency. Instead of imposing any explicit structural constraint, we allow the data structure to be in an arbitrary state, and we design the access and update algorithms to adjust the structure in a simple, uniform way, so that the efficiency of future operations is improved. We call such a data structure self-adjusting.

What are the pros and cons of self-adjusting data structures?

**1-2** For a skew heap, please construct a sequence of operations in which some operations take O(n) time.
