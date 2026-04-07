# CIS 525 — Introduction to Algorithm Analysis
## Unit 2 Test Study Guide
**Material: Lectures 07–11 | Professor: Torben Amtoft**

---

# TABLE OF CONTENTS
1. [Unit 07 — Divide & Conquer](#unit-07--divide--conquer)
2. [Unit 08 — Heaps & Priority Queues](#unit-08--heaps--priority-queues)
3. [Unit 09 — Sorting (Perspectives & Lower Bounds)](#unit-09--sorting-perspectives--lower-bounds)
4. [Unit 10 — Dynamic Programming](#unit-10--dynamic-programming)
5. [Unit 11 — Union-Find](#unit-11--union-find)

---

# Unit 07 — Divide & Conquer

## 📌 Core Paradigm
**Divide & Conquer** breaks a problem into subproblems, solves them recursively, and combines results.

The running time of a Divide & Conquer algorithm is described by a **recurrence**:
```
T(n) = 2·T(n/2) + Θ(n)   →  Solution: T(n) = Θ(n log n)
```

---

## 07-01: The Divide & Conquer Paradigm
- **Divide**: Split the problem into smaller subproblems
- **Conquer**: Solve subproblems recursively (base case: small enough → solve directly)
- **Combine**: Merge solutions to subproblems into the solution for the original

**Key insight**: The recurrence T(n) = aT(n/b) + f(n) is solved by the **Master Theorem**.

### Master Theorem (Quick Reference)
Let T(n) = aT(n/b) + f(n), a ≥ 1, b > 1:
- If f(n) = O(n^(log_b(a) − ε)) → T(n) = Θ(n^log_b(a))
- If f(n) = Θ(n^log_b(a)) → T(n) = Θ(n^log_b(a) · log n)
- If f(n) = Ω(n^(log_b(a) + ε)) → T(n) = Θ(f(n))

---

## 07-02: Merge Sort & Quicksort

### Merge Sort — Divide & Conquer Steps
Given array (example): `18 | 14 | 12 | 27 | 20 | 28 | 10 | 11`

**Step 1 — Divide**: Clarify the boundary between the two partitions
- Left half: `18 14 12 27`
- Right half: `20 28 10 11`

**Step 2 — Conquer**: Recursively sort each half
- Left sorted: `12 14 18 27`
- Right sorted: `10 11 20 28`

**Step 3 — Combine (Merge)**: Merge both halves into a new sorted array
- Result: `10 11 12 14 18 20 27 28`

**Recurrence**: T(n) = 2·T(n/2) + Θ(n)
**Solution**: T(n) = **Θ(n log n)** ✅

> Merge Sort *always* runs in Θ(n log n). It references **Cormen Section 2.3**.

---

### Quicksort — Divide & Conquer Steps
Discovered in **1959 by Tony Hoare**. Another application of Divide & Conquer.

**Key difference from Merge Sort**:
- Merge Sort does non-trivial work **after** solving subproblems (the merge)
- Quicksort does non-trivial work **before** solving subproblems (the partition)

**Steps**:
1. **Pick a pivot** (how to pick is discussed separately)
2. **Partition** the array so that all keys < pivot are left, all keys > pivot are right
3. **Recursively sort** the left and right partitions — **no merge needed**!

#### Example (using pivot = 20, array = `18 14 12 27 20 28 10 11`):
- After partition: `18 14 12 10 11 | 20 | 27 28`
- Recursive calls on arrays of size 5 and 2

**Quicksort always produces a sorted array after step 3 — no extra combine step.**

---

## 07-02 Section 2.1: Partitioning (Dutch National Flag Algorithm)
To partition, we use the **Dutch National Flag Algorithm**:
- 🔴 **Red** records: keys **less than** the pivot
- ⚪ **White** records: keys **equal to** the pivot
- 🔵 **Blue** records: keys **greater than** the pivot

Other partitioning algorithms exist (e.g., Cormen Section 7.1), but the Dutch National Flag handles **duplicate keys** better.

**Example** (picking 15 as pivot from `10 15 12 14 15 19 15 15`):
```
Red:   ::: 15 15 15
Result: [10 12 14] | [15 15 15] | [19]
→ Recursive calls on arrays of size 3 and 2
```

> If we use Cormen's algorithm instead, it does not distinguish keys equal to vs. less than the pivot, yielding different partition sizes.

---

## 07-03: Quicksort Analysis

### Best Case
Occurs when we **always pick a pivot close to the median**.
- If guaranteed, running time satisfies T(n) = 2·T(n/2) + Θ(n)
- **Solution**: T(n) = **Θ(n log n)**

### Worst Case
Occurs when we **always pick the smallest or largest key** as pivot.
- Every partition is maximally unbalanced
- T(n) = T(n − 1) + Θ(n)
- **Solution**: T(n) = **Θ(n²)**

### Almost-Worst Case
Occurs when ~90% of records have smaller (or larger) keys than the pivot.
- T(n) = T(9n/10) + T(n/10) + Θ(n)
- **Solution**: T(n) = **Θ(n log n)** (though constant c is larger than best case)

### "Average" Case
- Assume pivot is chosen **randomly**
- By probabilistic analysis (covered in CIS 775): **expected** running time is Θ(n log n)

### Space Use
- Dutch National Flag is **in-place** (uses Θ(1) extra space)
- But Quicksort's **recursion is not tail-recursive**: stack must hold info for deferred recursive calls
- If we always process the **shortest partition first**, at most **lg(n)** calls are pending
- Space use = **O(log n)** → considered "in-place"

### Concluding Remarks
| Algorithm | Time | In-Place? |
|-----------|------|-----------|
| Merge Sort | Always Θ(n log n) | No |
| QuickSort | Θ(n log n) avg/best | Yes (O(log n) stack) |
| Heapsort | Always Θ(n log n) | Yes |

**Optimization**: When number of elements is below a threshold, switch to **Insertion Sort** — this won't affect asymptotic complexity but is faster in practice.

---

## 07-04: Multiplying Large Integers

### Problem
> Multiply two **n-digit** positive integers.

Modern computers use a fixed number of bits per integer — if n is larger, we implement our own multiplication.

We use **base 10** (decimal) for examples (instead of base 2) for clarity.

### Naïve Approach: Grade School Multiplication
To multiply two n-digit numbers: perform **4 multiplications** of n/2-digit numbers + additions.

**Example**: 47 × 23
```
    47
  × 23
  ----
   141   (47 × 3)
  940    (47 × 20)
 -----
  1081
```
Recurrence: T(n) = 4·T(n/2) + Θ(n)
By Master Theorem: T(n) = **Θ(n²)** — same as grade school!

### Karatsuba's Algorithm (Divide & Conquer)
Key insight: We can compute (A·D + B·C) using **only one extra multiplication** instead of two:
```
A·D + B·C = (A+B)·(C+D) − A·C − B·D
```
So we only need **3 multiplications** of n/2-digit numbers (A·C, B·D, and (A+B)·(C+D)):

**Recurrence**: T(n) = 3·T(n/2) + Θ(n)
**Solution** (Master Theorem): T(n) = **Θ(n^log₂3) ≈ Θ(n^1.585)**

> This is significantly better than Θ(n²) for large n!

---

## 07-05: Matrix Multiplication (Strassen's Algorithm)

### Problem
Multiply two n×n matrices.

### Naïve: Θ(n³) — three nested loops

### Divide & Conquer (Block Matrix Multiplication)
Split each n×n matrix into 4 blocks of size n/2 × n/2.
- Requires **8 multiplications** of n/2 matrices → T(n) = 8T(n/2) + Θ(n²) → Θ(n³) — no improvement

### Strassen's Algorithm
Uses **7 matrix multiplications** (not 8) + additions:
- T(n) = 7·T(n/2) + Θ(n²)
- **Solution**: T(n) = **Θ(n^log₂7) ≈ Θ(n^2.81)**

> Strassen's is asymptotically better than Θ(n³) for large matrices.

---

## 07-06: Selection Problem

### Problem
> Given an array of n elements, find the **k-th smallest**.

Special cases: k=1 (min), k=n (max), k=⌈n/2⌉ (median).

### Approach: Quickselect (using Divide & Conquer)
1. Pick a pivot and **partition** the array
2. Only recurse on **one side** (the side containing rank k)
3. No need to sort both halves — saves time!

**Best case**: T(n) = T(n/2) + Θ(n) → **Θ(n)**
**Worst case**: T(n) = T(n−1) + Θ(n) → **Θ(n²)**
**Average case** (random pivot): **Θ(n)** expected

---

## 07-07: Linear Selection (Median-of-Medians)

### Goal: Find k-th smallest in **guaranteed Θ(n)** time

### Algorithm (Blum, Floyd, Pratt, Rivest, Tarjan):
1. Divide array into groups of 5
2. Find the **median of each group** (by sorting each group of 5 — constant time)
3. Recursively find the **median of those medians** — this is the pivot
4. Partition around the pivot; recurse on the appropriate side

### Why it works:
- The median-of-medians pivot is guaranteed to be "not too extreme"
- At least 30% of elements are ≤ pivot AND at least 30% are ≥ pivot
- So each recursive call is on at most **70% of the original input**

**Recurrence**: T(n) ≤ T(n/5) + T(7n/10) + Θ(n)
**Solution**: T(n) = **Θ(n)** — linear time!

> In practice, Quickselect with random pivot is faster due to smaller constants. Median-of-medians matters for **worst-case guarantees**.

---

# Unit 08 — Heaps & Priority Queues

## 08-01: Priority Queues

### What is a Priority Queue?
A data structure supporting:
- **Insert(x)**: Add element x with a key
- **FindMin()** / **FindMax()**: Return the element with min/max key
- **DeleteMin()** / **DeleteMax()**: Remove and return the min/max element

### Use Cases
- Task scheduling (highest-priority task next)
- Dijkstra's shortest path algorithm
- Event-driven simulation

### Implementations
| Structure | Insert | FindMin | DeleteMin |
|-----------|--------|---------|-----------|
| Unsorted Array | O(1) | O(n) | O(n) |
| Sorted Array | O(n) | O(1) | O(1) |
| **Binary Heap** | **O(log n)** | **O(1)** | **O(log n)** |

---

## 08-02: The Heap Property

### Max-Heap Property
Every node's key is **≥** the keys of its children.
- The **maximum** is always at the root

### Min-Heap Property
Every node's key is **≤** the keys of its children.
- The **minimum** is always at the root

### Shape Property
A heap is a **complete binary tree**: all levels are fully filled except possibly the last, which is filled left-to-right.

### Key Operations
- **Sift-Up** (Bubble-Up): After Insert, restore heap property going up → O(log n)
- **Sift-Down** (Heapify): After DeleteMax, restore heap property going down → O(log n)

---

## 08-03: Heap Representation

### Array Representation (no pointers needed!)
For a heap stored in array A[1..n]:
- **Root**: A[1]
- **Parent of A[i]**: A[⌊i/2⌋]
- **Left child of A[i]**: A[2i]
- **Right child of A[i]**: A[2i + 1]

```
Heap:        10
            /  \
           8    6
          / \  / \
         5   3 4   2

Array: [10, 8, 6, 5, 3, 4, 2]
Index:  [1,  2, 3, 4, 5, 6, 7]
```

**Space**: Θ(n) — no pointer overhead

---

## 08-04: Heap Creation (Build-Heap)

### Naïve: Insert one by one → O(n log n)

### Efficient: Build-Heap (Floyd's Algorithm) → O(n)

**Algorithm**:
1. Place all n elements in an array arbitrarily
2. Starting from the **last non-leaf** (index ⌊n/2⌋), call Sift-Down on each node going up to the root

**Why O(n)?**
- Most nodes are near the bottom and require little sifting
- Formal analysis shows total work = O(n) via geometric series

```
Build-Heap(A):
  for i = ⌊n/2⌋ down to 1:
    Sift-Down(A, i)
```

---

## 08-05: Heapsort

### Algorithm
1. **Build-Heap** from the input array → O(n)
2. Repeatedly **Extract-Max** (swap root with last element, reduce heap size, Sift-Down) → n × O(log n) = O(n log n)

```
Heapsort(A):
  Build-Heap(A)             // O(n)
  for i = n down to 2:
    swap A[1] and A[i]      // max goes to correct position
    Heap-Size--
    Sift-Down(A, 1)         // O(log n)
```

**Time**: O(n log n) always (best, avg, worst)
**Space**: O(1) — **in-place!**

### Comparison
| Property | Merge Sort | Quicksort | Heapsort |
|----------|-----------|-----------|----------|
| Best case | Θ(n log n) | Θ(n log n) | Θ(n log n) |
| Avg case | Θ(n log n) | Θ(n log n) | Θ(n log n) |
| Worst case | Θ(n log n) | **Θ(n²)** | Θ(n log n) |
| In-place? | ❌ No | ✅ Yes | ✅ Yes |

---

# Unit 09 — Sorting (Perspectives & Lower Bounds)

## 09-01: Sorting Perspectives

### Comparison-Based Sorting
All sorting algorithms covered so far (Merge Sort, Quicksort, Heapsort, Insertion Sort) are **comparison-based**: they only use comparisons between elements to determine order.

### Stability
A sorting algorithm is **stable** if equal elements maintain their original relative order.
- **Stable**: Merge Sort, Insertion Sort
- **Not stable**: Heapsort, Quicksort (basic versions)

### In-Place vs. Not In-Place
- **In-place**: Heapsort, Quicksort (O(log n) stack)
- **Not in-place**: Merge Sort (requires O(n) extra space)

---

## 09-02: Lower Bounds for Comparison-Based Sorting

### Claim
Any comparison-based sorting algorithm requires **Ω(n log n)** comparisons in the worst case.

### Proof via Decision Trees
- Any comparison-based sort can be modeled as a **decision tree**: each internal node represents a comparison, each leaf represents a permutation (outcome)
- For n elements, there are **n!** possible orderings → at least n! leaves
- A binary tree with n! leaves has height ≥ **log₂(n!)**
- By Stirling's approximation: log₂(n!) = **Θ(n log n)**

**Therefore**: Any comparison-based sorting algorithm must make Ω(n log n) comparisons.

### Consequence
Merge Sort and Heapsort are **asymptotically optimal** among comparison-based sorts.

### Non-Comparison Sorts (can beat Ω(n log n))
- **Counting Sort**: O(n + k) where k = range of keys
- **Radix Sort**: O(d(n + k)) where d = number of digits
- **Bucket Sort**: O(n) average when keys are uniformly distributed

> These work by exploiting **structure in the keys**, not just comparisons.

---

# Unit 10 — Dynamic Programming

## 📌 Core Idea
**Dynamic Programming (DP)** = Recursion + **Memoization** (storing results of subproblems to avoid redundant computation)

### When to Use DP
1. Problem has **overlapping subproblems** (same subproblems solved multiple times)
2. Problem has **optimal substructure** (optimal solution to the problem contains optimal solutions to subproblems)

### Two Approaches
- **Top-Down** (Memoization): Recursive + cache results
- **Bottom-Up** (Tabulation): Fill a table iteratively from smallest subproblems up

---

## 10-01: Introduction to Dynamic Programming

### Classic Example: Fibonacci
- Naïve recursion: **exponential** (redundant subproblems)
- With memoization: **O(n)**

### DP Recipe
1. Define the **subproblem** (what does OPT(i) or OPT(i,j) mean?)
2. Write the **recurrence** (how does the optimal for subproblem relate to smaller subproblems?)
3. Identify **base cases**
4. Fill the **table** bottom-up (or recurse top-down with cache)
5. **Reconstruct** the solution (trace back through the table)

---

## 10-02: Exact Change Problem

### Problem
> Given coin denominations c₁, c₂, ..., cₖ and a target amount T, can we make **exact change** for T?

### DP Formulation
- `can[j]` = true if amount j can be made using the given coins
- **Base case**: `can[0]` = true (amount 0 is trivially achievable)
- **Recurrence**: `can[j]` = true if ∃ coin cᵢ such that `can[j − cᵢ]` = true

### Table-Filling (Bottom-Up)
```
can[0] = true
for j = 1 to T:
  for each coin cᵢ:
    if j ≥ cᵢ and can[j − cᵢ]:
      can[j] = true
```
**Time**: O(T × k), **Space**: O(T)

---

## 10-03: Binary (0/1) Knapsack

### Problem
> Given n items, each with weight wᵢ and value vᵢ, and a knapsack of capacity W, choose items to **maximize total value** without exceeding weight W. Each item can be taken **at most once**.

### DP Formulation
- `OPT(i, w)` = max value using items 1..i with weight capacity w
- **Base case**: `OPT(0, w)` = 0 for all w
- **Recurrence**:
  ```
  OPT(i, w) = OPT(i−1, w)                          if wᵢ > w (can't take item i)
            = max(OPT(i−1, w), vᵢ + OPT(i−1, w−wᵢ))  otherwise
  ```

**Time**: O(n × W), **Space**: O(n × W)

> ⚠️ This is **pseudo-polynomial** — it depends on the *value* of W, not just the *number of bits* to encode W. The problem is NP-hard in general.

---

## 10-04: All-Pairs Shortest Path (Floyd-Warshall)

### Problem
> Given a directed weighted graph with n vertices, find the shortest path between **every pair** of vertices.

### Floyd-Warshall Algorithm
- `dist[i][j][k]` = shortest path from i to j using only vertices {1, 2, ..., k} as intermediates
- **Base case**: `dist[i][j][0]` = edge weight w(i,j) if edge exists, else ∞
- **Recurrence**:
  ```
  dist[i][j][k] = min(dist[i][j][k−1],
                      dist[i][k][k−1] + dist[k][j][k−1])
  ```
  (Either the shortest path doesn't use vertex k, or it goes through k)

**Space optimization**: Only keep the current and previous k-layers → O(n²) space.

**Time**: **O(n³)**, **Space**: O(n²)

### Handles Negative Edges?
✅ Yes — Floyd-Warshall works with negative edge weights (but not negative cycles).

---

## 10-05: Chained Matrix Multiplication

### Problem
> Given matrices A₁, A₂, ..., Aₙ, find the **optimal parenthesization** that minimizes the number of scalar multiplications.

Matrix multiplication is **associative** but **not commutative** — parenthesization matters for cost!

### DP Formulation
- `m[i][j]` = minimum number of multiplications to compute Aᵢ × Aᵢ₊₁ × ... × Aⱼ
- **Base case**: `m[i][i]` = 0
- **Recurrence**:
  ```
  m[i][j] = min over all i ≤ k < j of:
               m[i][k] + m[k+1][j] + pᵢ₋₁ · pₖ · pⱼ
  ```
  where matrix Aᵢ has dimensions pᵢ₋₁ × pᵢ

**Time**: O(n³), **Space**: O(n²)

---

## 10-06: Principle of Optimality (Bellman's Principle)

### Statement
> An optimal solution to a problem contains optimal solutions to its subproblems.

This is the fundamental requirement for **Dynamic Programming** to apply.

### When Does It Fail?
DP does NOT apply when subproblems are **not independent** (e.g., shortest path with constraints like "don't repeat vertices" → DP on graphs doesn't always work for such constraints).

### Summary of DP Problems
| Problem | State | Recurrence | Time |
|---------|-------|-----------|------|
| Exact Change | `can[j]` | `can[j−cᵢ]` | O(T·k) |
| 0/1 Knapsack | `OPT(i,w)` | Max with/without item i | O(n·W) |
| All-Pairs Shortest | `dist[i][j][k]` | Through vertex k or not | O(n³) |
| Chain Matrix Mult | `m[i][j]` | Split at k | O(n³) |

---

# Unit 11 — Union-Find

## 📌 Core Concept
**Union-Find** (Disjoint Set Union / DSU) is a data structure for maintaining a collection of **disjoint sets** under:
- **MakeSet(x)**: Create a new set {x}
- **Find(x)**: Return the **representative** (root) of the set containing x
- **Union(x, y)**: Merge the sets containing x and y

### Use Cases
- **Kruskal's MST algorithm** (detecting cycles)
- Connected components in a graph
- Network connectivity

---

## 11-01: Basic Union-Find

### Representation: Forest of Trees
- Each set is a **rooted tree**
- Each node stores a **parent pointer**
- Root = representative of the set (parent[root] = root)

```
MakeSet(x):     parent[x] = x

Find(x):        if parent[x] == x: return x
                else: return Find(parent[x])

Union(x, y):    rx = Find(x); ry = Find(y)
                parent[rx] = ry   // attach root of x's tree to root of y's tree
```

**Problem**: Naive union can create a **chain** (linear tree) → Find takes O(n)! 

---

## 11-02: Union by Rank

### Optimization
Always attach the **shorter tree under the taller tree** (by rank/height).

- `rank[x]` = upper bound on height of the tree rooted at x
- Initially: `rank[x] = 0`

```
Union(x, y):
  rx = Find(x); ry = Find(y)
  if rx == ry: return          // already in same set
  if rank[rx] < rank[ry]:
    parent[rx] = ry
  else if rank[rx] > rank[ry]:
    parent[ry] = rx
  else:
    parent[ry] = rx
    rank[rx]++
```

**Guarantee**: With Union by Rank, tree height is **at most O(log n)** → Find is O(log n).

---

## 11-03: Path Compression

### Optimization
During **Find**, make every node on the path **point directly to the root**.

```
Find(x):
  if parent[x] != x:
    parent[x] = Find(parent[x])   // path compression!
  return parent[x]
```

### Combined: Union by Rank + Path Compression
Together, these two optimizations give:
- **Amortized time per operation**: **O(α(n))**
- α = **inverse Ackermann function** — grows so slowly it is effectively constant for all practical n

| Strategy | Find (worst) | Union (worst) | n ops (amortized) |
|----------|-------------|--------------|-------------------|
| Naive | O(n) | O(1) | O(n²) |
| Union by Rank | O(log n) | O(log n) | O(n log n) |
| Path Compression only | O(log n) amortized | O(1) | O(n log n) |
| **Both** | **O(α(n))** | **O(α(n))** | **O(n · α(n))** |

> For all practical purposes, O(α(n)) ≈ O(1).

---

# 📝 KEY FORMULAS QUICK REFERENCE

## Recurrences & Solutions
| Recurrence | Solution | Example |
|-----------|---------|---------|
| T(n) = 2T(n/2) + Θ(n) | Θ(n log n) | Merge Sort |
| T(n) = T(n−1) + Θ(n) | Θ(n²) | Quicksort worst |
| T(n) = 3T(n/2) + Θ(n) | Θ(n^log₂3) ≈ Θ(n^1.585) | Karatsuba |
| T(n) = 7T(n/2) + Θ(n²) | Θ(n^log₂7) ≈ Θ(n^2.81) | Strassen |
| T(n) = T(n/5) + T(7n/10) + Θ(n) | Θ(n) | Linear Select |

## Algorithm Complexity Cheat Sheet
| Algorithm | Best | Average | Worst | Space | Stable? |
|-----------|------|---------|-------|-------|---------|
| Merge Sort | Θ(n log n) | Θ(n log n) | Θ(n log n) | O(n) | ✅ |
| Quicksort | Θ(n log n) | Θ(n log n) | Θ(n²) | O(log n) | ❌ |
| Heapsort | Θ(n log n) | Θ(n log n) | Θ(n log n) | O(1) | ❌ |
| Insertion Sort | Θ(n) | Θ(n²) | Θ(n²) | O(1) | ✅ |
| Build-Heap | — | Θ(n) | Θ(n) | O(1) | — |
| Linear Select | Θ(n) | Θ(n) | Θ(n) | O(log n) | — |

## Lower Bound Result
> Any **comparison-based** sorting algorithm requires Ω(n log n) comparisons in the worst case.

---

# ⚡ EXAM TIPS

1. **Know your recurrences cold** — be able to apply the Master Theorem quickly
2. **Trace through examples** for Merge Sort and Quicksort partition by hand
3. **Dutch National Flag** is the key partition algorithm — know it uses red/white/blue categories
4. **Heap indexing**: parent = ⌊i/2⌋, left = 2i, right = 2i+1
5. **Build-Heap is O(n)** — don't confuse with n insertions (O(n log n))
6. **DP**: always define your subproblem precisely before writing the recurrence
7. **Knapsack is pseudo-polynomial** — know why (W is not polynomial in input size)
8. **Floyd-Warshall**: works with negative weights; fails with negative cycles
9. **Union-Find with both optimizations** = O(α(n)) amortized ≈ constant
10. **Quicksort worst case = Θ(n²)** — occurs when pivot is always min or max

---

*Study guide built from lecture notes: CIS 525 Introduction to Algorithm Analysis, Units 07–11*
*Professor: Torben Amtoft | © 2026*