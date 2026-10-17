# 54. Spiral Matrix

**Difficulty:** Medium  
**Topics:** Array, Matrix, Simulation  
**Pattern:** Boundary Traversal / Matrix Traversal  
**LeetCode:** [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

---

## 1. Problem Statement

Given an `m × n` matrix, return all its elements in **spiral order**.

Spiral order means traversing the matrix in this sequence:

1. Move from left to right across the top row.
2. Move from top to bottom down the rightmost column.
3. Move from right to left across the bottom row.
4. Move from bottom to top up the leftmost column.
5. Repeat the process for the remaining inner matrix until every element has been visited.

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
[1, 2, 3, 6, 9, 8, 7, 4, 5]
```

**Spiral traversal:**
```text
1 → 2 → 3
        ↓
4 → 5   6
↑       ↓
7 ← 8 ← 9
```

The elements are collected in clockwise spiral order, starting from the top-left corner.

### Example 2

**Input:**
```python
matrix = [
    [1,  2,  3,  4],
    [5,  6,  7,  8],
    [9, 10, 11, 12]
]
```

**Output:**
```python
[1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

### Constraints

- `m == len(matrix)`
- `n == len(matrix[i])`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

---

## 2. Key Observation

Instead of tracking the direction of movement for every individual element, we can maintain **four boundaries** that define the unvisited portion of the matrix.

Initially, the boundaries cover the entire matrix. After traversing one side, we move the corresponding boundary inward.

For example:

```text
top, left = 0, 0
bottom = number_of_rows - 1
right  = number_of_columns - 1
```

The traversal always follows four steps:

1. Traverse the top row from left to right.
2. Traverse the right column from top to bottom.
3. Traverse the bottom row from right to left.
4. Traverse the left column from bottom to top.

After each traversal, update the corresponding boundary so that the same elements are not visited again.

**Important:** Some boundary checks are necessary because the remaining matrix may contain only one row or one column.

---

## 3. Approach: Boundary Traversal

We use four variables:

| Variable | Meaning |
|---|---|
| `top` | Index of the first unvisited row |
| `bottom` | Index of the last unvisited row |
| `left` | Index of the first unvisited column |
| `right` | Index of the last unvisited column |

We also maintain a `result` list to store elements in spiral order.

### Step 1: Initialize the boundaries

```python
result = []

top, left = 0, 0
bottom, right = len(matrix) - 1, len(matrix[0]) - 1
```

Initially, the entire matrix is unvisited.

### Step 2: Traverse the top row

```python
for i in range(left, right + 1):
    result.append(matrix[top][i])

top += 1
```

- Visit the top row from left to right.
- Increase `top` because that row has been processed.

### Step 3: Traverse the right column

```python
for i in range(top, bottom + 1):
    result.append(matrix[i][right])

right -= 1
```

- Visit the rightmost column from top to bottom.
- Decrease `right` because that column has been processed.

Notice that traversal starts at the updated `top`, avoiding the top-right element that was already visited.

### Step 4: Traverse the bottom row

```python
if top <= bottom:
    for i in range(right, left - 1, -1):
        result.append(matrix[bottom][i])

    bottom -= 1
```

- First check that an unvisited bottom row still exists.
- Visit it from right to left.
- Decrease `bottom` to exclude the processed row.

The condition prevents duplicate traversal when the remaining matrix has no rows left.

### Step 5: Traverse the left column

```python
if left <= right:
    for i in range(bottom, top - 1, -1):
        result.append(matrix[i][left])

    left += 1
```

- Check that an unvisited left column still exists.
- Visit it from bottom to top.
- Increase `left` to exclude the processed column.

The condition prevents duplicate traversal when the remaining matrix has no columns left.

### Step 6: Repeat until no unvisited region remains

```python
while top <= bottom and left <= right:
```

The loop continues only while the remaining matrix has at least one row and one column.

When either condition becomes false, the traversal is complete.

---

## 4. Python Solution

```python
class Solution:
    def spiralOrder(self, matrix: list[list[int]]) -> list[int]:
        if not matrix or not matrix[0]:
            return []

        result = []

        top, left = 0, 0
        bottom, right = len(matrix) - 1, len(matrix[0]) - 1

        while top <= bottom and left <= right:

            # 1. Traverse the top row: left to right
            for i in range(left, right + 1):
                result.append(matrix[top][i])
            top += 1

            # 2. Traverse the right column: top to bottom
            for i in range(top, bottom + 1):
                result.append(matrix[i][right])
            right -= 1

            # 3. Traverse the bottom row: right to left
            if top <= bottom:
                for i in range(right, left - 1, -1):
                    result.append(matrix[bottom][i])
                bottom -= 1

            # 4. Traverse the left column: bottom to top
            if left <= right:
                for i in range(bottom, top - 1, -1):
                    result.append(matrix[i][left])
                left += 1

        return result
```

---

## 5. Detailed Dry Run

Consider the following matrix:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

Initially:

```text
top = 0, bottom = 2
left = 0, right = 2

result = []
```

### Iteration 1

**A. Traverse the top row**

Visit `1, 2, 3`.

```text
1 → 2 → 3
```

```python
result = [1, 2, 3]
top = 1
```

**B. Traverse the right column**

Visit `6, 9`.

```text
3
  ↓
6
  ↓
9
```

```python
result = [1, 2, 3, 6, 9]
right = 1
```

**C. Traverse the bottom row**

Visit `8, 7`.

```text
9 ← 8 ← 7
```

```python
result = [1, 2, 3, 6, 9, 8, 7]
bottom = 1
```

**D. Traverse the left column**

Visit `4`.

```text
4
↑
7
```

```python
result = [1, 2, 3, 6, 9, 8, 7, 4]
left = 1
```

The remaining boundaries are:

```text
top = 1, bottom = 1
left = 1, right = 1
```

Only the center element remains.

### Iteration 2

The loop condition is still true:

```python
top <= bottom and left <= right
```

Visit the remaining top row, which contains only `5`.

```python
result = [1, 2, 3, 6, 9, 8, 7, 4, 5]
top = 2
```

The right-column loop does not visit any elements because `top > bottom`. The bottom-row condition also fails.

The left-column condition fails after the right boundary is decreased.

Now `top > bottom`, so the `while` loop terminates.

**Final output:**

```python
[1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

## 6. Why Are the Boundary Checks Necessary?

The two checks prevent duplicate visits in matrices with a single remaining row or column.

### Case 1: A single row

```python
matrix = [[1, 2, 3, 4]]
```

Expected output:

```python
[1, 2, 3, 4]
```

After traversing the top row, `top` becomes `1`, while `bottom` remains `0`.

Therefore, `top <= bottom` is false. The bottom-row traversal is skipped, preventing the same row from being visited in reverse.

### Case 2: A single column

```python
matrix = [
    [1],
    [2],
    [3]
]
```

Expected output:

```python
[1, 2, 3]
```

After traversing the right column, `right` becomes `-1`, while `left` remains `0`.

Therefore, `left <= right` is false. The left-column traversal is skipped, preventing duplicate visits.

### Case 3: A rectangular matrix

The approach works for both square and rectangular matrices because the boundaries are based independently on the number of rows and columns.

---

## 7. Time and Space Complexity

Let `m` be the number of rows and `n` be the number of columns.

### Time Complexity: `O(m × n)`

Every matrix element is appended to `result` exactly once. The four traversal loops collectively visit all `m × n` elements.

Therefore:

\[
\boxed{O(mn)}
\]

### Auxiliary Space Complexity: `O(1)`

The algorithm uses only four boundary variables and a few loop variables, requiring constant extra working space.

However, the returned `result` list stores all `m × n` elements.

- **Auxiliary space excluding the output:** `O(1)`
- **Space including the output list:** `O(m × n)`

This distinction is important when discussing complexity in interviews.

---

## 8. Alternative Approaches

### Approach 1: Boundary traversal — optimal practical solution

Maintain `top`, `bottom`, `left`, and `right`, traversing one side at a time.

- Time: `O(m × n)`
- Auxiliary space excluding output: `O(1)`
- Advantage: Simple, efficient, and does not modify the matrix.

### Approach 2: Direction simulation

Use direction vectors for right, down, left, and up. Track visited cells or change direction when the next cell is outside the matrix or already visited.

- Time: `O(m × n)`
- Auxiliary space: `O(m × n)` if a separate visited matrix is used.
- Advantage: A flexible approach for more general path-traversal problems.
- Disadvantage: Requires additional state and boundary or visited-cell checks.

### Approach 3: Repeatedly remove outer layers

Collect the first row, rotate or transform the remaining matrix, and repeat.

- Time: Often `O(m × n)` or worse, depending on the implementation and copying.
- Auxiliary space: May be `O(m × n)` because of intermediate lists or matrix copies.
- Advantage: Can be concise.
- Disadvantage: Usually creates unnecessary intermediate data structures.

**Best choice:** Boundary traversal is the most suitable approach here because it is efficient, easy to explain, and uses constant auxiliary working space.

---

## 9. Common Mistakes

1. **Using `right` instead of `right + 1` in the top-row loop.** Python's `range()` excludes its ending value, so the right boundary must be included.
2. **Forgetting to update a boundary.** This can cause elements to be revisited or the loop to run indefinitely.
3. **Traversing the bottom row without checking `top <= bottom`.** This may duplicate elements when only one row remains.
4. **Traversing the left column without checking `left <= right`.** This may duplicate elements when only one column remains.
5. **Incorrect reverse ranges.** To include `left` while moving backward, use `range(right, left - 1, -1)`.
6. **Assuming the matrix is square.** The algorithm must initialize `bottom` from the number of rows and `right` from the number of columns.
7. **Returning `None` for an empty matrix.** Returning `[]` is more consistent with the declared return type.

---

## 10. Interview Explanation

“I use four boundaries—top, bottom, left, and right—to represent the unvisited portion of the matrix. In each iteration, I traverse the top row from left to right, the right column from top to bottom, the bottom row from right to left, and the left column from bottom to top. After each traversal, I move the corresponding boundary inward. I check whether the bottom row and left column still exist before traversing them, which prevents duplicate visits in single-row or single-column cases. Each element is processed once, so the time complexity is `O(m × n)`, and the auxiliary working space excluding the output is `O(1)`.”

---

## 11. Key Takeaways

- Use four boundaries to track the remaining unvisited matrix.
- Follow the clockwise order: **right → down → left → up**.
- Shrink the relevant boundary after each side is processed.
- Use conditional checks before traversing the bottom row and left column.
- Each element is visited exactly once.
- Time complexity is `O(m × n)`; auxiliary working space is `O(1)` excluding the output.

---

## 12. Repository Metadata

- **Problem Number:** 54
- **Problem Name:** Spiral Matrix
- **Difficulty:** Medium
- **Primary Pattern:** Boundary Traversal
- **Core Concepts:** Arrays, 2D matrices, loops, boundary conditions
- **Time Complexity:** `O(m × n)`
- **Auxiliary Space Complexity:** `O(1)` excluding output
- **Status:** Solved
- **Tags:** `leetcode`, `python`, `dsa`, `matrix`, `array`, `boundary-traversal`, `medium`

