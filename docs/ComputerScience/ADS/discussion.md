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

## Week 7

**1-1** Three-way-mergesort : Suppose instead of dividing in two halves at each step of the mergesort, we divide into three one thirds, sort each part, and finally combine all of them using a three-way-merge. 

What is the overall time complexity of this algorithm for sorting n elements?  Prove it.

How about k-way merge?

**1-2** Please show an example of how a problem can be partitioned into sub-problems, yet divide and conquer method does not work.

## Week 8

**1-1** Dijkstra’s Algorithm for solving the shortest path problem is generally viewed as a greedy algorithm.

Please try to provide a dynamic programming perspective on this algorithm.

**1-2** When solving the problem All-Pairs Shortest Path by Floyd method, if we change the order of iterations to i, k, j, while other codes remain unchanged (as shown below), can we still get the correct answer?

```c
for( i = 0; i < N; i++ ) 
    for( k = 0; k < N; k++ ) 
        for( j = 0; j < N; j++ ) 
            if( D[ i ][ k ] + D[ k ][ j ] < D[ i ][ j ] ) 
                D[ i ][ j ] = D[ i ][ k ] + D[ k ][ j ]; 
```

**1-3** Can we use recursions (as we do in dfs) to solve a problem via dynamic programming?  Please explain your answer by analyzing an example.

## Week 9

**1-1** Let us consider the following problem: given the set of activities S, we must schedule them all using the minimum number of rooms.

**Greedy1:** Use the optimal algorithm for the Activity Selection Problem to find the max number of activities that can be scheduled in one room. Delete and repeat on the rest, until no activity is left.

**Greedy2:**

- Sort activities by start time. Open room 1 for a1​.
- for i=2 to n if ai​ can fit in any open room, schedule it in that room; otherwise open a new room for ai​.

Discuss on each of the above algorithms -- will they end up at the optimal solution?

**1-2** Is Huffman algorithm the only way to obtain the optimal codes?  Prove it if your answer is YES, or show a counter-example if NO.

## Week 10

**1-1** The longest Hamiltonian cycle problem is to find a simple cycle of maximum length in a graph.  To prove that it is NPC, we must first prove that it is in NP -- that is, to prove that an answer can be verified to be correct in polynomial time.

To verify that a cycle is Hamiltonian is easy. But how would you know if a cycle really is the longest?