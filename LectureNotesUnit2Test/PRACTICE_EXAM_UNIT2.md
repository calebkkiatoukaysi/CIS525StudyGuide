# CIS 525 — Unit 2 Practice Exam
**Covers: Units 07–11 | Divide & Conquer · Heaps · Sorting · Dynamic Programming · Union-Find**

> ⏱️ Suggested time: 90 minutes | 📝 Show ALL work for full credit

---

## 📋 SECTION 1 — Fill in the Blank
*Fill in each blank with the correct term, value, or expression.*

---

**Q1.** The three steps of Divide & Conquer are: **\_\_\_\_\_\_\_**, **\_\_\_\_\_\_\_**, and **\_\_\_\_\_\_\_**.

**Q2.** The recurrence for Merge Sort is T(n) = **\_\_\_\_\_\_\_**, and its solution is T(n) = **\_\_\_\_\_\_\_**.

**Q3.** Quicksort's worst case occurs when the pivot is always the **\_\_\_\_\_\_\_** element. In this case the recurrence is T(n) = **\_\_\_\_\_\_\_** and the solution is T(n) = **\_\_\_\_\_\_\_**.

**Q4.** In the Dutch National Flag algorithm, records are classified into three groups:
- 🔴 **Red**: keys **\_\_\_\_\_\_\_** the pivot
- ⚪ **White**: keys **\_\_\_\_\_\_\_** the pivot
- 🔵 **Blue**: keys **\_\_\_\_\_\_\_** the pivot

**Q5.** Karatsuba's algorithm multiplies two n-digit numbers using only **\_\_\_\_\_\_\_** recursive multiplications (instead of 4), giving recurrence T(n) = **\_\_\_\_\_\_\_** and solution T(n) = **\_\_\_\_\_\_\_**.

**Q6.** Strassen's algorithm multiplies two n×n matrices using **\_\_\_\_\_\_\_** recursive matrix multiplications. The recurrence is T(n) = **\_\_\_\_\_\_\_** and the solution is T(n) ≈ **\_\_\_\_\_\_\_**.

**Q7.** In a max-heap stored as array A[1..n], the parent of node at index i is at index **\_\_\_\_\_\_\_**, the left child is at index **\_\_\_\_\_\_\_**, and the right child is at index **\_\_\_\_\_\_\_**.

**Q8.** Build-Heap (Floyd's algorithm) runs in **\_\_\_\_\_\_\_** time, which is more efficient than inserting elements one by one, which takes **\_\_\_\_\_\_\_** time.

**Q9.** Any comparison-based sorting algorithm requires at least **\_\_\_\_\_\_\_** comparisons in the worst case. This is proven using a **\_\_\_\_\_\_\_** argument.

**Q10.** In Dynamic Programming, **\_\_\_\_\_\_\_** means storing the results of subproblems to avoid redundant computation. The two required properties of a problem for DP to apply are **\_\_\_\_\_\_\_** subproblems and **\_\_\_\_\_\_\_** substructure.

**Q11.** In the 0/1 Knapsack DP, the recurrence is:
OPT(i, w) = \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  if wᵢ > w
OPT(i, w) = \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  otherwise

**Q12.** Floyd-Warshall finds **\_\_\_\_\_\_\_** shortest paths in a graph of n vertices and runs in **\_\_\_\_\_\_\_** time. It handles **\_\_\_\_\_\_\_** edge weights but NOT **\_\_\_\_\_\_\_**.

**Q13.** In Union-Find, **\_\_\_\_\_\_\_** always attaches the shorter tree under the taller one, guaranteeing Find runs in O(**\_\_\_\_\_\_\_**) time. Adding **\_\_\_\_\_\_\_** makes every node on the search path point directly to the root.

**Q14.** With both Union by Rank and Path Compression, n Union-Find operations take **\_\_\_\_\_\_\_** total time, where α is the **\_\_\_\_\_\_\_** function, which is effectively **\_\_\_\_\_\_\_** for all practical n.

---

## 🧮 SECTION 2 — Apply the Master Theorem
*For each recurrence, identify a, b, and f(n), then solve using the Master Theorem. State which case applies.*

---

**Q15.** T(n) = 2T(n/2) + Θ(n)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

**Q16.** T(n) = 4T(n/2) + Θ(n)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

**Q17.** T(n) = 4T(n/2) + Θ(n²)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

**Q18.** T(n) = 3T(n/2) + Θ(n)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

**Q19.** T(n) = 8T(n/2) + Θ(n²)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

**Q20.** T(n) = 2T(n/4) + Θ(√n)
- a = \_\_\_, b = \_\_\_, f(n) = \_\_\_\_\_\_\_
- log_b(a) = \_\_\_\_\_\_\_
- Case \_\_\_ applies because: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
- **T(n) = \_\_\_\_\_\_\_**

---

## 🔢 SECTION 3 — Algorithm Trace (Show Every Step)

---

### Q21 — Merge Sort Trace
Sort the array `[35, 10, 42, 7, 23, 18, 55, 3]` using Merge Sort.

**(a)** Draw the full **divide tree** showing how the array is split at each level.

**(b)** Show each **merge step** as you combine back up.

**(c)** Write the **final sorted array**.

**(d)** What is the total number of comparisons made in the merge phase? \_\_\_\_\_\_\_ 

---

### Q22 — Quicksort + Dutch National Flag Partition
Given array `[15, 3, 9, 15, 22, 7, 15, 40, 1]` and **pivot = 15**:

**(a)** Apply the Dutch National Flag partition. Write the contents of the Red, White, and Blue regions after partitioning:

- 🔴 Red (key < 15):  `[ \_\_\_ ]`
- ⚪ White (key = 15): `[ \_\_\_ ]`
- 🔵 Blue (key > 15):  `[ \_\_\_ ]`

**(b)** What are the sizes of the two recursive subproblems? Left = \_\_\_\_, Right = \_\_\_\_

**(c)** Write the recurrence for this partition, then give the resulting sorted array.

---

### Q23 — Heap Operations

Given the array `A = [_, 5, 13, 2, 25, 7, 17, 3, 36, 18, 12]` (1-indexed, ignore `_` at index 0):

**(a)** Draw this array as a **binary tree**.

**(b)** Is this a valid **max-heap**? Justify your answer by checking the heap property at each non-leaf node.

**(c)** Apply **Build-Heap** to the array `[4, 10, 3, 5, 1, 8, 7, 2, 9, 6]`. Show each Sift-Down step.

**(d)** After running Build-Heap on the array above, what value is at index 1 (the root)? \_\_\_\_\_\_\_ 

**(e)** Perform one **ExtractMax** from your heap in part (c). What is the value extracted? \_\_\_\_\_\_\_ . Show the state of the array after the extraction and Sift-Down.

---

### Q24 — Heapsort Trace
Given array `[8, 3, 11, 1, 6, 14, 2]`:

**(a)** Run **Build-Heap** and write the resulting heap array.

**(b)** Run **Heapsort** by performing ExtractMax repeatedly. Show the array after **each** extraction:

| Extraction # | Element Removed | Array State After |
|-------------|----------------|-------------------|
| 1 | \_\_\_ | `[ ]` |
| 2 | \_\_\_ | `[ ]` |
| 3 | \_\_\_ | `[ ]` |
| 4 | \_\_\_ | `[ ]` |
| 5 | \_\_\_ | `[ ]` |
| 6 | \_\_\_ | `[ ]` |
| 7 | \_\_\_ | `[ ]` |

**(c)** What is the final sorted array (ascending)? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

## 📐 SECTION 4 — Dynamic Programming
*Show your DP table. Write the recurrence. Trace back to find the optimal solution.*

---

### Q25 — Exact Change
Coin denominations: **{1, 4, 6}**. Target amount: **T = 8**

**(a)** Write the recurrence for `can[j]`.

**(b)** Fill in the table:

| j | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|---|
| can[j] | T | \_ | \_ | \_ | \_ | \_ | \_ | \_ | \_ |

**(c)** Can exact change be made for 8? \_\_\_\_. If yes, what coins are used? \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**(d)** What is the **minimum number** of coins needed to make change for 8? \_\_\_\_

---

### Q26 — 0/1 Knapsack
Items and capacity W = 7:

| Item | Weight | Value |
|------|--------|-------|
| 1 | 1 | 2 |
| 2 | 3 | 9 |
| 3 | 4 | 10 |
| 4 | 2 | 4 |

**(a)** Write the recurrence for OPT(i, w).

**(b)** Fill in the DP table OPT(i, w):

|   | w=0 | w=1 | w=2 | w=3 | w=4 | w=5 | w=6 | w=7 |
|---|-----|-----|-----|-----|-----|-----|-----|-----|
| i=0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| i=1 | 0 | \_ | \_ | \_ | \_ | \_ | \_ | \_ |
| i=2 | 0 | \_ | \_ | \_ | \_ | \_ | \_ | \_ |
| i=3 | 0 | \_ | \_ | \_ | \_ | \_ | \_ | \_ |
| i=4 | 0 | \_ | \_ | \_ | \_ | \_ | \_ | \_ |

**(c)** What is the maximum value? \_\_\_\_\_

**(d)** Which items are included? Trace back through the table: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

### Q27 — Floyd-Warshall All-Pairs Shortest Paths
Consider a directed graph with 4 vertices. The initial distance matrix (∞ = no direct edge, 0 on diagonal) is:

```
     1    2    3    4
1  [ 0    3    ∞    7  ]
2  [ ∞    0    2    ∞  ]
3  [ ∞    ∞    0    1  ]
4  [ 6    ∞    ∞    0  ]
```

**(a)** Write the Floyd-Warshall recurrence.

**(b)** Compute dist[k=1] (after allowing vertex 1 as intermediate). Show your work.

**(c)** Compute dist[k=2] (after allowing vertices 1 and 2 as intermediates).

**(d)** Compute dist[k=3] and dist[k=4] (final answer).

**(e)** What is the shortest path from vertex 4 to vertex 3? Explicitly give the path: \_\_\_ → \_\_\_ → \_\_\_ and its total cost: \_\_\_\_

---

### Q28 — Chained Matrix Multiplication
Three matrices: A₁ (10×30), A₂ (30×5), A₃ (5×60)

**(a)** Write the recurrence for m[i][j].

**(b)** Compute m[1][1], m[2][2], m[3][3].

**(c)** Compute m[1][2] and m[2][3].

**(d)** Compute m[1][3] and give the **optimal parenthesization**.

**(e)** How many scalar multiplications does the optimal parenthesization require? \_\_\_\_

---

## 🌲 SECTION 5 — Union-Find Simulation
*Trace the Union-Find data structure. Draw the forest after each operation. Show rank arrays.*

---

### Q29 — Union-Find Step-by-Step
Start with elements {1, 2, 3, 4, 5, 6, 7, 8}. Perform all operations using **Union by Rank**. After each Union, redraw the forest.

**(a)** Initial state — draw 8 single-node trees:

`parent = [_, 1, 2, 3, 4, 5, 6, 7, 8]`
`rank   = [_, 0, 0, 0, 0, 0, 0, 0, 0]`

**(b)** Perform: Union(1,2), Union(3,4), Union(5,6), Union(7,8)
- Draw forest after these 4 unions.
- Write parent[] and rank[] arrays.

**(c)** Perform: Union(1,3), Union(5,7)
- Draw forest after these 2 unions.
- Write parent[] and rank[] arrays.

**(d)** Perform: Union(1,5)
- Draw final forest.
- Write parent[] and rank[] arrays.

**(e)** Now apply **Path Compression**. Call Find(8).
- What nodes get their parent pointer updated? \_\_\_\_\_\_\_\_\_\_\_\_\_
- Write the updated parent[] array.

**(f)** After path compression, how many steps does Find(8) take? \_\_\_\_

---

## 💬 SECTION 6 — Short Answer / Conceptual

---

**Q30.** Why is Quicksort generally preferred over Merge Sort in practice, even though Quicksort has a worse worst-case complexity?

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

**Q31.** Explain why Build-Heap runs in O(n) time, even though it calls Sift-Down n/2 times. Why isn't it O(n log n)?

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

**Q32.** Prove (informally) that any comparison-based sorting algorithm requires Ω(n log n) comparisons. What is the key counting argument?

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

**Q33.** What is "pseudo-polynomial" time? Why is the 0/1 Knapsack DP considered pseudo-polynomial and not truly polynomial?

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

**Q34.** Compare and contrast Merge Sort and Heapsort. In what situations would you prefer one over the other? Consider: time complexity, space complexity, stability, and in-place behavior.

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

**Q35.** What does "optimal substructure" mean? Give one example from the material where it holds and explain why DP works there.

*Answer:* \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

---

## ✅ ANSWER KEY

> *(Don't peek until you've tried each question!)*

<details>
<summary>🔑 Click to reveal answers</summary>

### Section 1 — Fill in the Blank
1. Divide, Conquer, Combine
2. T(n) = 2T(n/2) + Θ(n) ; T(n) = Θ(n log n)
3. smallest (or largest); T(n) = T(n−1) + Θ(n) ; T(n) = Θ(n²)
4. Red: less than; White: equal to; Blue: greater than
5. 3 multiplications; T(n) = 3T(n/2) + Θ(n) ; T(n) = Θ(n^log₂3) ≈ Θ(n^1.585)
6. 7 multiplications; T(n) = 7T(n/2) + Θ(n²) ; T(n) = Θ(n^log₂7) ≈ Θ(n^2.81)
7. parent = ⌊i/2⌋ ; left child = 2i ; right child = 2i+1
8. O(n) ; O(n log n)
9. Ω(n log n) comparisons; decision tree
10. memoization; overlapping; optimal
11. OPT(i−1, w) [if wᵢ > w]; max(OPT(i−1, w), vᵢ + OPT(i−1, w−wᵢ)) [otherwise]
12. all-pairs; O(n³); negative; negative cycles
13. Union by Rank; O(log n); Path Compression
14. O(n·α(n)); inverse Ackermann; constant (≤ 4)

### Section 2 — Master Theorem
15. a=2, b=2, f(n)=Θ(n), log₂2=1, f(n)=Θ(n^1) → Case 2 → **T(n) = Θ(n log n)**
16. a=4, b=2, f(n)=Θ(n), log₂4=2, f(n)=O(n^(2-ε)) → Case 1 → **T(n) = Θ(n²)**
17. a=4, b=2, f(n)=Θ(n²), log₂4=2, f(n)=Θ(n²) → Case 2 → **T(n) = Θ(n² log n)**
18. a=3, b=2, f(n)=Θ(n), log₂3≈1.585, f(n)=O(n^(1.585-ε)) → Case 1 → **T(n) = Θ(n^log₂3)**
19. a=8, b=2, f(n)=Θ(n²), log₂8=3, f(n)=O(n^(3-ε)) → Case 1 → **T(n) = Θ(n³)**
20. a=2, b=4, f(n)=Θ(√n)=Θ(n^0.5), log₄2=0.5, f(n)=Θ(n^0.5) → Case 2 → **T(n) = Θ(√n · log n)**

### Section 3 — Algorithm Traces

**Q21 — Merge Sort on [35,10,42,7,23,18,55,3]:**
Divide: [35,10,42,7] | [23,18,55,3]
→ [35,10] [42,7] | [23,18] [55,3]
→ [35][10] [42][7] | [23][18] [55][3]
Merge up: [10,35] [7,42] | [18,23] [3,55]
→ [7,10,35,42] | [3,18,23,55]
→ Final: **[3, 7, 10, 18, 23, 35, 42, 55]**

**Q22 — Dutch National Flag on [15,3,9,15,22,7,15,40,1], pivot=15:**
- 🔴 Red: [3, 9, 7, 1]
- ⚪ White: [15, 15, 15]
- 🔵 Blue: [22, 40]
- Left recursive call size = 4, Right = 2

**Q23 — Heap array A = [_,5,13,2,25,7,17,3,36,18,12]:**
- Tree: root=5, left subtree root=13, right=2, etc.
- NOT a valid max-heap: node at index 2 (value 13) has child at index 4 (value 25) — 25 > 13 violates max-heap property.
- Build-Heap on [4,10,3,5,1,8,7,2,9,6]: root after = **10**

**Q24 — Heapsort on [8,3,11,1,6,14,2]:**
After Build-Heap: [14, 6, 11, 1, 3, 8, 2]
Extractions: 14, 11, 8, 6, 3, 2, 1
Final sorted: **[1, 2, 3, 6, 8, 11, 14]**

### Section 4 — Dynamic Programming

**Q25 — Exact Change {1,4,6}, T=8:**
- can[0]=T, can[1]=T(1), can[2]=T(1+1), can[3]=T(1+1+1), can[4]=T(4), can[5]=T(4+1), can[6]=T(6), can[7]=T(6+1), can[8]=T(4+4 or 6+1+1)
- can[8] = **True** ; coins: {4, 4} (or {6,1,1})
- Minimum coins: **2** (using {4,4})

**Q26 — 0/1 Knapsack (W=7):**
Final OPT(4,7) = **19** (Items 1+2+4: weights 1+3+2=6, values 2+9+4=15... recalculate)
Actually: Items 2+3: w=3+4=7, v=9+10=19 → max = **19**
Items selected: **{2, 3}**

**Q27 — Floyd-Warshall:**
After k=4, shortest path 4→3: 4→1→2→3, cost = 6+3+2 = **11**

**Q28 — Chain Matrix Mult A₁(10×30), A₂(30×5), A₃(5×60):**
- m[1][2] = 10×30×5 = 1500
- m[2][3] = 30×5×60 = 9000
- m[1][3]: split at k=1: m[1][1]+m[2][3]+p₀·p₁·p₃ = 0+9000+10×30×60 = 27000
                split at k=2: m[1][2]+m[3][3]+p₀·p₂·p₃ = 1500+0+10×5×60 = 4500
- **Optimal: 4500** with parenthesization **(A₁A₂)A₃**

### Section 5 — Union-Find

**Q29:**
- After Union(1,2),(3,4),(5,6),(7,8): Four pairs, each rank 1
- After Union(1,3),(5,7): Two groups {1,2,3,4} and {5,6,7,8}, each rank 2
- After Union(1,5): One group, root = 1, rank = 3
- Find(8) with path compression: traverses 8→7→5→1, so nodes 8,7,5 all get parent=1
- After compression, Find(8) takes **1 step** (8 → root directly)

### Section 6 — Short Answer
30. Quicksort is in-place (O(log n) stack vs O(n) for Merge Sort), has better cache behavior, and in practice the average case is fast. Merge Sort's guaranteed O(n log n) is only preferred when worst-case guarantees matter.

31. Build-Heap calls Sift-Down on n/2 nodes, but most nodes are near the bottom of the tree and require very little sifting. The geometric series sum of (height × nodes_at_height) converges to O(n), not O(n log n).

32. Any comparison-based sort can be modeled as a decision tree with n! leaves (one per permutation). A binary tree with n! leaves has height ≥ log₂(n!). By Stirling: log₂(n!) = Θ(n log n). Therefore any sort must make Ω(n log n) comparisons.

33. Pseudo-polynomial means the running time is polynomial in the *numeric value* of the input, but not in the *number of bits* used to represent it. Knapsack is O(n·W) — if W=2^k, then W is exponential in the input size k, so it's not truly polynomial.

34. Both are Θ(n log n) always. Heapsort is in-place (O(1) space) while Merge Sort needs O(n). Merge Sort is stable, Heapsort is not. Merge Sort has better cache behavior in practice. Prefer Heapsort when memory is constrained; prefer Merge Sort when stability matters.

35. Optimal substructure: the optimal solution to a problem contains optimal solutions to its subproblems. Example: 0/1 Knapsack — if we know we include item i, the remaining items must form an optimal solution for capacity w−wᵢ. DP works because we can build the global optimum from local optima.

</details>

---

*Practice exam generated from CIS 525 lecture notes, Units 07–11 | Professor: Torben Amtoft