# Lab 07: The Integer-Valued Knapsack Problem

**Name:** _______________________  

## Introduction
In this lab, you will explore different variations of the Knapsack problem and design a dynamic programming solution for one of them. You may work in small groups or individually, but everyone should write up their own answers to the questions below. 

Suppose we're going hiking. We have items $1, \ldots, n$ that we could pack in multiples with corresponding values $v_1, \ldots, v_n$ and weights $w_1, \ldots, w_n$. For example, we might have a water bottle (weight 1, value 2) and a sandwich (weight 2, value 5). Consider the following three problem variations:

1. Maximize value subject to capacity $W$, where you can pack at most one of each item.
2. Maximize value subject to capacity $W$, where you can pack a positive integer value of each item.
3. Minimize weight subject to minimum value $V$, where you can pack a positive integer value of each item.

---

## Part 1: Dynamic Programming Fundamentals

**Question:** Which of the three problem variations can be solved using the dynamic programming approaches we've studied in lecture? Explain your reasoning.

*Your Answer:*

All three variations can be solved using dynamic programming because they have **optimal substructure** and **overlapping subproblems**. An optimal solution can be constructed from optimal solutions to smaller problems, and the answers to repeated subproblems can be stored and reused.

1. **Maximize value with at most one of each item:** Define $DP(i,c)$ as the maximum value achievable using the first $i$ items with capacity $c$. For each item, compare excluding it with including it once. Both choices use only the first $i-1$ items for the remaining problem, preventing an item from being selected more than once.

2. **Maximize value with multiple copies allowed:** Define $DP(c)$ as the maximum value achievable with capacity $c$. Choosing one copy of item $i$ adds value $v_i$ and leaves a smaller problem with capacity $c-w_i$. The same item remains available in that smaller problem, allowing repeated selections. Store the maximum value over all feasible choices.

3. **Minimize weight while achieving at least value $V$:** Define $DP(t)$ as the minimum weight needed to achieve at least the remaining required value $t$. Choosing item $i$ adds weight $w_i$ and reduces the remaining requirement to $\max(0,t-v_i)$. Compare all choices and store the minimum weight, with $DP(0)=0$.

Thus, changing the objective from maximization to minimization or allowing repeated items changes the states and transitions, but does not prevent dynamic programming from being used.

These formulations assume positive integer weights and values. If "a positive integer" in variations 2 and 3 means that **at least one copy of every item is required**, first include one of each item and then apply DP to the remaining capacity or value requirement, provided the initial selection is feasible.

## Part 2: The Naive Recursive Algorithm

Below is a naive recursive algorithm intended to solve one of the variations listed above.

```text
Function MaxValue(W):
    if W == 0:
        return 0
        
    max_val = 0
    for i from 1 to n:
        if w_i <= W:
            current_val = v_i + MaxValue(W - w_i)
            if current_val > max_val:
                max_val = current_val
                
    return max_val
```

**Question:** Which of the three problem variations (1, 2, or 3) does this algorithm solve? How do you know? Explain your reasoning.

*Your Answer:*

The algorithm solves **Variation 2**, interpreted as allowing unlimited copies of each item. It maximizes value by comparing $v_i + \text{MaxValue}(W-w_i)$ for every item that fits and returning the largest result. The condition $w_i \leq W$ prevents exceeding the capacity. Each recursive call considers all items again, so the same item can be selected repeatedly, unlike Variation 1. It does not solve Variation 3 because it maximizes value under a weight limit rather than minimizing weight to meet a value requirement. Strictly speaking, the code allows zero copies of an item; it does not require at least one of every item if "positive integer" is read literally.

## Part 3: Recursion Tree

Consider the following simple example:
* **Knapsack Capacity:** $W = 3$
* **Item 1:** Weight $w_1 = 1$, Value $v_1 = 2$
* **Item 2:** Weight $w_2 = 2$, Value $v_2 = 5$
* **Item 3:** Weight $w_3 = 1$, Value $v_3 = 3$

**Question:** Draw the full recursion tree for `MaxValue(3)` using the example above. Label the remaining capacity at each node and indicate which item is chosen along each branch.

*Your Answer (take a picture and upload it):*


## Part 4: Recurrence Relation

**Question:** Derive a mathematical recurrence relation for $OPT(W)$, the maximum value you can pack into a knapsack of capacity $W$, based on the naive recursive algorithm. Be sure to state your base case(s) and explain how you arrived at your formula.

*Your Answer:*


## Part 5: Bottom-Up Dynamic Programming

Instead of using the slow recursive approach, we can build a table bottom-up. 

**Question:** Use your recurrence relation from Part 4 to fill in the following table for the example provided in Part 3 ($w_1=1, v_1=2$ and $w_2=2, v_2=5$). At each step, record the optimal value and the specific item chosen to achieve that value. 

| Capacity ($W$) | Optimal Value ($OPT$) | Item Chosen |
|:---:|:---|:---|
| **0** | | |
| **1** | | |
| **2** | | |
| **3** | | |
| **4** | | |
| **5** | | |
| **6** | | |

*Explain how you calculated the row for $W=6$*


## Part 6: Traceback

**Question:** Using the "Item Chosen" column from the table you completed in Part 5, trace back to find the exact quantities of each item to pack that maximize the value. What is the final optimal packing configuration for $W=6$ and the maximum value?

*Your Answer:*
