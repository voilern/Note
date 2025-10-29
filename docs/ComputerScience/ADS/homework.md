# Homework

!!! abstract 
    本部分用以记录每周发布在 PTA 上作业题的客观题，部分题目 ~~看我心情~~ 会写解析。

## Homework 1

**2-1** Insert 2, 1, 4, 5, 9, 3, 6, 7 into an initially empty AVL tree.  Which one of the following statements is FALSE?

- A. 4 is the root

- B. 3 and 7 are siblings

- C. 2 and 6 are siblings

- D. 9 is the parent of 7

**2-2** For the result of accessing the keys 3, 9, 1, 5 in order in the splay tree in the following figure, which one of the following statements is FALSE?

![](img/hw1_2-2.png){.center}

- A. 5 is the root

- B. 1 and 9 are siblings

- C. 6 and 10 are siblings

- D. 3 is the parent of 4

**2-3** If the depth of an AVL tree is 6 (the depth of an empty tree is defined to be -1), then the minimum possible number of nodes in this tree is:

- A. 13

- B. 17

- C. 20

- D. 33

**2-4** When doing amortized analysis, which one of the following statements is FALSE?

- A. Aggregate analysis shows that for all $n$, a sequence of $n$ operations takes worst-case time $T(n)$ in total.  Then the amortized cost per operation is therefore $T(n)/n$

- B. For potential method, a good potential function should always assume its maximum at the start of the sequence

- C. For accounting method, when an operation's amortized cost exceeds its actual cost, we save the difference as credit to pay for later operations whose amortized cost is less than their actual cost

- D. The difference between aggregate analysis and accounting method is that the later one assumes that the amortized costs of the operations may differ from each other

**2-5** Consider the following buffer management problem. Initially the buffer size (the number of blocks) is one. Each block can accommodate exactly one item. As soon as a new item arrives, check if there is an available block. If yes, put the item into the block, induced a cost of one. Otherwise, the buffer size is doubled, and then the item is able to put into. Moreover, the old items have to  be moved into the new buffer so it costs $k+1$ to make this insertion, where $k$ is the number of old items. Clearly, if there are $N$ items, the worst-case cost for one insertion can be $\Omega (N)$.  To show that the average cost is $O(1)$, let us turn to the amortized analysis. To simplify the problem, assume that the buffer is full after all the $N$ items are placed. Which of the following potential functions works?

- A. The number of items currently in the buffer

- B. The opposite number of items currently in the buffer

- C. The number of available blocks currently in the buffer

- D. The opposite number of available blocks in the buffer


## Homework 2

**1-1** A 2-3 tree with 3 nonleaf nodes must have 18 keys at most.

- T

- F

**2-1** In the red-black tree that results after successively inserting the keys 41; 38; 31; 12; 19; 8 into an initially empty red-black tree, which one of the following statements is FALSE?

- A. 38 is the root

- B. 19 and 41 are siblings, and they are both red

- C. 12 and 31 are siblings, and they are both black

- D. 8 is red

**2-2** After deleting 15 from the red-black tree given in the figure, which one of the following statements must be FALSE?

![](img/hw2_2-2.png){.center}

- A. 11 is the parent of 17, and 11 is black

- B. 17 is the parent of 11, and 11 is red

- C. 11 is the parent of 17, and 11 is red

- D. 17 is the parent of 11, and 17 is black

**2-3** Insert 3, 1, 4, 5, 9, 2, 6, 8, 7, 0 into an initially empty 2-3 tree (with splitting).  Which one of the following statements is FALSE?

- A. 7 and 8 are in the same node

- B. the parent of the node containing 5 has 3 children

- C. the first key stored in the root is 6

- D. there are 5 leaf nodes

**2-4** After deleting 9 from the 2-3 tree given in the figure, which one of the following statements is FALSE?

![](img/hw2_2-4.png){.center}

- A. the root is full

- B. the second key stored in the root is 6

- C. 6 and 8 are in the same node

- D. 6 and 5 are in the same node

**2-5** Which of the following statements concerning a B+ tree of order $M$ is TRUE?

- A. the root always has between 2 and $M$ children

- B. not all leaves are at the same depth

- C. leaves and nonleaf nodes have some key values in common

- D. all nonleaf nodes have between $\lceil M/2\rceil$ and $M$ children

## Homework 3

**1-1** In distributed indexing, document-partitioned strategy is to store on each node all the documents that contain the terms in a certain range.

- T

- F

**1-2** When evaluating the performance of data retrieval, it is important to measure the relevancy of the answer set.

- T

- F

**1-3** Precision is more important than recall when evaluating the explosive detection in airport security.   

- T

- F

**1-4** While accessing a term by hashing in an inverted file index, range searches are expensive.  

- T

- F

**2-1** When measuring the relevancy of the answer set, if the precision is high but the recall is low, it means that:

- A.most of the relevant documents are retrieved, but too many irrelevant documents are returned as well

- B.most of the retrieved documents are relevant, but still a lot of relevant documents are missed

- C.most of the relevant documents are retrieved, but the benchmark set is not large enough

- D.most of the retrieved documents are relevant, but the benchmark set is not large enough

**2-2** Which of the following is NOT concerned for measuring a search engine?

- A.How fast does it index

- B.How fast does it search

- C.How friendly is the interface

- D.How relevant is the answer set

**2-3** There are 28000 documents in the database. The statistic data for one query are shown in the following table. The recall is: __

|       |   Relevant    |   Irrelevant  |
| :---: |   :---:       |   :---:       |
| Retrieved |   4000    |   12000       |
| Not Retrieved |   8000|   4000        |

- A.14%

- B.25%

- C.33%

- D.50%

## Homework 4

**1-1** The result of inserting keys 1 to $2^k -1$ for any $k>4$ in order into an initially empty skew heap is always a full binary tree.

- T

- F

**1-2** The right path of a skew heap can be arbitrarily long. 

- T

- F

**2-1** Merge the two leftist heaps in the following figure.  Which one of the following statements is FALSE?

![](img/hw4_2-1.png){.center}

- A.2 is the root with 11 being its right child

- B.the depths of 9 and 12 are the same

- C.21 is the deepest node with 11 being its parent

- D.the null path length of 4 is less than that of 2

**2-2** We can perform BuildHeap for leftist heaps by considering each element as a one-node leftist heap, placing all these heaps on a queue, and performing the following step: Until only one heap is on the queue, dequeue two heaps, merge them, and enqueue the result.  Which one of the following statements is FALSE?

- A.in the $k$-th run, $\lceil N/2^k \rceil$ leftist heaps are formed, each contains $2^k$ nodes

- B.the worst case is when $N=2^K$ for some integer $K$

- C.the time complexity $T(N) = O(\frac{N}{2}log 2^0 + \frac{N}{2^2}log 2^1 + \frac{N}{2^3}log 2^2 + \cdots + \frac{N}{2^K}log 2^{K-1})$ for some integer $K$ so that $N=2^K$

- D.the worst case time complexity of this algorithm is $\Theta (NlogN)$

**2-3** Insert keys 1 to 15 in order into an initially empty skew heap.  Which one of the following statements is FALSE?

- A.the resulting tree is a complete binary tree

- B.there are 6 leaf nodes

- C.6 is the left child of 2

- D.11 is the right child of 7

**2-4** Merge the two skew heaps in the following figure.  Which one of the following statements is FALSE?

![](img/hw4_2-4.png){.center}

- A.15 is the right child of 8

- B.14 is the right child of 6

- C.1 is the root

- D.9 is the right child of 3

**5-1** **Merge two leftist heaps**

The function is to merge two leftist heaps H1 and H2.

```c
PriorityQueue Merge( PriorityQueue H1, PriorityQueue H2 )
{ 
  if (H1==NULL) return H2;
  if (H2==NULL) return H1;
  if ( ) (3 分)
    swap(H1, H2);  //swap H1 and H2
  if ( H1->Left == NULL )
    ( ) (3 分)
  else {
    H1->Right = Merge( H1->Right, H2 );
    if ( H1->Left->Npl < H1->Right->Npl )
        SwapChildren( H1 );  //swap the left child and right child of H1
        ( ) (3 分)
  }
  return H1;
}
```
<!-- ??? note "solution"
    1. H1->Element > H2->Element
    2. H1->Left = H2
    3. H1->Npl = H1->Right->npl + 1 -->

## Homework 5

**2-1** Which of the following binomial trees can represent a binomial queue of size 42?

- A.$B_0$ $B_1$ $B_2$ $B_3$ $B_4$ $B_5$

- B.$B_1$ $B_3$ $B_5$

- C.$B_1$ $B_5$

- D.$B_2$ $B_4$

**2-2** For a binomial queue, __ takes a constant time on average.

- A.merging

- B.find-max

- C.delete-min

- D.insertion

**2-3** Merge the two binomial queues in the following figure.  Which one of the following statements must be FALSE?

![](img/hw5_2-3.png){.center}

- A.there are two binomial trees after merging, which are $B_2$ and $B_4$

- B.13 and 15 are the children of 4

- C.if 23 is a child of 2, then 12 must be another child of 2

- D.if 4 is a child of 2, then 23 must be another child of 2

**2-4** Delete the minimum number from the given binomial queues in the following figure.  Which one of the following statements must be FALSE?

![](img/hw5_2-4.png){.center}

- A.there are two binomial trees after deletion, which are $B_1$ and $B_2$

- B.11 and 15 can be the children of 4

- C.29 can never be the root of any resulting binomial tree

- D.if 29 is a child of 4, then 15 must be the root of $B_1$

**5-1** **BinQueue_Find**

The functions BinQueue_Find and Recur_Find are to find X in a binomial queue H.  Return the node pointer if found, otherwise return NULL.

```c
BinTree BinQueue_Find( BinQueue H, ElementType X )
{
    BinTree T, result = NULL;
    int i, j; 

    for( i=0, j=1; j<=H->CurrentSize; i++, j*=2) {  /* for each tree in H */
        T= H->TheTrees[i];
        if ( X ( )(2 分) ){  /* if need to search inside this tree */
            result = Recur_Find(T, X);
            if ( result != NULL ) return result;
        } 
    }
    return result;
}

BinTree Recur_Find( BinTree T, ElementType X )
{
    BinTree result = NULL;
    if ( X==T->Element ) return T;
    if ( T->LeftChild!=NULL ){
        result = Recur_Find(T->LeftChild, X);
        if ( result!=NULL ) return result;
    } 
    if ( ( )(2 分) )
        result = Recur_Find(T->NextSibling, X);
    return result;
}
```

## Homework 6

**R2-1** In the Tic-tac-toe game, a "goodness" function of a position is defined as $f(P) = W_{computer} - W_{human}$where $W$ is the number of potential wins at position $P$.In the following figure, O represents the computer and X the human. What is the goodness of the position of the figure?

![](./img/hw6_2-1.png){.center}

- A.-1

- B.0

- C.4

- D.5

**R2-2** Given the following game tree, which node is the first one to be pruned with α-β pruning algorithm?

![](./img/hw6_2-2.png){.center}

- A.a

- B.b

- C.c

- D.d
