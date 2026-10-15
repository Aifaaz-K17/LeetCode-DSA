# 48. Rotate Image

**Difficulty:** Medium  
**Language:** Python  
**DSA Pattern:** Matrix Manipulation / In-Place Transformation

---

## 📌 Problem Statement

You are given an `n × n` 2D matrix representing an image. Rotate the image **90 degrees clockwise**.

The rotation must be performed **in-place**, meaning the original matrix must be modified directly without allocating another 2D matrix.

### Example 1

**Input:**

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

**Output:**

```python
[
    [7, 4, 1],
    [8, 5, 2],
    [9, 6, 3]
]
```

### Example 2

**Input:**

```python
matrix = [
    [5, 1, 9, 11],
    [2, 4, 8, 10],
    [13, 3, 6, 7],
    [15, 14, 12, 16]
]
```

**Output:**

```python
[
    [15, 13, 2, 5],
    [14, 3, 4, 1],
    [12, 6, 8, 9],
    [16, 7, 10, 11]
]
```

### Constraints

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

---

## 💡 Key Observation

A 90° clockwise rotation can be achieved through **two simple matrix operations**:

1. Reverse the order of the rows.
2. Transpose the matrix.

Instead of moving every element individually to its final position, we can combine these operations to rotate the matrix in-place.

**The key formula is:**

```text
90° Clockwise Rotation
        =
Reverse Rows + Transpose
```

---

## 🧠 Approach

### Step 1 — Reverse the Rows

Your code:

```python
matrix.reverse()
```

This reverses the order of the rows.

Before reversing:

```text
1 2 3
4 5 6
7 8 9
```

After reversing:

```text
7 8 9
4 5 6
1 2 3
```

Notice that the elements inside each row remain in the same order. Only the positions of the rows change.

### Step 2 — Transpose the Matrix

Your code:

```python
for i in range(len(matrix)):
    for j in range(i + 1, len(matrix)):
        matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

A **transpose** converts rows into columns and columns into rows.

The element at position `(i, j)` is swapped with the element at `(j, i)`.

For example:

```text
Before transpose:       After transpose:

7 8 9                   7 4 1
4 5 6                   8 5 2
1 2 3                   9 6 3
```

The loop starts at `j = i + 1` because we only need to swap elements above the main diagonal with their corresponding elements below it.

The diagonal elements remain unchanged.

---

## 🔎 Detailed Walkthrough

Consider:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

### Phase 1 — Reverse Rows

Execute:

```python
matrix.reverse()
```

The matrix becomes:

```text
7 8 9
4 5 6
1 2 3
```

### Phase 2 — Transpose

Initially, the outer loop starts at `i = 0`.

**Swap 1:** `matrix[0][1]` with `matrix[1][0]`

```text
Swap 8 and 4

7 4 9
8 5 6
1 2 3
```

**Swap 2:** `matrix[0][2]` with `matrix[2][0]`

```text
Swap 9 and 1

7 4 1
8 5 6
9 2 3
```

Next, `i = 1`.

**Swap 3:** `matrix[1][2]` with `matrix[2][1]`

```text
Swap 6 and 2

7 4 1
8 5 2
9 6 3
```

The transpose is complete.

### Final Result

```text
7 4 1
8 5 2
9 6 3
```

The matrix has been rotated 90° clockwise without allocating another matrix.

---

## 🧩 Algorithm

```text
1. Reverse the order of the rows.
2. For each row index i:
   a. Iterate through column indices j from i + 1 to n - 1.
   b. Swap matrix[i][j] and matrix[j][i].
3. The matrix is now rotated 90° clockwise.
```

---

## 💻 Python Solution

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        matrix.reverse()

        for i in range(len(matrix)):
            for j in range(i + 1, len(matrix)):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

**Note:** LeetCode provides `List` in its standard Python environment. If using built-in generic type hints, `list[list[int]]` can also be used.

---

## 🔍 Understanding the Important Details

### 1. Why Does Reversing Rows Help?

A clockwise rotation moves the first row into the last column, the second row into the second-last column, and so on.

Reversing the rows establishes the correct vertical order before transposition.

### 2. Why Transpose After Reversing?

Transposition swaps the row and column indices:

```text
matrix[i][j] ↔ matrix[j][i]
```

Combining this with row reversal produces the required clockwise orientation.

### 3. Why Does the Inner Loop Start at `i + 1`?

```python
for j in range(i + 1, len(matrix)):
```

A transpose swaps each pair of symmetric elements only once.

For a 3 × 3 matrix, the relevant pairs are:

```text
(0,1) ↔ (1,0)
(0,2) ↔ (2,0)
(1,2) ↔ (2,1)
```

If we started at `j = 0`, we would swap the same pair twice, undoing the first swap.

The main diagonal contains positions where `i == j`; those elements do not need to move during transposition.

### 4. Why Is This In-Place?

Both operations modify the original matrix:

- `matrix.reverse()` reverses the row references in-place.
- Tuple assignment swaps individual elements without creating another matrix.

No separate `n × n` result matrix is created.

---

## ⏱️ Complexity Analysis

Let `n` be the number of rows and columns.

### Time Complexity

**O(n²)**

Reversing the rows takes O(n) time. Transposing visits approximately half of the matrix's off-diagonal elements, which is O(n²).

Therefore, the overall time complexity is **O(n²)**.

### Extra Space Complexity

**O(1)** auxiliary space.

The algorithm uses only loop variables and swaps elements directly in the existing matrix. Python's `reverse()` modifies the list in-place.

---

## 🐢 Alternative Approach — Direct Rotation

A direct method moves each element from its original position to its rotated position:

```text
matrix[i][j] → matrix[j][n - 1 - i]
```

This formula represents a 90° clockwise rotation.

For example, in a 3 × 3 matrix:

```text
matrix[0][0] → matrix[0][2]
matrix[0][1] → matrix[1][2]
matrix[0][2] → matrix[2][2]
```

However, moving elements directly in-place is more complicated because values may be overwritten before they are moved. A correct direct in-place implementation typically needs cycle-based swaps.

The reverse-rows-and-transpose approach is easier to implement and reason about.

---

## 📊 Approach Comparison

| Approach | Time | Extra Space | Notes |
|---|---|---|---|
| Reverse rows + transpose | O(n²) | O(1) | Simple and optimal |
| Create a new matrix | O(n²) | O(n²) | Easy, but violates the in-place requirement |
| Direct cycle-based rotation | O(n²) | O(1) | In-place, but more complex |

**Recommended approach:** Reverse rows, then transpose.

---

## ⚠️ Edge Cases

### 1. Single Element

```python
matrix = [[1]]
```

Output:

```python
[[1]]
```

Reversing and transposing a 1 × 1 matrix does not change it.

### 2. Two-by-Two Matrix

```python
matrix = [
    [1, 2],
    [3, 4]
]
```

Output:

```python
[
    [3, 1],
    [4, 2]
]
```

### 3. Negative Values

```python
matrix = [
    [-1, -2],
    [-3, -4]
]
```

Output:

```python
[
    [-3, -1],
    [-4, -2]
]
```

The algorithm works with negative and positive integers because it only rearranges elements.

---

## 🎯 DSA Pattern

### Matrix Manipulation / In-Place Transformation

This problem demonstrates how combining two simple operations can achieve a more complex matrix transformation.

Recognize this pattern when a problem asks you to:

- Rotate a square matrix.
- Transpose a matrix.
- Modify a matrix in-place.
- Rearrange rows and columns.
- Avoid allocating another matrix.

### Useful Rotation Formulas

For a clockwise 90° rotation:

```text
matrix[i][j] → matrix[j][n - 1 - i]
```

For transposition:

```text
matrix[i][j] ↔ matrix[j][i]
```

For a clockwise 90° rotation using simple operations:

```text
Reverse Rows + Transpose
```

For a counterclockwise 90° rotation, the corresponding operation is:

```text
Transpose + Reverse Rows
```

---

## 🧠 Interview Explanation

> "I rotate the matrix in-place using two operations. First, I reverse the order of its rows. Then, I transpose the matrix by swapping each element above the main diagonal with its corresponding element below the diagonal. These operations together produce a 90-degree clockwise rotation. The solution runs in O(n²) time and uses O(1) auxiliary space."

---

## 🔑 Key Takeaways

- A 90° clockwise rotation can be performed by reversing rows and transposing.
- Transposition swaps `matrix[i][j]` with `matrix[j][i]`.
- Starting the inner loop at `i + 1` avoids duplicate swaps.
- The matrix is modified directly without allocating another 2D array.
- The solution takes **O(n²) time and O(1) auxiliary space**.
- Understanding matrix transformations helps simplify more complex grid problems.

---

## 📚 DSA Concepts Used

- Matrix Manipulation
- Two-Dimensional Arrays
- Transposition
- In-Place Algorithms
- Nested Loops
- Index Mapping
- Time and Space Complexity

---

## 🏷️ Tags

```text
#LeetCode
#Python
#DSA
#Matrix
#TwoDimensionalArray
#InPlace
#MatrixRotation
#Medium
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(n²)  
**Auxiliary Space Complexity:** O(1)  
**Pattern:** Matrix Manipulation / In-Place Transformation
