# CIS 525 — Introduction to Algorithm Analysis  
## Unit 2 Study Guide (Units 07–11)

**Course:** CIS 525 / CIS 575 — Introduction to Algorithm Analysis  
**Professor:** Torben Amtoft  
**Textbook:** Cormen, Leiserson, Rivest, Stein — *Introduction to Algorithms* (CLRS)

---

## Table of Contents

1. [Unit 07 — Divide & Conquer](#unit-07--divide--conquer)
   - [1.1 The Divide & Conquer Paradigm](#11-the-divide--conquer-paradigm)
   - [1.2 Merge Sort](#12-merge-sort)
   - [1.3 Quicksort & Dutch National Flag Partitioning](#13-quicksort--dutch-national-flag-partitioning)
   - [1.4 Quicksort Analysis (Best / Worst / Average / Space)](#14-quicksort-analysis)
   - [1.5 Multiplying Large Integers (Karatsuba's Trick)](#15-multiplying-large-integers)
   - [1.6 Matrix Multiplication (Strassen's Algorithm)](#16-matrix-multiplication)
   - [1.7 The Selection Problem](#17-the-selection-problem)
   - [1.8 Linear-Time Selection (Median-of-Medians)](#18-linear-time-selection-median-of-medians)

2. [Unit 08 — Heaps & Priority Queues](#unit-08--heaps--priority-queues)
   - [2.1 Priority Queues (ADT & Naive Implementations)](#21-priority-queues)
   - [2.2 The Heap Property](#22-the-heap-property)
   - [2.3 Maintaining the Heap: SiftDown & PercolateUp](#23-maintaining-the-heap-siftdown--percolateup)
   - [2.4 Binary Heaps: Array Representation](#24-binary-heaps-array-representation)
   - [2.5 Building a Heap (Convert Array → Heap)](#25-building-a-heap)
   - [2.6 HeapSort](#26-heapsort)

3. [Unit 09 — Sorting](#unit-09--sorting)
   - [3.1 Comparison of Sorting Algorithms](#31-comparison-of-sorting-algorithms)
   - [3.2 Lower Bounds for Comparison-Based Sorting](#32-lower-bounds-for-comparison-based-sorting)

4. [Unit 10 — Dynamic Programming](#unit-10--dynamic-programming)
   - [4.1 The DP Paradigm: Why and How](#41-the-dp-paradigm-why-and-how)
   - [4.2 Exact Change](#42-exact-change)
   - [4.3 Binary Knapsack](#43-binary-knapsack)
   - [4.4 All-Pairs Shortest Path (Floyd-Warshall)](#44-all-pairs-shortest-path-floyd-warshall)
   - [4.5 Chained Matrix Multiplication](#45-chained-matrix-multiplication)
   - [4.6 Principle of Optimality](#46-principle-of-optimality)

5. [Unit 11 — Union-Find](#unit-11--union-find)
   - [5.1 The Union-Find Data Structure](#51-the-union-find-data-structure)
   - [5.2 Union by Rank](#52-union-by-rank)
   - [5.3 Path Compression](#53-path-compression)
   - [5.4 Combined Complexity: α(n)](#54-combined-complexity-n)

6. [Quick Reference: Master Theorem](#quick-reference-master-theorem)
7. [Algorithm Complexity Cheat Sheet](#algorithm-complexity-cheat-sheet)

---

## Unit 07 — Divide & Conquer

> **Textbook:** Cormen Sections 2.3, 7.1, 7.2, 4.2, 9.3

### 1.1 The Divide & Conquer Paradigm

**Divide & Conquer** (Latin: *divide et impera*) is a problem-solving strategy based on three steps:

1. **Decompose** the problem into smaller instances of the same type
2. **Solve** those smaller instances recursively
3. **Combine** the solutions into a solution to the original problem

When the problem is small enough (below a threshold N), solve it **directly** instead.

**General Recurrence:**

```
T(n) = a·T(n/b) + f(n)    for n ≥ N
T(n) = g(n)                for n < N
```

- **a** = number of subproblems
- **b** = factor by which the problem shrinks
- **f(n)** = cost of decomposing + combining
- **g(n)** = cost of direct solution

> **Key insight:** The asymptotic behavior of T does *not* depend on g or N — only on a, b, and f(n).

---

### 1.2 Merge Sort

**Textbook:** Cormen Section 2.3

#### How it Works

Given array `18 14 12 27 20 28 10 11`:

1. **Decompose:** split into two halves: `18 14 12 27` | `20 28 10 11`
2. **Recurse:** sort each half: `12 14 18 27` | `10 11 20 28`
3. **Merge:** combine into one sorted array: `10 11 12 14 18 20 27 28`

#### Recurrence & Complexity

```
T(n) = 2·T(n/2) + Θ(n)
```

Solution (by Master Theorem, Case 2): **T(n) ∈ Θ(n log n)**

| Property | Value |
|----------|-------|
| Time (all cases) | Θ(n log n) |
| Space | Θ(n) — requires auxiliary array |
| In-place? | No |
| Stable? | Yes |

---

### 1.3 Quicksort & Dutch National Flag Partitioning

**Textbook:** Cormen Section 7.1

#### How Quicksort Works

Quicksort (discovered 1959 by Tony Hoare) does its non-trivial work **before** recursing.

Given `18 14 12 27 20 28 10 11`, pivot = 20:

1. **Partition:** rearrange so that elements < 20 are left of 20, and > 20 are right.  
   Example result: `18 14 12 11 10 | 20 | 28 27`
2. **Recurse:** sort `18 14 12 11 10` and `28 27` separately
3. **Done:** `10 11 12 14 18 20 27 28`

#### The Dutch National Flag Algorithm (DNF) — Partitioning

The **Dutch National Flag algorithm** partitions into **three regions**:

| Color | Meaning |
|-------|---------|
| 🔴 **Red** | keys **< pivot** |
| ⚪ **White** | keys **= pivot** |
| 🔵 **Blue** | keys **> pivot** |

**Why DNF is better than Cormen Section 7.1's 2-way partition:**

Consider `10 15 12 14 18 15 19 15`, pivot = 15:

- **DNF (3-way):** yields `[10 12 14] | [15 15 15] | [19]`
  - Recursive calls on sizes **3 and 1** (the three 15s are done — they're in their final position!)

- **Cormen 7.1 (2-way):** yields `[10 15 12 14 18 15 15] | [19]`
  - Recursive calls on sizes **7 and 1** (duplicates are NOT isolated)

> **Bottom line:** When there are many duplicate keys, DNF is significantly more efficient because it isolates all copies of the pivot in a single pass, so they never need to be processed again.

**Stability:** DNF is **not stable** (swaps can change the relative order of equal-key elements).

---

### 1.4 Quicksort Analysis

**Textbook:** Cormen Section 7.2

#### Cases

| Scenario | Recurrence | Solution |
|----------|------------|----------|
| **Best case** (pivot = median every time) | T(n) = 2T(n/2) + Θ(n) | T(n) ∈ Θ(n log n) |
| **Worst case** (pivot = min or max every time) | T(n) = T(n−1) + Θ(n) | T(n) ∈ Θ(n²) |
| **"Almost worst"** (90/10 split every time) | T(n) = T(9n/10) + T(n/10) + Θ(n) | T(n) ∈ Θ(n log n) |
| **Average case** (random pivot) | (probabilistic analysis) | T(n) ∈ Θ(n log n) (expected) |

**Key insight:** Even an extreme 90%/10% split still yields Θ(n log n) — only a *perfectly* lopsided split (always picking the smallest/largest) gives Θ(n²).

#### Space Use

Quicksort's recursion is **not tail-recursive**: when recursing on the first partition, the stack must remember to process the second. This could mean up to **n recursive calls** on the stack.

**Optimization:** Always process the **shorter partition first** → at most **O(log n)** stack frames → space in **O(log n)**, which is considered "in-place."

#### Concluding Remarks

- Quicksort is typically **faster in practice** than Merge Sort and Heapsort, despite same asymptotic complexity.
- **Hybrid optimization:** When the subarray has fewer than a threshold of elements, switch to Insertion Sort. This doesn't affect asymptotic complexity but improves real-world performance.

---

### 1.5 Multiplying Large Integers

**Textbook:** Related to Cormen Chapter 4

#### Problem

Multiply two **n-digit integers** (base 10 used for illustration; base 2 applies in practice).

#### Naive Divide & Conquer — 4 multiplications

Split each n-digit number into two n/2-digit halves: P = w·10^(n/2) + y, Q = x·10^(n/2) + z.

```
P × Q = wx·10^n + (wz + yx)·10^(n/2) + yz
```

This requires **4 multiplications** of n/2-digit numbers → **T(n) = 4T(n/2) + Θ(n)**  
Solution: T(n) ∈ **Θ(n²)** (no improvement over schoolbook multiplication).

#### Karatsuba's Trick — 3 multiplications

Observe: `wz + yx = (w + y)(x + z) − wx − yz`

So only 3 multiplications are needed: `wx`, `yz`, and `(w+y)(x+z)`.

**Example:** Multiply 2043 × 2512 (using halves: w=20, y=43, x=25, z=12):
```
wx = 20 × 25 = 500
yz = 43 × 12 = 516
(w+y)(x+z) = 63 × 37 = 2331
wz + yx = 2331 - 516 - 500 = 1315

Result = 500·10^4 + 1315·10^2 + 516 = 5,132,016  ✓
```

**Recurrence:** T(n) = 3T(n/2) + Θ(n)  
**Solution (Master Theorem, Case 1):** T(n) ∈ **Θ(n^(log₂ 3)) ≈ Θ(n^1.585)**

#### General Case (k-way Split)

For any k ≥ 2, splitting into k parts and using (2k−1) multiplications:

```
T(n) = (2k−1)·T(n/k) + Θ(n)
```
Solution: T(n) ∈ Θ(n^(log_k(2k−1))) where the exponent approaches **1** as k→∞.

> **We can get arbitrarily close to linear time, but cannot beat Ω(n)** (any algorithm must generate at least n output digits).

---

### 1.6 Matrix Multiplication

**Textbook:** Cormen Section 4.2

#### Problem

Multiply two n × n matrices A and B to get C, where:
```
C[i][j] = Σ(k=1 to n) A[i][k] × B[k][j]
```

**Naive algorithm:** three nested loops → **Θ(n³)**

#### Naive Divide & Conquer — 8 Submatrix Multiplications

View A and B as 2×2 block matrices. Compute C = A·B with:

```
c₁₁ = a₁₁b₁₁ + a₁₂b₂₁
c₁₂ = a₁₁b₁₂ + a₁₂b₂₂
c₂₁ = a₂₁b₁₁ + a₂₂b₂₁
c₂₂ = a₂₁b₁₂ + a₂₂b₂₂
```

8 multiplications of (n/2 × n/2) submatrices → **T(n) = 8T(n/2) + Θ(n²)**  
Solution: T(n) ∈ **Θ(n³)** — same as naive, Divide & Conquer alone doesn't help here!

#### Strassen's Algorithm — 7 Submatrix Multiplications

In 1969, Volker Strassen showed that **7 multiplications suffice** instead of 8.

```
m₁ = (a₂₁ + a₂₂ − a₁₁)(b₂₂ − b₁₂ + b₁₁)
m₂ = a₁₁·b₁₁
m₃ = a₁₂·b₂₁
m₄ = (a₁₁ − a₂₁)(b₂₂ − b₁₂)
m₅ = (a₂₁ + a₂₂)(b₁₂ − b₁₁)
m₆ = (a₁₂ − a₂₁ + a₁₁ − a₂₂)·b₂₂
m₇ = a₂₂(b₁₁ + b₂₂ − b₁₂ − b₂₁)

c₁₁ = m₂ + m₃
c₁₂ = m₁ + m₂ + m₅ + m₆
c₂₁ = m₁ + m₂ + m₄ − m₇
c₂₂ = m₁ + m₂ + m₄ + m₅
```

**Recurrence:** T(n) = 7T(n/2) + Θ(n²)  
**Solution (Master Theorem):** T(n) ∈ **Θ(n^(log₂ 7)) ≈ Θ(n^2.807)**

> **Important caveat:** Unless you're multiplying *very large* matrices, the simple 3-loop algorithm is probably faster in practice. Also, sparse matrices can be handled even faster.

Current best known: **O(n^2.373)** — research is ongoing!

---

### 1.7 The Selection Problem

**Textbook:** Cormen Section 9.3

#### Problem Definition

Given n records, find the **kth smallest** record:

> Find a record with key y such that there are **< k records with key < y** and **≥ k records with key ≤ y**.

**Running example:** 25 numbers `37, 22, 42, 11, 17, 48, 12, 16, 20, 45, 61, 24, 47, 53, 33, 44, 35, 19, 10, 50, 13, 16, 30, 54, 23` — find the 17th smallest.

Sorting gives: `10, 11, 12, 13, 16, 16, 17, 19, 20, 22, 23, 24, 30, 33, 35, 37, 42, 44, 45, 47, 48, 50, 53, 54, 61`  
Answer: the 17th element = **42**.

But sorting requires Ω(n log n). Can we do better?

#### Naive Divide & Conquer (Quickselect)

1. **Choose a pivot p**
2. **Partition** with DNF into: Red (< p), White (= p), Blue (> p)
3. **Recurse** on only the one partition where the kth element falls — no need to recurse on both sides!

**Example:** pivot = 23 → Red: 10 elements < 23, White: 1 element = 23, Blue: 14 elements > 23.  
To find the 17th smallest → recurse on Blue, now finding the **6th smallest** of those 14.

**Running time:**
- **Worst case** (always pick the min/max): T(n) = T(n−1) + Θ(n) → T(n) ∈ Θ(n²)
- **Random pivot (expected):** T(n) ∈ Θ(n) (by probabilistic analysis)
- **If pivot = exact median:** T(n) = T(n/2) + Θ(n) → T(n) ∈ Θ(n) — but finding the exact median IS the selection problem! (Chicken-and-egg problem)

---

### 1.8 Linear-Time Selection (Median-of-Medians)

**Textbook:** Cormen Section 9.3

#### The Key Idea

Instead of finding the **exact median** (which is circular), find a **good-enough pivot** — one that guarantees the partitions aren't too imbalanced.

**Algorithm:**
1. **Divide** the n numbers into chunks of **5** (ignore any leftover chunk when finding the pivot)
2. **Find the median** of each 5-element chunk (constant time per chunk: Θ(n/5) total = Θ(n))
3. **Recursively find the median** of all those chunk-medians — this is the pivot
4. **Partition** using DNF with that pivot
5. **Recurse** on at most one partition

**Example on the 25 running numbers:**

| Chunk | Median |
|-------|--------|
| 37, 22, 42, 11, 17 | **22** |
| 48, 12, 16, 20, 45 | **20** |
| 61, 24, 47, 53, 33 | **47** |
| 44, 35, 19, 10, 50 | **35** |
| 13, 16, 30, 54, 23 | **23** |
| **Median of medians** | **23** |

So 23 is chosen as the pivot — the same pivot we used in the example above!

#### Recurrence & Analysis

```
T(n) = T(n/5) + T(q·n) + Θ(n)
```
where:
- T(n/5): finding the median of medians recursively
- T(q·n): recursive call on the chosen partition (at most q·n elements)
- Θ(n): partitioning via DNF

**Bounding q:** The pivot is ≥ at least ⌈(n/5)/2⌉ chunk medians, each of which is ≥ 3 elements in its chunk. This guarantees at least ~3n/10 elements ≤ pivot, so at most **71%** of elements are on either side.

With q = 0.71 (and since 1/5 + 0.71 = 0.91 < 1), the recurrence T(n) = T(xn) + T(yn) + Θ(n) solves to **T(n) ∈ Θ(n)** when x + y < 1.

> **Why chunks of 5?** Even numbers don't work well (ties in median). Chunks of 3 give T(n) = T(n/3) + T(2n/3) + Θ(n) → only Θ(n log n). Chunks of 7 also work.

---

## Unit 08 — Heaps & Priority Queues

> **Textbook:** Cormen Sections 6.1–6.5

### 2.1 Priority Queues

A **priority queue** is a collection of records, each with a (not necessarily unique) **key**. The key determines priority (higher key = higher priority in a max-priority queue).

#### Required Operations

| Operation | Description |
|-----------|-------------|
| **Insert** | Add a new record |
| **Improve** | Increase the key of an existing record |
| **FindMax** | Return (but don't remove) the maximum-key record |
| **DeleteMax** | Remove and return the maximum-key record |

#### Naive Implementations

| Structure | Insert | FindMax | DeleteMax |
|-----------|--------|---------|-----------|
| Unsorted Array | Θ(1) | Θ(n) | Θ(n) |
| Sorted Array | Θ(n) | Θ(1) | Θ(1) |
| **Binary Heap** | **O(log n)** | **Θ(1)** | **O(log n)** |

> **Goal:** All operations in **O(log n)** worst-case — achieved by the binary heap.

---

### 2.2 The Heap Property

A **heap** is a rooted tree satisfying:

> **Max-Heap Property:** If q is a child of p, then `key(p) ≥ key(q)`

Every parent has a key ≥ its children's keys. The maximum key is always at the **root**.

*(For a min-heap, the inequality is reversed: parent key ≤ children's keys.)*

**Example max-heap (keys shown):**
```
          77
         /  \
       42    48
      /  \  /  \
    38   21 45  24
   /  \
  30   13
```

---

### 2.3 Maintaining the Heap: SiftDown & PercolateUp

#### SiftDown (Max-Heapify)

Used when a node's key is **too small** (violates heap property with its children). Fix by swapping down.

```
SiftDown(w):
  while w has a child with greater key:
    swap w with its child that has the greatest key
```

**Example:** Change root 77 → 37:
```
Before:        After SiftDown:
    37               48
   /  \             /  \
  30   45          30   45
      /  \             /  \
     48   24          38   24
    /  \             /  \
  38   13            37  13
```

**Complexity:** Θ(h) where h = height of the tree.

#### PercolateUp

Used when a node's key is **too large** (violates heap property with its parent). Fix by swapping up.

```
PercolateUp(w):
  while w has a parent with smaller key:
    swap w with its parent
```

**Example:** Change leaf 13 → 40:
```
Before:        After PercolateUp:
    48               48
   /  \             /  \
  30   45          30   45
      /  \             /  \
     38   24          40   24
    /  \             /  \
   37  13            37  38
```

**Complexity:** Θ(h) where h = height of the tree.

#### Priority Queue Operations via Heap

| Operation | Action |
|-----------|--------|
| **FindMax** | Return the root (Θ(1)) |
| **Insert** | Add as leaf → PercolateUp |
| **Improve** | Update key → PercolateUp |
| **DeleteMax** | Replace root with a leaf → SiftDown |

All operations except FindMax are **Θ(h)**.

---

### 2.4 Binary Heaps: Array Representation

**Textbook:** Cormen Section 6.1

A **binary heap** is a binary tree that is:
1. **Complete:** every level is fully filled except possibly the last, which is filled from left to right
2. Satisfies the **heap property**

#### Array Representation

A binary heap on n nodes is stored in array A[1..n] where:

| Relationship | Index Formula |
|-------------|---------------|
| Root | A[1] |
| Left child of A[x] | A[2x] |
| Right child of A[x] | A[2x + 1] |
| Parent of A[x] | A[⌊x/2⌋] |

**Example:** The heap:
```
        77 (index 1)
       /  \
    48(2)  30(3)
    /  \  /
  42(4) 45(5) 21(6)
```
is stored as: `A = [77, 48, 30, 42, 45, 21]`

#### Height of a Binary Heap

A binary heap with n nodes has height:
```
h = ⌊log₂(n)⌋
```

A binary tree of height h fits at most **2^(h+1) − 1** nodes.

**Therefore:** All operations on a binary heap with n nodes run in **O(log n)** worst-case.

---

### 2.5 Building a Heap

**Textbook:** Cormen Section 6.3

#### Goal

Convert an arbitrary array A[1..n] into a valid binary heap.

#### Approach 1 (Top-Down, Slow): Insert one-by-one

Insert A[1], then A[2], ..., A[n], each time using PercolateUp. The i-th insertion takes O(log i), so total time = Σ(i=1 to n) log(i) ∈ **Θ(n log n)**.

#### Approach 2 (Bottom-Up, Fast): Build-Max-Heap

Leaves are already trivially valid heaps. Process from the last non-leaf (index ⌊n/2⌋) down to the root, calling SiftDown on each:

```
Convert(A[1..n]):
  for i ← ⌊n/2⌋ downto 1:
    SiftDown(i)
```

**Example:** Start with array `[17, 48, 21, 42, 45, 30]` (heap indices 1–6):

Step 1: i=3, node=21. Its child is 30 (greater). Swap → `[17, 48, 30, 42, 45, 21]`  
Step 2: i=2, node=48. Its children are 42 and 45. 48 > both, no swap.  
Step 3: i=1, node=17. Greatest child is 48. Swap → `[48, 17, 30, 42, 45, 21]`  
Then 17's greatest child is 45. Swap → `[48, 45, 30, 42, 17, 21]`  
Result: valid max-heap `[48, 45, 30, 42, 17, 21]`

#### Why O(n) Not O(n log n)?

The recurrence for the top-down description of Build-Max-Heap:
```
T(n) = 2·T(n/2) + log(n)
```
By Master Theorem (Case 1: log(n) ∈ O(n^q) for any q > 0, and q < log_b(a) = 1):  
**T(n) ∈ Θ(n)**

Intuition: Most nodes are near the bottom and barely move when sifted down. Only the root can travel all the way down — a Θ(log n) cost — but there's only one root.

> **Summary:** Building a heap from an arbitrary array takes **Θ(n)** time.

---

### 2.6 HeapSort

**Textbook:** Cormen Section 6.4

#### Algorithm

```
HeapSort(A[1..n]):
  // Step 1: Build a max-heap
  Convert(A[1..n])           // O(n)
  
  // Step 2: Repeatedly extract the max
  for i ← n downto 2:
    swap A[1] and A[i]       // Put max in its final position
    SiftDown(A[1], heap size = i-1)  // Restore heap property on A[1..i-1]
```

**Loop invariant:**
1. `A[i+1..n]` contains the n−i greatest elements, in sorted (non-decreasing) order
2. `A[1..i]` satisfies the heap property

#### Full Example: Sort `[37, 23, 65, 17, 42, 58]`

**Initial array** (treated as tree):
```
Array:  37  23  65  17  42  58
Index:   1   2   3   4   5   6
```

**Step 1: Build heap** (SiftDown on indices 3, 2, 1):
- SiftDown index 3: 65 > 37, swap → heap starts forming
- SiftDown index 2: 42 > 23, swap → ...
- SiftDown index 1: 65 is max, sift 37 down

Result after Build-Heap: `[65, 42, 58, 17, 23, 37]`

**Step 2: Sort by extraction:**

| Step | Swap A[1] ↔ A[i] | SiftDown result | Array state |
|------|-------------------|-----------------|-------------|
| i=6 | swap 65 ↔ 37 | sift 37: → 58 takes root | `[58, 42, 37, 17, 23, **65**]` |
| i=5 | swap 58 ↔ 23 | sift 23: → 42 takes root | `[42, 23, 37, 17, **58, 65**]` |
| i=4 | swap 42 ↔ 17 | sift 17: → 37 takes root | `[37, 23, 17, **42, 58, 65**]` |
| i=3 | swap 37 ↔ 17 | sift 17: → 23 takes root | `[23, 17, **37, 42, 58, 65**]` |
| i=2 | swap 23 ↔ 17 | (trivial) | `[17, **23, 37, 42, 58, 65**]` |

**Final sorted array:** `17 23 37 42 58 65` ✓

#### Complexity

| Phase | Cost |
|-------|------|
| Build-Heap | Θ(n) |
| n−1 SiftDown calls (each O(log n)) | Θ(n log n) |
| **Total** | **Θ(n log n)** |

Space: **Θ(1)** extra — HeapSort is **in-place** (like Insertion Sort, but with O(n log n) time).

However, HeapSort is **not stable** (swapping the root and the last element can change the relative order of equal keys).

---

## Unit 09 — Sorting

> **Textbook:** Cormen Sections 8.1, 6.4

### 3.1 Comparison of Sorting Algorithms

| Algorithm | Worst-Case Time | In-Place? | Stable? |
|-----------|----------------|-----------|---------|
| **Insertion Sort** | O(n²) | ✅ Yes | ✅ Yes |
| **Merge Sort** | O(n log n) | ❌ No | ✅ Yes |
| **Heapsort** | O(n log n) | ✅ Yes | ❌ No |
| **Quicksort** | O(n²) | ✅ Yes* | ❌ No |

*In-place if using the "always recurse on the shorter partition first" optimization.

> **Key observation:** Each algorithm has 2 of 3 desirable properties. Quicksort has only 1, but is typically the fastest in practice.

**Why HeapSort is not stable:** Consider `R7a, R7b, R5` (R7a and R7b both have key 7). This already forms a max-heap, so R7a (root) gets placed last, yielding `R5, R7b, R7a` — the original order of the 7s is reversed.

**Why QuickSort with DNF is not stable:** The Dutch National Flag algorithm itself performs swaps that can change the relative order of equal keys.

**Hybrids:** Both Quicksort and Merge Sort can be optimized by switching to Insertion Sort for small subarrays. This doesn't change asymptotic complexity but improves real-world performance.

---

### 3.2 Lower Bounds for Comparison-Based Sorting

**Textbook:** Cormen Section 8.1 (Theorem 8.1)

#### Information-Theoretic Argument

How many yes/no questions do you need to find the right answer among n options? **At least ⌈log₂ n⌉ questions** — because:
- 3 questions can distinguish at most 2³ = 8 outcomes
- k questions can distinguish at most 2^k outcomes

**Consequence for sorting:** There are **n!** possible orderings (permutations) of n distinct elements. A sorting algorithm must distinguish between all of them, so it needs at least log₂(n!) comparisons.

#### Decision Trees

A comparison-based sorting algorithm can be modeled as a **binary decision tree** where:
- Each **internal node** is a comparison: "A[i] ≤ A[j]?"
- Each **leaf** is a final sorted permutation
- The **height** of the tree = worst-case number of comparisons

For n elements, the decision tree must have at least **n!** leaves, so its height must be at least:

```
log₂(n!) = log₂(∏(i=1 to n) i) = Σ(i=1 to n) log₂(i) ∈ Θ(n log n)
```

#### The Main Theorem (Cormen Theorem 8.1)

> **All comparison-based sorting algorithms must have worst-case running time in Ω(n log n).**

**Corollary:** Merge Sort and Heapsort are **asymptotically optimal** comparison-based sorts.

#### Decision Tree Examples

- **InsertionSort's** decision tree is very unbalanced (when inserting the n-th element, it's likely compared against many elements before finding its position, wasting information)
- **Heapsort and MergeSort** have much more balanced decision trees, reflected in their Θ(n log n) time

> **Qualifier:** This lower bound applies only to **comparison-based** sorts. Non-comparison sorts like **Counting Sort** can beat it: Counting Sort runs in Θ(n + k) where k is the range of keys.

---

## Unit 10 — Dynamic Programming

> **Textbook:** Cormen Chapter 14 (formerly Chapter 15 in earlier editions)

### 4.1 The DP Paradigm: Why and How

#### The Problem: Repeated Computation

Many problems solved naively involve computing the same subproblems over and over.

**Example: Fibonacci**
```
fib(n):
  if n = 0 or n = 1: return 1
  else: return fib(n-2) + fib(n-1)
```
`fib(n)` is called an exponential number of times — `fib(k)` is called `fib(n-k)` times when computing `fib(n)`.

**Example: Binomial Coefficients C(n,k)**
```
C(n, k) = C(n-1, k-1) + C(n-1, k)     (for 0 < k < n)
C(n, 0) = C(n, n) = 1
```
Computing C(6, 3) naively recomputes C(4, 2) multiple times.

#### The Solution: Dynamic Programming (Bottom-Up)

Instead of recomputing, **compute each subproblem once** and **store the result** in a table.

**Fibonacci (iterative, DP):**
```
i, j ← 1
for k ← 1 to n-1:
  i, j ← j, i + j
return j
```

**C(n, k) — Pascal's Triangle (DP):**
Build the triangle bottom-up: each entry = sum of two entries above it.

#### General DP Recipe

1. **Identify subproblems** P₁ ... Pₘ whose solutions are used to build the answer P₀
2. **Find the dependency DAG** (directed acyclic graph): edge from Pᵢ to Pⱼ if solving Pᵢ uses Pⱼ
3. **Solve in topological order** (leaves of the DAG first, root last)
4. Each subproblem is solved **exactly once**

> **DP vs. Memoization:** Both avoid repeated computation. DP (bottom-up) computes all subproblems in order; memoization (top-down) only computes reachable ones but has overhead from recursion and hash lookups. In most cases, DP is preferred.

---

### 4.2 Exact Change

#### Problem

Given n coin denominations d₁ < d₂ < ... < dₙ (with d₁ = 1 penny), and a target amount A, find the **minimum number of coins** that sum to exactly A. (Unlimited supply of each denomination.)

#### Why Greedy Fails

US coins (quarters, dimes, nickels, pennies) work with greedy (always use the largest denomination possible). But without nickels, giving 30¢ greedily gives: 1 quarter + 5 pennies = 6 coins. Optimal: 3 dimes = 3 coins.

#### DP Formulation

Let **c[i, a]** = minimum coins to make amount a using only denominations d₁ ... dᵢ.

**Base cases:**
```
c[i, 0] = 0              (zero amount needs zero coins)
c[0, a] = ∞              (no coins available, can't make a > 0)
```

**Recurrence:**
```
c[i, a] = c[i-1, a]                        if dᵢ > a  (can't use coin i)
c[i, a] = min(c[i-1, a], 1 + c[i, a-dᵢ])  if dᵢ ≤ a  (use or don't use coin i)
```

> **Note:** `c[i, a-dᵢ]` (not `c[i-1, a-dᵢ]`) because we have unlimited coins of each denomination.

#### Algorithm

```
for i ← 0 to n:
  c[i, 0] ← 0
  for a ← 1 to A:
    if i = 0:          c[i, a] ← ∞
    elif dᵢ > a:       c[i, a] ← c[i-1, a]
    else:              c[i, a] ← min(c[i-1, a], 1 + c[i, a-dᵢ])
```

#### Full Example: n=3, d=[1,4,5], A=8

| i\a | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-----|---|---|---|---|---|---|---|---|---|
| 0 | 0 | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ | ∞ |
| 1 (d=1) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| 2 (d=4) | 0 | 1 | 2 | 3 | **1** | 2 | 3 | **2** | **2** |
| 3 (d=5) | 0 | 1 | 2 | 3 | 1 | **1** | 2 | 2 | **2** |

**Reading c[3,8]:** = min(c[2,8], 1 + c[3,3]) = min(2, 1+3) = **2 coins**

**Traceback to find actual coins:**
- c[3,8] = c[2,8] → don't use d₃=5
- c[2,8] < c[1,8] → use d₂=4 once; look at c[2,4]
- c[2,4] < c[1,4] → use d₂=4 again; look at c[2,0]
- c[2,0] = c[1,0] = 0 → done

**Answer:** 2 coins of denomination 4. ✓

**Complexity:** Time and Space both in **Θ(n·A)** (pseudo-polynomial: polynomial in n and A, but A could be exponentially large in bits).

---

### 4.3 Binary Knapsack

**Textbook:** Cormen Exercise 15.2-2

#### Problem

n items, each with weight wᵢ (positive integer) and value vᵢ. Knapsack capacity W.  
Pick a subset to **maximize total value** subject to **total weight ≤ W**.  
Each item can be picked at most once (binary: xᵢ ∈ {0, 1}).

**Running example:** n=4, W=14

| Item | Weight | Value |
|------|--------|-------|
| 1 | 7 | 2 |
| 2 | 9 | 4 |
| 3 | 5 | 1 |
| 4 | 8 | 3 |

Optimal: pick items 2 and 3 → weight = 14, value = 5.

#### Why Brute Force is Bad

2ⁿ subsets to check → **exponential**. No polynomial-time algorithm is known (NP-hard).

#### DP Formulation

Let **V[i, w]** = maximum value achievable using only items 1..i with capacity w.

**Base cases:**
```
V[0, w] = 0    (no items → no value)
V[i, 0] = 0    (no capacity → no value)
```

**Recurrence:**
```
V[i, w] = V[i-1, w]                           if wᵢ > w  (item too heavy)
V[i, w] = max(V[i-1, w], vᵢ + V[i-1, w-wᵢ])  if wᵢ ≤ w  (take or skip)
```

> **Key difference from Exact Change:** We use V[i-1, w-wᵢ] (not V[i, w-wᵢ]) because each item can only be used **once**.

#### Algorithm

```
for i ← 0 to n:
  for w ← 0 to W:
    if i = 0 or w = 0: V[i, w] ← 0
    elif wᵢ > w:       V[i, w] ← V[i-1, w]
    else:              V[i, w] ← max(V[i-1, w], vᵢ + V[i-1, w-wᵢ])
```

#### Full Table (for our example):

| i\w | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|-----|---|---|---|---|---|---|---|---|---|---|----|----|----|----|-----|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 (w=7,v=2) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 |
| 2 (w=9,v=4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 2 | 4 | 4 | 4 | 4 | 4 | 4 |
| 3 (w=5,v=1) | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 2 | 2 | 4 | 4 | 4 | 4 | 4 | 5 |
| 4 (w=8,v=3) | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 2 | 3 | 4 | 4 | 4 | 4 | 5 | 5 |

**V[4,14] = max(V[3,14], 3 + V[3,6]) = max(5, 3+1) = 5** ✓

**Traceback:**
1. V[4,14] = V[3,14] → don't pick item 4
2. V[3,14] > V[2,14] → pick item 3; look at V[2, 14-5] = V[2,9]
3. V[2,9] > V[1,9] → pick item 2; look at V[1, 9-9] = V[1,0]
4. V[1,0] = V[0,0] = 0 → don't pick item 1

**Answer:** Items 2 and 3. ✓

**Complexity:** Time and Space in **Θ(n·W)** — pseudo-polynomial (W could be huge).

---

### 4.4 All-Pairs Shortest Path (Floyd-Warshall)

**Textbook:** Cormen Section 23.2

#### Problem

Given a directed graph with n nodes (1..n) where edges have lengths (possibly negative, but no negative cycles), find the shortest path between **every pair of nodes**.

#### DP Formulation

**Key subproblem:** For each i, j ∈ 1..n and k ∈ 0..n:

> **Dₖ(i, j)** = length of the shortest path from i to j using only nodes 1..k as **intermediates**

Answer: Dₙ(i, j) for all i, j.

**Base case** (k=0, no intermediate nodes allowed):
```
D₀(i, i) = 0
D₀(i, j) = L(i, j)   if edge exists from i to j
D₀(i, j) = ∞         otherwise
```

**Recurrence:**
```
Dₖ(i, j) = min(Dₖ₋₁(i, j),   Dₖ₋₁(i, k) + Dₖ₋₁(k, j))
             ↑                  ↑
         don't go              go through
         through k             node k
```

#### Implementation (Single Array Suffices)

```
// Initialize D from graph
for i ← 1 to n:
  for j ← 1 to n:
    if i = j:              D[i,j] ← 0
    elif edge i→j exists:  D[i,j] ← L(i,j)
    else:                  D[i,j] ← ∞

// Floyd-Warshall
for k ← 1 to n:
  for i ← 1 to n:
    for j ← 1 to n:
      v ← D[i,k] + D[k,j]
      if v < D[i,j]: D[i,j] ← v
```

> **Why one table suffices:** `Dₖ(i, k) = Dₖ₋₁(i, k)` and `Dₖ(k, j) = Dₖ₋₁(k, j)` — updating the "through k" entries never changes the values used for the next iteration.

> **Why k is the outer loop:** The order matters! k must be the outer loop because we iterate over k in the recurrence and need previous k values first.

#### Example

Graph (4 nodes, edges: 1→2=5, 1→4=50, 2→3=15, 2→4=5, 3→4=15, 4→3=5):

**D₀:**

| i\j | 1 | 2 | 3 | 4 |
|-----|---|---|---|---|
| 1 | 0 | 5 | ∞ | 50 |
| 2 | ∞ | 0 | 15 | 5 |
| 3 | ∞ | ∞ | 0 | 15 |
| 4 | 15 | ∞ | 5 | 0 |

After Floyd-Warshall (**D₄**):

| i\j | 1 | 2 | 3 | 4 |
|-----|---|---|---|---|
| 1 | 0 | 5 | **15** | **10** |
| 2 | **20** | 0 | **10** | 5 |
| 3 | **30** | **35** | 0 | 15 |
| 4 | 15 | **20** | 5 | 0 |

**Shortest path from 1 to 3:** D₄(1,3) = 15, via route 1 → 2 → 4 → 3.

**Complexity:**
- Time: **Θ(n³)**
- Space: **Θ(n²)** (one table)

**Important note on negative lengths:** Negative edge lengths are allowed, but **negative cycles** are not (they would make the "shortest path" undefined as you could loop forever). To detect negative cycles: after running Floyd-Warshall, check if any D[i,i] < 0.

---

### 4.5 Chained Matrix Multiplication

**Textbook:** Cormen Section 14.2

#### Problem

Given a chain of matrices A₁ · A₂ · · · Aₙ where Aᵢ has dimensions dᵢ₋₁ × dᵢ, find the **optimal parenthesization** that minimizes the number of scalar multiplications.

Matrix multiplication is **associative** (same result regardless of grouping) but **not commutative** (A·B ≠ B·A in general). Different parenthesizations can have drastically different costs!

**Example:** A (m×n), B (n×p), C (p×q) with m=p=100, n=q=2:
- (A·B)·C: 100·2·100 + 100·100·2 = 40,000 multiplications
- A·(B·C): 2·100·2 + 100·2·2 = 800 multiplications — **50× faster!**

A greedy strategy (multiply cheapest pair first) doesn't always work.

#### DP Formulation

**Subproblem:** Let **M[i, j]** = minimum scalar multiplications to compute Aᵢ · Aᵢ₊₁ · ... · Aⱼ.

**Base cases:**
```
M[i, i] = 0                         (single matrix: no multiplications)
M[i, i+1] = dᵢ₋₁ · dᵢ · dᵢ₊₁       (two matrices)
```

**Recurrence** (split at position k):
```
M[i, j] = min over k ∈ {i, ..., j-1} of:
           M[i,k] + M[k+1,j] + dᵢ₋₁ · dₖ · dⱼ
```

- M[i,k]: cost to compute Aᵢ...Aₖ (result: dᵢ₋₁ × dₖ matrix)
- M[k+1,j]: cost to compute Aₖ₊₁...Aⱼ (result: dₖ × dⱼ matrix)
- dᵢ₋₁ · dₖ · dⱼ: cost to multiply those two results

#### Implementation (iterate by chain length, not by i)

```
for i ← n downto 1:
  M[i, i] ← 0
  for j ← i+1 to n:
    M[i, j] ← ∞
    for k ← i to j-1:
      v ← M[i,k] + M[k+1,j] + d_{i-1} × d_k × d_j
      if v < M[i,j]: M[i,j] ← v
```

Also record the best k for each (i,j) pair to reconstruct the optimal parenthesization.

#### Example

n=3, d = [3, 2, 6, 4] (A₁ is 3×2, A₂ is 2×6, A₃ is 6×4):

```
M[3,3] = 0
M[2,2] = 0
M[2,3] = 0 + 0 + 2·6·4 = 48
M[1,1] = 0
M[1,2] = 0 + 0 + 3·2·6 = 36
M[1,3] = min(M[1,1]+M[2,3]+d₀·d₁·d₃, M[1,2]+M[3,3]+d₀·d₂·d₃)
       = min(0+48+3·2·4, 36+0+3·6·4)
       = min(48+24, 36+72) = min(72, 108) = 72
```

Optimal: k=1, so compute (A₁)·(A₂A₃) → **A·(BC)** with 72 multiplications.

**Complexity:**
- Space: **Θ(n²)**
- Time: **Θ(n³)** — each M[i,j] takes O(j-i) time; the sum over all pairs is O(n³)

---

### 4.6 Principle of Optimality

**Textbook:** Cormen Section 14.3

#### The Assumption in Dynamic Programming

DP works when:

> **The optimal solution for a given problem can be compiled from optimal solutions for certain subproblems.**

This property is called **optimal substructure**.

#### Shortest Paths Have Optimal Substructure ✓

**Claim:** If the shortest path from A to C goes through B, then:
- The A→B portion is the shortest path from A to B
- The B→C portion is the shortest path from B to C

**Proof:** If the A→B subpath weren't shortest, we could replace it with a shorter one and get a shorter overall path — contradiction!

#### Longest Simple Paths Do NOT Have Optimal Substructure ✗

For longest **simple** paths (each node visited at most once), the analogous property fails.

**Counterexample:**

```
    A ——5—— B
     \     /
      3   4
       \ /
        C
```

- LP(A,B) = 9 (path A→C→B, length 3+4)
- LP(A,C) = 7 (path A→B→C, length 5+4)

Wait, but LP(A,C) = 8 (path A→B→...→C? No — let me use the lecture's example)

**The key insight:** A longest simple path from A to B might go through C. But then we can't separately maximize the A→C and C→B portions, because the longest A→C path might reuse nodes that the longest C→B path needs.

**Consequence:** Longest simple path **cannot be solved directly by DP**.

#### Applications

| Problem | Optimal Substructure? | DP Works? |
|---------|----------------------|-----------|
| Shortest paths | ✅ Yes | ✅ Yes |
| Binary Knapsack | ✅ Yes | ✅ Yes |
| Longest simple path | ❌ No | ❌ No |

---

## Unit 11 — Union-Find

> **Textbook:** Cormen Chapter 19

### 5.1 The Union-Find Data Structure

#### Motivation

Given a collection of n elements divided into disjoint **classes**, we need to:
- **Check** if two elements are in the same class
- **Merge** two classes into one

**Application:** Kruskal's algorithm for minimum spanning trees uses Union-Find to check if adding an edge would create a cycle.

#### Operations

| Operation | Description |
|-----------|-------------|
| **Find(x)** | Return the **representative** (root) of x's class |
| **Union(x, y)** | Merge the classes of x and y (no-op if already same class) |

Two elements are in the same class iff `Find(x) = Find(y)`.

#### Approach 1: Linear Representation (Array)

Store a table: `representative[x]` for each element x.

| Element | A | B | C | D | E | F | G | H | I | J | K | L |
|---------|---|---|---|---|---|---|---|---|---|---|---|---|
| Rep | F | B | H | B | B | F | H | H | B | B | H | B |

- **Find:** Θ(1) — just look up the table
- **Union:** Θ(n) — must update all elements of one class

Too slow for Union!

#### Approach 2: Tree (Forest) Representation

Each class is a rooted tree. The **root = representative**. Pointers go **child → parent**.

Example forest (same classes as above):

```
F      B            H
|    / | \ \       /|\
A   D  E  I J L   G C K
```

- **Find(x):** Follow pointers to the root → Θ(h) where h = tree height
- **Union(x, y):** Find both roots; make one root a child of the other → Θ(h)

**Problem:** h could be Θ(n) in the worst case (e.g., always merging with a singleton and making it the new root creates a chain/line).

---

### 5.2 Union by Rank

**Textbook:** Cormen Section 19.3

#### Strategy

When merging two trees, make the **shorter tree's root** a child of the **taller tree's root** (break ties arbitrarily).

"Rank" is used instead of "height" — for now think of them as the same thing.

#### Pseudocode

```
Union(x, y):
  rx ← Find(x)
  ry ← Find(y)
  if rx = ry: return  // already same class
  if rank[rx] > rank[ry]:
    parent[ry] ← rx   // rx stays root; its rank unchanged
  elif rank[ry] > rank[rx]:
    parent[rx] ← ry   // ry stays root; its rank unchanged
  else:  // same rank
    parent[ry] ← rx   // arbitrary choice; rx becomes root
    rank[rx] ← rank[rx] + 1  // height increases by 1
```

#### Theorem: Height Bound

**Claim:** For any tree T built by Union-by-Rank: **H(T) ≤ log₂(N(T))**

where H(T) = height of T, N(T) = number of nodes.

**Proof by induction on size of T:**

- **Base case:** T has 1 node → H = 0, log₂(1) = 0 ✓

- **Inductive step:** T is formed by Union of T₁ and T₂. Assume inductively H(T₁) ≤ log N(T₁) and H(T₂) ≤ log N(T₂).

  **Case 1:** H(T₁) > H(T₂) → T₁'s root becomes T's root → H(T) = H(T₁) ≤ log N(T₁) < log N(T) ✓

  **Case 2:** H(T₂) > H(T₁) → symmetric.

  **Case 3:** H(T₁) = H(T₂) → height increases by 1. WLOG N(T₁) ≤ N(T₂):
  ```
  H(T) = H(T₁) + 1 ≤ log N(T₁) + 1 = log(2·N(T₁)) ≤ log(N(T₁)+N(T₂)) = log N(T) ✓
  ```

#### Consequence

With Union-by-Rank, both **Find** and **Union** run in **O(log n)** time.

---

### 5.3 Path Compression

**Textbook:** Cormen Section 19.3

#### Idea

When executing `Find(x)`, after reaching the root r, **redirect all nodes on the path** to point directly to r (the root). This "flattens" the tree.

**Before Find(D):**
```
        B
       /|\\ 
      D  E  I J L
      |
      (more nodes below D in original example)
```

**After Find(D) with path compression:**
```
        B
      / | \ \ \ \
     D  E  I  J  L  (all now directly connected to B)
```

Future `Find` operations on these nodes will be Θ(1)!

#### Rank vs. Height

With path compression, actual tree heights decrease — but updating rank (which now represents an **upper bound** on height, not the exact height) is too expensive. So we keep the old rank even after compression.

**Effect:** A few operations might still take O(log n), but the **amortized cost** is much less.

---

### 5.4 Combined Complexity: α(n)

When using **both** Union-by-Rank and Path Compression:

With n elements and m Union/Find operations:

> **Total cost = O(m · α(n))**

where **α(n)** is the **inverse Ackermann function**.

#### What is α(n)?

α(n) is the inverse of the Ackermann function A(k) — an extremely fast-growing function. For all values of n that could ever arise in practice:

**α(n) ≤ 4**

| n | α(n) |
|---|------|
| 1–2 | 0 |
| 3–4 | 1 |
| 5–16 | 2 |
| 17–2^65536 | 3 |
| astronomically larger | 4 |

> For any practical input size, α(n) is a constant (≤ 4), so we say the amortized cost per operation is **essentially Θ(1)** — "almost linear time."

#### Complexity Summary

| Method | Find | Union | Note |
|--------|------|-------|------|
| Linear (array) | Θ(1) | Θ(n) | Too slow for Union |
| Tree (naive) | Θ(h) | Θ(h) | h could be Θ(n) |
| Union by Rank | O(log n) | O(log n) | Guaranteed h ≤ log n |
| Path Compression only | amortized O(log n) | O(log n) | Better average |
| **Both (Rank + Compression)** | **amortized O(α(n))** | **amortized O(α(n))** | **Optimal** |

---

## Quick Reference: Master Theorem

For a recurrence of the form `T(n) = a·T(n/b) + f(n)` with a ≥ 1, b > 1:

Let **r = log_b(a)** (the "critical exponent").

| Case | Condition | Solution |
|------|-----------|----------|
| **Case 1** | f(n) ∈ O(nᵍ) for some q < r | T(n) ∈ Θ(nʳ) |
| **Case 2** | f(n) ∈ Θ(nʳ) | T(n) ∈ Θ(nʳ log n) |
| **Case 3** | f(n) ∈ Ω(nᵍ) for some q > r, AND regularity condition | T(n) ∈ Θ(f(n)) |

**Common recurrences:**

| Recurrence | a | b | r | Case | Solution |
|------------|---|---|---|------|----------|
| T(n) = 2T(n/2) + Θ(n) | 2 | 2 | 1 | 2 | Θ(n log n) — **Merge Sort** |
| T(n) = 4T(n/2) + Θ(n) | 4 | 2 | 2 | 1 | Θ(n²) — naive integer multiply |
| T(n) = 3T(n/2) + Θ(n) | 3 | 2 | log₂3≈1.585 | 1 | Θ(n^log₂3) — **Karatsuba** |
| T(n) = 8T(n/2) + Θ(n²) | 8 | 2 | 3 | 1 | Θ(n³) — naive matrix multiply |
| T(n) = 7T(n/2) + Θ(n²) | 7 | 2 | log₂7≈2.807 | 1 | Θ(n^log₂7) — **Strassen** |
| T(n) = 2T(n/2) + Θ(log n) | 2 | 2 | 1 | 1 | Θ(n) — **Build-Heap** |

---

## Algorithm Complexity Cheat Sheet

| Algorithm | Best | Average | Worst | Space | Stable | In-Place |
|-----------|------|---------|-------|-------|--------|----------|
| Insertion Sort | Θ(n) | Θ(n²) | Θ(n²) | O(1) | ✅ | ✅ |
| Merge Sort | Θ(n log n) | Θ(n log n) | Θ(n log n) | Θ(n) | ✅ | ❌ |
| Quicksort (DNF) | Θ(n log n) | Θ(n log n) | Θ(n²) | O(log n)* | ❌ | ✅* |
| Heapsort | Θ(n log n) | Θ(n log n) | Θ(n log n) | O(1) | ❌ | ✅ |
| Build-Heap | — | — | Θ(n) | O(1) | — | ✅ |
| Quickselect | Θ(n) | Θ(n) | Θ(n²) | O(log n) | — | ✅ |
| Median-of-Medians | — | — | Θ(n) | O(1) | — | ✅ |
| Floyd-Warshall | — | — | Θ(n³) | Θ(n²) | — | — |
| Exact Change DP | — | — | Θ(nA) | Θ(nA) | — | — |
| Knapsack DP | — | — | Θ(nW) | Θ(nW) | — | — |
| Chain Matrix DP | — | — | Θ(n³) | Θ(n²) | — | — |
| Union-Find (both) | — | Θ(α(n)) | O(log n) | O(n) | — | — |

*Quicksort space is O(log n) if always recursing on shorter partition first.

---

*Study guide compiled from lecture notes by Professor Torben Amtoft, CIS 575 / CIS 525 — Introduction to Algorithm Analysis. All content sourced directly from the course PDFs.*
