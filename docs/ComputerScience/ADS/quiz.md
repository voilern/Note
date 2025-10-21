# Quiz

!!! abstract
    本部分用以记录陈越老师班级每周的课前 quiz 题。

## Week 2

**R1-1** In the worst case the DELETE operation in a RED-BLACK tree of $n$ nodes requires $\Omega (\log n)$ rotations.

- T

- F

**R1-2** When insert three keys into a non-empty 2-3 tree, and if the tree gains height when the first key is in, then it is possible that the 2-3 tree will gain more height after the insertions of the next two keys. 

- T

- F

**R1-3** When inserting a node into a red-black tree, we shall first insert the node as into an ordinary binary search tree, and then color the node black.

- T

- F

**R1-4** In a Red-Black tree, the path from the root to the farthest leaf is no more than twice as long as the path from the root to the nearest leaf.   

- T

- F

**R1-5** The following binary search tree is a valid red-black tree.

![](img/quiz2_1-5.png){.center}

- T

- F

**R1-6** In a red-black tree with 3 nodes, there must be a red node.

- T

- F

**R1-7** In a red-black tree with 2 nodes, there must be a red node.

- T

- F

**R1-8** The time bound of the FIND operation in a B+ tree containing $N$ numbers is $O(\log N)$, no matter what the degree of the tree is.

- T

- F

**R1-9** The root of a B+ tree of order $m$ has at most $m$ subtrees.

- T

- F

**R1-10** In a B+ tree, leaves and nonleaf nodes have some key values in common. 

- T

- F

## Week 3

**R1-1** In a search engine, thresholding for query retrieves the top $k$ documents according to their weights.  

- T

- F

**R1-2** When measuring the relevancy of the answer set, if the precision is high but the recall is low, it means that most of the relevant documents are retrieved, but too many irrelevant documents are returned as well.  

- T

- F

**R1-3** Precision measures the quality of all the retrieved documents.

- T

- F

**R1-4** While accessing a term stored in a B+ tree in an inverted file index, range searchings are expensive.

- T

- F

**R1-5** Word stemming is to eliminate the commonly used words from the original documents. 

- T

- F

**R1-6** Inverted file contains a list of pointers to all occurrences of a term in the text.

- T

- F

**R1-7** For the document-partitioned strategy in distributed indexing, each node contains a subset of all documents that have a specific range of index.

- T

- F

**R1-8** In general, comparing with the terms with short posting lists,  those with very long posting lists are less important. 

- T

- F

**R2-1** Which of the following is NOT a step in the process of building an inverted file index?

- A.Use stemming and stop words filter to obtain terms

- B.Read in strings and parse to get words

- C.Get the posting list for each term and calculate the precision

- D.Check dictionary with each term: if it is not in, insert it into the dictionary

**R2-2** Among the following groups of concepts, which group is not totally relevant to a search engine?

- A.posting list, thresholding, recall

- B.distributed index, backtracking, query

- C.inverted file index, stop words, precision

- D.word stemming, hashing, compression

## Week 4

**R1-1** In order to prove the amortized time bound for a skew heap, the potential function can be defined to be the number of right nodes of the resulting tree.

- T

- F

**R1-2** The relationship of skew heaps to leftist heaps is analogous to the relation between splay trees and AVL trees.

- T

- F

**R1-3** With the same operations, the resulting skew heap is always more balanced than the leftist heap. 

- T

- F

**R1-4** If we merge two heaps represented by complete binary trees, the time complexity is $\Theta(N)$ provided that the size of each heap is $N$.

- T

- F

**R1-5** A leftist heap with the null path length of the root being $r$ must have at least $2^{r+1}-1$ nodes.  

- T

- F

**R1-6** A skew heap is a heap data structure implemented as a binary tree. Skew heaps are advantageous because of their ability to merge more quickly than balanced binary heaps. The worst case time complexities for Merge, Insert, and DeleteMin are all $O(N)$, while the amorited time complexities for Merge, Insert, and DeleteMin are all $O(logN)$.  

- T

- F

**R1-7** The NPL of each node  in a heap is supposed to be calculated from top down.

- T

- F

**R2-1** Which one of the following statements is TRUE?

- A.![a.png](img/quiz4_2-1-1.png){.center}
may be a leftist heap

- B.![c.png](img/quiz4_2-1-2.png){.center}
may be a skew heap

- C.![b.png](img/quiz4_2-1-3.png){.center}
may be a leftist heap

- D.![c.png](img/quiz4_2-1-4.png){.center}
may be a leftist heap

**R2-2** Merge the two given skew heaps. Which one of the following statements is FALSE?

![](img/quiz4_2-2.png){.center}

- A.25 is the left child of 14

- B.17 is the right child of 16

- C.3 is the root

- D.39 is the right child of 25

**R2-3** Merge the two leftist heaps in the following figure.  Which one of the following statements is FALSE?

![](img/quiz4_2-3.png){.center}

- A.5 is the left child of 4

- B.the null path length of 6 is the same as that of 2

- C.1 is the root with 3 being its right child

- D.Along the left most path from top down, we have 1, 2, 6, and 8

## Week 5

**R1-1** To implement a binomial queue, left-child-next-sibling structure is used to represent each binomial tree.

- T

- F

**R1-2** For a binomial queue, delete-min takes a constant time on average. 

- T

- F

**R1-3** For a binomial queue, merging takes a constant time on average. 

- T

- F

**R1-4** Making $N$ insertions into an initally empty binomial queue takes $O(N)$ time in the worst case. 

- T

- F

**R1-5** Inserting a number into a binomial heap with 15 nodes costs less time than inserting a number into a binomial heap with 19 nodes. 

- T

- F

**R2-1** After inserting number 20 into a binomial queue of 6 numbers { 12, 13, 14, 23, 24, 35 }, which of the followings is impossible?

- A.the NextSibling link of node 14 may point to node 20

- B.the LeftChild link of node 12 may point to node 14

- C.the NextSibling link of the node 20 is NULL

- D.the LeftChild link of the node 20 is NULL

**R2-2** After deleting number 14 from a binomial queue of 5 numbers { 12, 13, 14, 23, 24 }, which of the followings is impossible?

- A.the LeftChild link of the node 12 is NULL;

- B.the LeftChild link of node 24 is NULL;

- C.the NextSibling link of node 13 may point to node 23;

- D.the NextSibling link of the node 12 is NULL;

**R2-3** The potential function $Q$ of a binomial queue is the number of the trees.  After merging two binomial queues $H1$ with 22 nodes and $H2$ with 13 nodes，what is the potential change $Q (H1+H2)-(Q (H1)+Q (H2))$ ?

- A.-3

- B.-2

- C.0

- D.2

**R5-1** **BinQueue_Insert**

The function BinQueue_Insert is to insert X into a binomial queue H, and return H as the result.

```C
BinQueue BinQueue_Insert( ElementType X, BinQueue H )
{
    BinTree Carry; 
    int i; 

    H->CurrentSize++;
    Carry = malloc( sizeof( struct BinNode ) );
    Carry->Element = X;
    Carry->LeftChild = Carry->NextSibling = NULL;

    i = 0;
    while ( H->TheTrees[i] ) { 
        Carry = CombineTrees( Carry, ( )(1 分) ); //combine two equal-sized trees
        H->TheTrees[i++] = NULL;
    }
    ( )(1 分);
    return  H;
}
```

