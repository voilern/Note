---
comments: true
---

# Lecture 6: Backtraking

从本课起我们开始进入算法部分，我们将主要介绍回溯、分治、动态规划、贪心、NP 问题、近似算法、局部搜索、随机算法、并行算法以及外部排序。在接下来的课程中，我们将主要关注两个要求：  

- 有效性（Effectiveness）：能保证算法的结果是正确的，或者之后近似算法保证结果在一定近似比内，或者随机算法保证一定概率内是合理的。  
- 有效率（Efficiency）：算法的运行速度应当是快速的。  

## 回溯的基本思想

对于有限可能有解问题，我们可以通过所谓的暴力搜索（exhausting search），或称蛮力法（brute force）对所有可能性逐一检查以得到正确解。这种方法尽管可以保证结果的正确性，但在很多问题中，搜索空间可能是相当巨大的，暴力搜索难以在可接受的时间内给出正确解。

然而在很多情况下我们通过简单的判断便可以将大部分可能性排除在正确结果之外，这个过程我们称为剪枝（pruning），即在合适的情况下对决策树进行修剪，从而不断缩小搜索空间，在可接受的时间内完成求解。

回溯的典型过程包括：

- 构造空间树
- 进行遍历
- 如遇到边界条件，进行剪枝并回溯
- 达到目标条件

回溯的代码实现模板：

```c
bool Backtracking (int i) {
    Found = false;
    if (i > N)
        return true;  // solved with (x1, ..., xN)
    for (each xi in Si) {
        // check if satisfies the restriction R
        OK = Check((x1, ..., xi), R);  // pruning
        if (OK) {
            Count xi in;
            Found = Backtracking(i + 1);
            if (!Found)
                Undo(i);  // recover to (x1, ..., x{i-1})
        }
        if (Found) break; 
    }
    return Found;
}
```

接下来我们将在几个经典案例中进一步了解回溯的思想。

## 八皇后问题


## 收费站重建问题

收费站重建问题（The Turnpike Reconstruction Problem）

## Tic-tac-toe



## 五子棋