# 88. Merge Sorted Array

**Difficulty:** Easy  
**Language:** Python  
**DSA Pattern:** Two Pointers / In-Place Merge

---

## 📌 Problem Statement

You are given two integer arrays, `nums1` and `nums2`, sorted in non-decreasing order, and two integers, `m` and `n`, representing the number of valid elements in each array.

Merge the two arrays into a single sorted array and store the result **inside `nums1`**.

- The first `m` elements of `nums1` are valid elements.
- The last `n` positions of `nums1` are empty space represented by zeros and should not be treated as input elements.
- `nums2` contains `n` valid elements.
- The function must modify `nums1` in-place rather than return a new array.

### Example 1

```text
Input:
nums1 = [1, 2, 3, 0, 0, 0]
m = 3
nums2 = [2, 5, 6]
n = 3

Output:
[1, 2, 2, 3, 5, 6]
```

### Example 2

```text
Input:
nums1 = [1]
m = 1
nums2 = []
n = 0

Output:
[1]
```

### Example 3

```text
Input:
nums1 = [0]
m = 0
nums2 = [1]
n = 1

Output:
[1]
```

### Constraints

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-10^9 <= nums1[i], nums2[j] <= 10^9`

---

## 💡 Key Observation

Both arrays are already sorted. We do not need to sort the combined array again.

The main challenge is that `nums1` already contains its valid elements at the beginning. If we merge from the beginning, we may overwrite elements that have not yet been processed.

**Solution: Merge from the end toward the beginning.**

The extra space at the end of `nums1` allows us to place the largest remaining element without destroying any unprocessed values.

---

## 🧠 Approach — Three Pointers

We use three pointers:

```python
last = m + n - 1
i = m - 1
j = n - 1
```

Their purposes are:

| Pointer | Initial Position | Purpose |
|---|---:|---|
| `i` | `m - 1` | Last valid element in `nums1` |
| `j` | `n - 1` | Last element in `nums2` |
| `last` | `m + n - 1` | Position where the next largest element goes |

### Step 1 — Compare the Largest Remaining Elements

```python
if i >= 0 and nums1[i] > nums2[j]:
    nums1[last] = nums1[i]
    i -= 1
else:
    nums1[last] = nums2[j]
    j -= 1
```

Because both arrays are sorted, their last unprocessed elements are their largest remaining elements.

- If `nums1[i]` is larger, place it at `nums1[last]`.
- Otherwise, place `nums2[j]` at `nums1[last]`.

Then move the pointer belonging to the selected element.

### Step 2 — Move the Destination Pointer

```python
last -= 1
```

After placing an element, move one position left to fill the next position.

### Step 3 — Stop When `nums2` Is Exhausted

```python
while j >= 0:
```

We continue until all elements of `nums2` have been placed.

If `nums2` becomes empty, any remaining elements in `nums1` are already in their correct positions.

---

## 🔎 Detailed Walkthrough

Consider:

```text
nums1 = [1, 2, 3, 0, 0, 0]
m = 3

nums2 = [2, 5, 6]
n = 3
```

### Initial State

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]

i = 2
j = 2
last = 5
```

We compare the largest remaining elements and fill `nums1` from right to left.

### Iteration 1

Compare:

```text
nums1[i] = 3
nums2[j] = 6
```

Since `6 > 3`, place `6` at index `5`.

```text
nums1 = [1, 2, 3, 0, 0, 6]
```

Update:

```text
j = 1
last = 4
```

### Iteration 2

Compare:

```text
nums1[i] = 3
nums2[j] = 5
```

Since `5 > 3`, place `5` at index `4`.

```text
nums1 = [1, 2, 3, 0, 5, 6]
```

Update:

```text
j = 0
last = 3
```

### Iteration 3

Compare:

```text
nums1[i] = 3
nums2[j] = 2
```

Since `3 > 2`, place `3` at index `3`.

```text
nums1 = [1, 2, 3, 3, 5, 6]
```

Update:

```text
i = 1
last = 2
```

### Iteration 4

Compare:

```text
nums1[i] = 2
nums2[j] = 2
```

The values are equal, so the `else` branch places the element from `nums2`.

```text
nums1 = [1, 2, 2, 3, 5, 6]
```

Update:

```text
j = -1
last = 1
```

The loop stops because all elements from `nums2` have been merged.

### Final Result

```text
nums1 = [1, 2, 2, 3, 5, 6]
```

The array is sorted, and no additional array was required.

---

## 🧩 Algorithm

```text
1. Set i = m - 1.
2. Set j = n - 1.
3. Set last = m + n - 1.

4. While j >= 0:
   a. If i >= 0 and nums1[i] > nums2[j]:
      - Place nums1[i] at nums1[last].
      - Decrement i.
   b. Otherwise:
      - Place nums2[j] at nums1[last].
      - Decrement j.
   c. Decrement last.

5. Stop when all elements of nums2 are placed.
```

---

## 💻 Python Solution

```python
class Solution:
    def merge(self, nums1: list[int], m: int, nums2: list[int], n: int) -> None:
        """
        Merge nums2 into nums1 in-place.
        Do not return anything.
        """
        last = m + n - 1
        i, j = m - 1, n - 1

        while j >= 0:
            if i >= 0 and nums1[i] > nums2[j]:
                nums1[last] = nums1[i]
                i -= 1
            else:
                nums1[last] = nums2[j]
                j -= 1

            last -= 1
```

---

## 🔍 Understanding the Important Conditions

### 1. Why `while j >= 0`?

```python
while j >= 0:
```

Every element of `nums2` must be copied into the merged result.

If `j` becomes `-1`, all elements of `nums2` have been placed.

Any remaining elements in `nums1` are already in their correct positions.

### 2. Why Check `i >= 0`?

```python
if i >= 0 and nums1[i] > nums2[j]:
```

The valid elements in `nums1` may be exhausted before those in `nums2`.

For example:

```text
nums1 = [0, 0, 0]
m = 0

nums2 = [1, 2, 3]
n = 3
```

Here, `i = -1` initially. We must copy elements from `nums2` rather than access an invalid logical position in `nums1`.

The `i >= 0` condition handles this case safely.

### 3. Why Merge from the End?

Suppose we merge from the beginning:

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [2, 5, 6]
```

Writing `2` into an early position of `nums1` could overwrite the original `2` or `3` before they are processed.

Merging from the end prevents this problem because each destination position is at or beyond the last unprocessed element of `nums1`.

### 4. Why Is There No Final Copying Loop for `nums1`?

When `nums2` is exhausted, the remaining elements in `nums1` are already positioned correctly.

For example:

```text
nums1 = [1, 2, 3, 0, 0, 0]
nums2 = [4, 5, 6]
```

After placing `6`, `5`, and `4` from right to left:

```text
nums1 = [1, 2, 3, 4, 5, 6]
```

The original elements `1`, `2`, and `3` do not need to move.

---

## ⏱️ Complexity Analysis

Let `m` and `n` be the numbers of valid elements in `nums1` and `nums2`.

### Time Complexity

**O(m + n)**

Each element is processed at most once. The pointers move only toward the beginning of the arrays.

### Extra Space Complexity

**O(1)**

The algorithm uses only three pointers: `i`, `j`, and `last`.

It modifies `nums1` directly without creating another array.

---

## 🐢 Alternative Approach — Concatenate and Sort

A simpler alternative is to copy the elements from `nums2` and sort the result.

```python
class Solution:
    def merge(self, nums1: list[int], m: int, nums2: list[int], n: int) -> None:
        nums1[m:] = nums2
        nums1.sort()
```

### Complexity

- **Time:** O((m + n) log(m + n))
- **Extra space:** Depends on the sorting implementation.

This approach is concise, but it does not take full advantage of the fact that both input arrays are already sorted.

---

## 📊 Approach Comparison

| Approach | Time Complexity | Extra Space | Notes |
|---|---|---|---|
| Concatenate and sort | O((m+n) log(m+n)) | Depends on sorting | Simple but does unnecessary sorting |
| Merge from the beginning | O(m+n) | May require extra storage | Can overwrite unprocessed elements |
| Three pointers from the end | O(m+n) | O(1) | Optimal in-place approach |

**Recommended approach:** Three pointers from the end.

---

## ⚠️ Edge Cases

| Input | Expected `nums1` | Reason |
|---|---|---|
| `nums1 = [1], m = 1; nums2 = [], n = 0` | `[1]` | Nothing to merge |
| `nums1 = [0], m = 0; nums2 = [1], n = 1` | `[1]` | First array has no valid elements |
| `nums1 = [1,2,3,0,0,0]; nums2 = [4,5,6]` | `[1,2,3,4,5,6]` | All elements of `nums2` are larger |
| `nums1 = [4,5,6,0,0,0]; nums2 = [1,2,3]` | `[1,2,3,4,5,6]` | All elements of `nums2` are smaller |
| `nums1 = [1,2,0,0]; nums2 = [1,2]` | `[1,1,2,2]` | Duplicate values |
| `nums1 = [-3,0,0]; nums2 = [-2,1]` | `[-3,-2,1]` | Negative values |

---

## 🎯 DSA Pattern

### Two Pointers + In-Place Merge

This problem is a classic example of merging two sorted arrays using pointers.

The key principle is:

**When the destination array has free space at the end, merge from right to left.**

This technique is useful when:
- Merging sorted arrays in-place.
- Preserving existing elements without allocating another array.
- Combining sorted data efficiently.
- Solving array manipulation problems with limited extra memory.

---

## 🧠 Interview Explanation

> "Both arrays are already sorted, and `nums1` has enough space to hold the final result. I initialize one pointer at the last valid element of each array and a third pointer at the final position of `nums1`. I compare the two largest remaining elements and place the larger one at the destination pointer, moving the corresponding pointer backward. I continue until all elements from `nums2` have been placed. This prevents overwriting unprocessed values and achieves O(m+n) time with O(1) extra space."

---

## 🔑 Key Takeaways

- Use the existing sorted order instead of sorting again.
- Start from the end to avoid overwriting valid elements.
- The three pointers track both input positions and the destination.
- The `i >= 0` check handles an exhausted first array.
- The loop only needs to continue while `j >= 0`.
- The solution modifies `nums1` in-place.
- The optimal solution runs in **O(m+n) time and O(1) extra space**.

---

## 📚 DSA Concepts Used

- Two Pointers
- In-Place Array Manipulation
- Merging Sorted Arrays
- Reverse Traversal
- Greedy Selection
- Time and Space Complexity

---

## 🏷️ Tags

```text
#LeetCode
#Python
#DSA
#TwoPointers
#Arrays
#InPlace
#MergeSortedArray
#Easy
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(m+n)  
**Extra Space Complexity:** O(1)  
**Pattern:** Two Pointers / In-Place Merge
