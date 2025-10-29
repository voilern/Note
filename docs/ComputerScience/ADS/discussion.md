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

## Week 4

**1-1** **Self-adjusting data structures**

In typical applications of data structures, it is not a single operation that is performed, but rather a sequence of operations, and the relevant complexity measure is not the time taken by one operation but the total time of a sequence.

If we are content with a data structure that is efficient in only an amortized sense, there is another way to obtain efficiency. Instead of imposing any explicit structural constraint, we allow the data structure to be in an arbitrary state, and we design the access and update algorithms to adjust the structure in a simple, uniform way, so that the efficiency of future operations is improved. We call such a data structure self-adjusting.

What are the pros and cons of self-adjusting data structures?

**1-2** For a skew heap, please construct a sequence of operations in which some operations take O(n) time.

## Week 5

**1-1** Please compare the performance of BuildHeap with leftist heaps and binomial queues.

**1-2** What if we simply use some normal tree for heaps without restriction of two child?  For the union procedure why don't we just make one heap the left child of the other heap?

**1-3** We have learnt the 2-3-4 B+tree in which the internal node (other than possibly the root) has two, three, or four children and all leaves have the same depth. In this problem, we shall implement 2-3-4 heaps, which support the mergeable-heap operations.  

The 2-3-4 heaps differ from 2-3-4 trees in the following ways:  

- In 2-3-4 heaps, only leaves store keys, and each leaf $x$ stores exactly one key in the field `key[x]`.  
- There is no particular ordering of the keys in the leaves; that is, from left to right, the keys may be in any order.  
- Each internal node $x$ contains a value `small[x]` that is equal to the smallest key stored in any leaf in the subtree rooted at $x$.  
- The root $r$ contains a field `height [r]` that is the height of the tree.  

Finally, 2-3-4 heaps are intended to be kept in main memory, so that disk reads and writes are not needed.  

Implement the following 2-3-4 heap operations. Each of the operations in parts (a)-(e) should run in $O(log n)$ time on a 2-3-4 heap with $n$ elements. The `UNION` operation in part (f) should run in $O(log n)$ time, where $n$ is the number of elements in the two input heaps.  

- (a) `MINIMUM`, which returns a pointer to the leaf with the smallest key.  
- (b) `DECREASE-KEY`, which decreases the key of a given leaf $x$ to a given value k ≤ `key[x]`.  
- \(c) `INSERT`, which inserts leaf $x$ with key $k$.  
- (d) `DELETE`, which deletes a given leaf $x$.  
- (e) `EXTRACT-MIN`, which extracts the leaf with the smallest key.  
- (f) `UNION`, which unites two 2-3-4 heaps, returning a single 2-3-4 heap and destroying the input heaps.

## Week 6

**1-1** When solving the Turnpike Reconstruction Problem, in Step 3 we always find the next "largest" distance and check.  How about finding the next “smallest” distance and check?

**1-2** How would you design the evaluation function for Gobang （五子棋）?
