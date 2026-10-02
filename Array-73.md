# 73. Set Matrix Zeroes

**Difficulty:** Medium  
**Platform:** LeetCode  
**Problem:** [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

---

## 📌 Problem Statement

Given an `m x n` integer matrix, if an element is `0`, set its **entire row and column** to `0`.

The modification must be performed **in-place**.

The goal is to modify the original matrix without creating another matrix proportional to its size.

---

## 🧪 Example 1

```text
Input:
matrix = [
    [1,1,1],
    [1,0,1],
    [1,1,1]
]

Output:
[
    [1,0,1],
    [0,0,0],
    [1,0,1]
]
```

The `0` at:

```text
row = 1
column = 1
```

causes:

- Row `1` → all zero
- Column `1` → all zero

---

## 🧪 Example 2

```text
Input:
matrix = [
    [0,1,2,0],
    [3,4,5,2],
    [1,3,1,5]
]

Output:
[
    [0,0,0,0],
    [0,4,5,0],
    [0,3,1,0]
]
```

There are zeros at:

```text
(0,0)
(0,3)
```

Therefore:

- Row `0` becomes zero.
- Column `0` becomes zero.
- Column `3` becomes zero.

---

# 🧠 Key Challenge

A straightforward solution would store which rows and columns contain zeroes.

For example:

```python
zero_rows = set()
zero_cols = set()
```

This would require:

```text
O(m + n)
```

extra space.

But the problem asks whether we can achieve:

```text
O(1)
```

extra space.

The key observation is:

> **We can use the first row and first column of the matrix itself as storage for our zero markers.**

---

# 💡 Core Idea

Suppose we find:

```text
matrix[r][c] == 0
```

We need to remember:

```text
Row r → must become zero
Column c → must become zero
```

Instead of creating separate arrays, we mark:

```text
matrix[r][0] = 0
matrix[0][c] = 0
```

So:

```text
matrix[r][0]
```

acts as a marker for row `r`.

And:

```text
matrix[0][c]
```

acts as a marker for column `c`.

---

# ⚠️ The Special Problem with `matrix[0][0]`

There is one important complication.

`matrix[0][0]` belongs to both:

```text
First row
```

and:

```text
First column
```

Therefore, we cannot use it to independently represent both states.

We need one extra variable:

```python
rowZero = False
```

This variable remembers whether the **first row itself** originally contained a zero.

Meanwhile:

```python
matrix[0][0]
```

is used as the marker for the **first column**.

---

# 🧩 Marker Strategy

We use:

```text
matrix[0][c] → marker for column c

matrix[r][0] → marker for row r

matrix[0][0] → marker for first column

rowZero → marker for first row
```

This lets us solve the problem with:

```text
O(1)
```

extra space.

---

# 🔎 Approach

The algorithm has three major phases.

### Phase 1 — Find Zeroes and Mark Rows/Columns

Traverse the entire matrix.

Whenever:

```python
matrix[r][c] == 0
```

mark:

```python
matrix[0][c] = 0
```

to indicate that column `c` must become zero.

For the row:

```python
matrix[r][0] = 0
```

But if `r == 0`, we cannot use `matrix[0][0]` as the first-row marker because it is already being used for the first-column state.

So we use:

```python
rowZero = True
```

---

# 🔎 Phase 1 Example

Consider:

```text
[
    [1,1,1],
    [1,0,1],
    [1,1,1]
]
```

We find:

```text
matrix[1][1] == 0
```

So mark:

```text
matrix[1][0] = 0
matrix[0][1] = 0
```

The matrix becomes:

```text
[
    [1,0,1],
    [0,0,1],
    [1,1,1]
]
```

The first row and first column now contain our markers.

---

# 🔎 Phase 2 — Set Inner Matrix to Zero

Now we process only:

```text
rows 1 → m-1
columns 1 → n-1
```

For each cell:

```python
if matrix[r][0] == 0 or matrix[0][c] == 0:
    matrix[r][c] = 0
```

This means:

```text
If row r is marked
        OR
If column c is marked

→ Set matrix[r][c] to 0
```

We intentionally start from row `1` and column `1`.

Why?

Because the first row and first column contain our markers and should not be destroyed until we finish using them.

---

# 🔎 Phase 3 — Handle First Column

After processing the inner matrix, check:

```python
if matrix[0][0] == 0:
```

If true, the **first column** must become zero.

So:

```python
for r in range(rows):
    matrix[r][0] = 0
```

---

# 🔎 Phase 4 — Handle First Row

Finally:

```python
if rowZero:
```

the original first row contained a zero.

Therefore:

```python
for c in range(cols):
    matrix[0][c] = 0
```

The first row is processed **last** so that we don't destroy the marker information before using it.

---

# 🔬 Detailed Walkthrough

Consider:

```text
matrix = [
    [1,1,1],
    [1,0,1],
    [1,1,1]
]
```

Initially:

```text
rowZero = False
```

---

### Find the Zero

We find:

```text
matrix[1][1] = 0
```

Mark:

```text
matrix[1][0] = 0
matrix[0][1] = 0
```

Matrix becomes:

```text
[
    [1,0,1],
    [0,0,1],
    [1,1,1]
]
```

---

### Process Inner Matrix

Start from:

```text
r = 1
c = 1
```

For:

```text
matrix[1][1]
```

we have:

```text
matrix[1][0] == 0
```

Therefore:

```text
matrix[1][1] = 0
```

For:

```text
matrix[1][2]
```

again:

```text
matrix[1][0] == 0
```

So:

```text
matrix[1][2] = 0
```

For row `2`, column `1`:

```text
matrix[0][1] == 0
```

So:

```text
matrix[2][1] = 0
```

Result:

```text
[
    [1,0,1],
    [0,0,0],
    [1,0,1]
]
```

---

### Handle First Column

`matrix[0][0]` is `1`, so the first column does not need to be zeroed.

---

### Handle First Row

`rowZero` is still `False`.

Therefore, the first row remains:

```text
[1,0,1]
```

Final result:

```text
[
    [1,0,1],
    [0,0,0],
    [1,0,1]
]
```

---

# 📊 Visual Representation

The first row and first column act as markers:

```text
        Column markers
             ↓
        ┌───────────
        │  C  C  C
        │
Row  →  │
markers │
        │
        │
```

More concretely:

```text
matrix[0][c]
     ↓
marks column c

matrix[r][0]
     ↓
marks row r
```

So the matrix itself becomes our auxiliary storage.

---

# 💻 Python Solution

```python
class Solution:
    def setZeroes(self, matrix: list[list[int]]) -> None:

        rows = len(matrix)
        cols = len(matrix[0])

        rowZero = False

        # Phase 1: Mark rows and columns
        for r in range(rows):
            for c in range(cols):

                if matrix[r][c] == 0:

                    # Mark column
                    matrix[0][c] = 0

                    # Mark row
                    if r > 0:
                        matrix[r][0] = 0
                    else:
                        rowZero = True

        # Phase 2: Set inner matrix to zero
        for r in range(1, rows):
            for c in range(1, cols):

                if matrix[r][0] == 0 or matrix[0][c] == 0:
                    matrix[r][c] = 0

        # Phase 3: Handle first column
        if matrix[0][0] == 0:
            for r in range(rows):
                matrix[r][0] = 0

        # Phase 4: Handle first row
        if rowZero:
            for c in range(cols):
                matrix[0][c] = 0
```

---

# 🧠 Line-by-Line Explanation

### Get matrix dimensions

```python
rows = len(matrix)
cols = len(matrix[0])
```

Stores the number of rows and columns.

---

### Track first-row zero

```python
rowZero = False
```

This variable tells us whether the original first row contained a zero.

---

### Find zeroes

```python
for r in range(rows):
    for c in range(cols):
```

Traverse every matrix element.

---

### Detect zero

```python
if matrix[r][c] == 0:
```

If a zero is found, we need to mark its row and column.

---

### Mark the column

```python
matrix[0][c] = 0
```

The first row stores column markers.

---

### Mark the row

```python
if r > 0:
    matrix[r][0] = 0
```

For rows other than the first row, the first column stores the row marker.

---

### Handle first row separately

```python
else:
    rowZero = True
```

We cannot use `matrix[0][0]` to represent the first row because it is also needed to represent the first column.

---

# 🔄 Why Do We Start From `1`?

This loop:

```python
for r in range(1, rows):
    for c in range(1, cols):
```

does not process:

```text
row 0
column 0
```

because they contain our marker information.

If we modified them too early, we could destroy information needed to process the rest of the matrix.

---

# ⚠️ Why Is the Order Important?

The order of operations is critical.

We do:

```text
1. Find zeros and create markers
2. Process inner matrix
3. Process first column
4. Process first row
```

We must handle the first row and first column **after** using them as markers.

If we zeroed them immediately, we would lose the information needed to determine which rows and columns should be zeroed.

---

# ⏱️ Complexity Analysis

Let:

```text
m = number of rows
n = number of columns
```

## Time Complexity

```text
O(m × n)
```

We scan the matrix several times:

```text
First traversal  → O(m × n)
Second traversal → O(m × n)
First column     → O(m)
First row        → O(n)
```

Therefore:

```text
O(mn) + O(mn) + O(m) + O(n)
```

which simplifies to:

```text
O(mn)
```

---

## Space Complexity

```text
O(1)
```

We only use a few variables:

```text
rows
cols
rowZero
r
c
```

We do **not** create:

- Another matrix
- A row array
- A column array
- A set

The existing matrix itself is used for storing markers.

Therefore:

```text
Extra Space = O(1)
```

---

# 🚫 Brute Force Approach

A straightforward approach would create another matrix or copy the original matrix.

For example:

```python
copy = [[...]]
```

Then whenever a zero is found, we could modify the corresponding row and column in the copy.

However, this requires:

```text
O(mn)
```

extra space.

For a matrix of size `200 × 200`, that is unnecessary.

---

# 🚫 O(m + n) Approach

A better approach is to store zero rows and columns separately:

```python
zero_rows = set()
zero_cols = set()
```

This requires:

```text
O(m + n)
```

space.

This is better than `O(mn)`, but still not optimal.

The marker approach improves this to:

```text
O(1)
```

extra space.

---

# 🔄 Approach Comparison

| Approach | Time | Extra Space | In-Place |
|---|---:|---:|---|
| Copy Matrix | O(mn) | O(mn) | ❌ |
| Row + Column Arrays | O(mn) | O(m+n) | ❌ |
| First Row/Column Markers | **O(mn)** | **O(1)** | **✅** |

---

# 🧠 Pattern Learned

## Pattern: In-Place Matrix Marking

This problem teaches an important technique:

> **Use part of the input structure as storage for metadata.**

Instead of:

```text
Create extra arrays
```

we use:

```text
First row → column markers
First column → row markers
```

This is a common technique in space-optimized matrix problems.

---

# 🔑 Most Important Concept

Remember this mapping:

```text
matrix[0][c] → Is column c supposed to be zero?

matrix[r][0] → Is row r supposed to be zero?

rowZero       → Is the first row supposed to be zero?

matrix[0][0]  → Is the first column supposed to be zero?
```

This is the heart of the solution.

---

# 🧪 Edge Cases

## 1. Matrix With No Zero

```text
[
    [1,2],
    [3,4]
]
```

Nothing changes:

```text
[
    [1,2],
    [3,4]
]
```

---

## 2. Single Element — Zero

```text
[
    [0]
]
```

Output:

```text
[
    [0]
]
```

---

## 3. Single Element — Non-Zero

```text
[
    [5]
]
```

Output:

```text
[
    [5]
]
```

---

## 4. Entire First Row Contains a Zero

```text
[
    [1,0,3],
    [4,5,6],
    [7,8,9]
]
```

The first row must become:

```text
[0,0,0]
```

and column `1` must also become zero:

```text
[
    [0,0,0],
    [4,0,6],
    [7,0,9]
]
```

This is why `rowZero` is necessary.

---

## 5. Zero in First Column

```text
[
    [1,2,3],
    [0,5,6],
    [7,8,9]
]
```

The first column becomes zero:

```text
[
    [0,2,3],
    [0,0,0],
    [0,8,9]
]
```

This is why `matrix[0][0]` is used to remember the first-column state.

---

## 6. Zero at `[0][0]`

```text
[
    [0,2],
    [3,4]
]
```

Both the first row and first column must become zero:

```text
[
    [0,0],
    [0,4]
]
```

The algorithm handles this using:

```python
matrix[0][0]
```

and:

```python
rowZero
```

independently.

---

# 🎯 Interview Explanation

A concise interview explanation:

> "I use the first row and first column of the matrix as marker storage instead of creating additional arrays. Whenever I find a zero at `(r,c)`, I mark `matrix[r][0]` and `matrix[0][c]` as zero. The special case is the first row because `matrix[0][0]` belongs to both the first row and first column, so I use a separate boolean `rowZero` to track whether the first row originally contained a zero. After marking, I process the inner matrix using those markers, then handle the first column and first row separately. This gives O(mn) time and O(1) extra space."

---

# 📚 DSA Concepts

This problem reinforces:

- 2D Arrays
- Matrix Traversal
- In-Place Algorithms
- Constant-Space Techniques
- Array Indexing
- Marker Technique
- Space Optimization

---

# 🔥 Most Important Takeaways

### 1. Use the first row as column markers

```python
matrix[0][c] = 0
```

### 2. Use the first column as row markers

```python
matrix[r][0] = 0
```

### 3. Handle first row separately

```python
rowZero = True
```

### 4. Process the inner matrix first

```python
for r in range(1, rows):
    for c in range(1, cols):
```

### 5. Handle boundaries last

```text
First column → First row
```

This preserves all marker information until it is no longer needed.

---

## 🏷️ Tags

`Array` `Matrix` `In-Place` `Constant Space` `Matrix Traversal` `LeetCode` `Python` `DSA` `Medium` `Interview Preparation`

---

## 🔗 Problem

[LeetCode #73 — Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

---

## ✅ Status

**Solved:** ✅  
**Difficulty:** Medium  
**Language:** Python  
**Time Complexity:** O(m × n)  
**Space Complexity:** O(1)  
**Pattern:** In-Place Matrix Marking
