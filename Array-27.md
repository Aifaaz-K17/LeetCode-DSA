# 27. Remove Element

**Difficulty:** Easy  
**Language:** Python  
**DSA Pattern:** Two Pointers / In-Place Array Manipulation

---

## 📌 Problem Statement

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` **in-place** and return the number of elements that are not equal to `val`.

Let `k` be the number of elements that are not equal to `val`.

To solve the problem:

- The first `k` elements of `nums` must contain all elements that are not equal to `val`.
- Return `k`.
- The order of the remaining elements does not need to be preserved.
- Elements beyond index `k - 1` do not matter.
- Do not create another array to store the result.

### Example 1

```text
Input:
nums = [3, 2, 2, 3]
val = 3

Output:
2

Modified nums:
[2, 2, _, _]
```

Explanation: There are two elements that are not equal to `3`.

### Example 2

```text
Input:
nums = [0, 1, 2, 2, 3, 0, 4, 2]
val = 2

Output:
5

One possible modified nums:
[0, 1, 4, 0, 3, _, _, _]
```

Explanation: Five elements are not equal to `2`. Their order can change, so any arrangement containing the correct five elements in the first five positions is acceptable.

### Constraints

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

---

## 💡 Key Observation

The problem does not require preserving the order of the elements.

Therefore, instead of shifting every element after removing `val`, we can replace an unwanted element with the **last unchecked element**.

For example:

```text
nums = [3, 2, 2, 3]
val = 3
```

The first element is `3`, so replace it with the last unchecked element:

```text
[3, 2, 2, 3]
 ↑        ↑
 i       last

After replacement:

[3, 2, 2, 3]
```

The replacement happens to be another `3`. We must check the same index again because the replacement might also be unwanted.

We reduce the effective array length and continue until every element in the active portion has been checked.

**Key idea:** Replace unwanted elements with elements from the end, and shrink the active array instead of shifting elements.

---

## 🧠 Approach — Two Pointers

Your solution uses two variables:

```python
i = 0
n = len(nums)
```

Their roles are:

| Variable | Purpose |
|---|---|
| `i` | Index of the element currently being checked |
| `n` | Effective length of the unchecked portion of the array |

Initially, `n` is the original length of `nums`.

As unwanted elements are found, `n` decreases.

### Step 1 — Check the Current Element

```python
while i < n:
```

Continue while `i` is inside the active portion of the array.

### Step 2 — If the Element Equals `val`

```python
if nums[i] == val:
    n -= 1
    nums[i] = nums[n]
```

Two operations happen:

1. Decrease `n` because one element has been removed from the active portion.
2. Replace `nums[i]` with the last unchecked element.

**Important:** Do not increment `i` here. The replacement element has not been checked yet.

### Step 3 — If the Element Is Not `val`

```python
else:
    i += 1
```

The current element is valid, so leave it in place and move to the next index.

### Step 4 — Return the New Length

```python
return n
```

When the loop finishes, all elements in `nums[:n]` are different from `val`.

The value of `n` is the required answer `k`.

---

## 🔎 Detailed Walkthrough

Consider:

```text
nums = [3, 2, 2, 3]
val = 3
```

Initial state:

```text
i = 0
n = 4

nums = [3, 2, 2, 3]
```

### Iteration 1

Current element:

```text
nums[i] = nums[0] = 3
```

It equals `val`, so reduce `n` and replace the current element with the last unchecked element.

```python
n -= 1
nums[i] = nums[n]
```

State:

```text
nums = [3, 2, 2, 3]
i = 0
n = 3
```

The replacement is still `3`, so we must check index `0` again.

### Iteration 2

Current element:

```text
nums[0] = 3
```

It still equals `val`.

Reduce `n` again and replace the element:

```text
nums = [2, 2, 2, 3]
i = 0
n = 2
```

Now `nums[0]` is `2`, so it is valid.

### Iteration 3

Current element:

```text
nums[0] = 2
```

It does not equal `val`.

Move to the next index:

```text
i = 1
n = 2
```

### Iteration 4

Current element:

```text
nums[1] = 2
```

It does not equal `val`.

Move forward:

```text
i = 2
n = 2
```

The loop condition `i < n` is now false.

### Final Result

```text
nums = [2, 2, 2, 3]
k = 2
```

Only the first `k` elements matter:

```text
nums[:2] = [2, 2]
```

Therefore, the function returns:

```text
2
```

The remaining elements are irrelevant.

---

## 🧩 Algorithm

```text
1. Initialize i = 0.
2. Set n = len(nums).

3. While i < n:
   a. If nums[i] == val:
      - Decrease n by 1.
      - Replace nums[i] with nums[n].
      - Do not increment i.
   b. Otherwise:
      - Increment i.

4. Return n.
```

---

## 💻 Python Solution

```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        i = 0
        n = len(nums)

        while i < n:
            if nums[i] == val:
                n -= 1
                nums[i] = nums[n]
            else:
                i += 1

        return n
```

---

## 🔍 Understanding the Important Details

### 1. Why Decrease `n` Before Replacing?

```python
n -= 1
nums[i] = nums[n]
```

After decreasing `n`, `nums[n]` refers to the last element in the active portion of the array.

That element is moved into the position of the unwanted value.

The active portion is now one element shorter.

### 2. Why Not Increment `i` After Replacement?

Suppose:

```text
nums = [2, 3, 4, 5]
val = 3
```

At index `1`, the element is `3`.

After replacement:

```text
nums = [2, 5, 4, 5]
```

The element `5` has not been checked yet. If we immediately incremented `i`, we would skip it.

Instead, we check the same index again.

This guarantees that every element remaining in the active portion is examined.

### 3. Why Can the Order Change?

The problem explicitly allows the order of valid elements to change.

For example:

```text
Original:
[1, 2, 3, 4, 5]

Remove 2:
[1, 5, 3, 4, _]
```

The first four elements contain the correct remaining values, even though their order changed.

This freedom allows the algorithm to avoid shifting all subsequent elements.

### 4. Why Return `n` Instead of `len(nums)`?

The physical list length does not change in Python. Only the **effective length** decreases.

For example:

```text
nums = [2, 2, 2, 3]
n = 2
```

The list still contains four positions, but only the first two positions are part of the answer.

Therefore, return `n`, not `len(nums)`.

---

## ⏱️ Complexity Analysis

Let `N` be the original length of `nums`.

### Time Complexity

**O(N)**

Each iteration either advances `i` or decreases `n`. Both pointers move in only one direction, and neither can move more than `N` times.

Therefore, the total number of operations is linear.

### Extra Space Complexity

**O(1)**

The solution uses only two integer variables, `i` and `n`.

The original array is modified directly, and no additional array is created.

---

## 🐢 Alternative Approach — Slow and Fast Pointers

Another common solution uses a write pointer to retain only the elements that do not equal `val`.

```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        k = 0

        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]
                k += 1

        return k
```

### How It Works

- `i` scans every element.
- `k` tracks the next position for a valid element.
- Whenever `nums[i] != val`, copy it to `nums[k]` and increment `k`.
- Return `k`.

### Complexity

- **Time:** O(N)
- **Extra space:** O(1)

This approach preserves the relative order of the retained elements.

Your solution is particularly useful when **the order does not matter**, because it replaces unwanted elements with elements from the end.

---

## 📊 Approach Comparison

| Approach | Time | Extra Space | Preserves Order? |
|---|---|---|---|
| Repeated shifting | O(N²) worst case | O(1) | Yes |
| Slow and fast pointers | O(N) | O(1) | Yes |
| Replace with last unchecked element | O(N) | O(1) | No |

**Recommended approach for this problem:** Replace with the last unchecked element, since the problem allows the order to change.

If order preservation is required, use the slow and fast pointer approach.

---

## ⚠️ Edge Cases

| Input | `val` | Expected `k` | Explanation |
|---|---:|---:|---|
| `[]` | `0` | `0` | Empty array |
| `[3, 3]` | `3` | `0` | All elements must be removed |
| `[1, 2, 3]` | `4` | `3` | No matching elements |
| `[2, 2, 2]` | `2` | `0` | All elements match |
| `[1, 2, 3, 4]` | `2` | `3` | One element is removed |
| `[3, 1, 3, 2]` | `3` | `2` | Matching values at both ends |

---

## 🎯 DSA Pattern

### Two Pointers / In-Place Array Manipulation

This problem demonstrates how pointer movement can eliminate unwanted elements without allocating additional memory.

Recognize this pattern when a problem asks you to:

- Remove particular values from an array.
- Compact valid elements in-place.
- Return the number of retained elements.
- Modify an array using O(1) extra space.

### Important Distinction

There are two useful variants:

**1. Slow and fast pointers**
- Preserve the relative order.
- Write valid elements toward the beginning.

**2. Replace with the last element**
- Order does not need to be preserved.
- Replace unwanted values with unchecked elements from the end.

Always check whether the problem requires preserving order before choosing an approach.

---

## 🧠 Interview Explanation

> "Since the order of elements does not need to be preserved, I maintain an index `i` for the current element and an effective length `n`. When `nums[i]` equals the target value, I reduce `n` and replace that element with the last unchecked element. I keep `i` unchanged because the replacement has not yet been checked. If the current element is valid, I increment `i`. When `i` reaches `n`, the first `n` elements contain no occurrences of the target. The algorithm runs in O(N) time and O(1) extra space."

---

## 🔑 Key Takeaways

- Modify the array in-place rather than creating another array.
- Use the last unchecked element to replace unwanted values.
- Do not increment `i` immediately after a replacement.
- Return the effective length, not the physical length of the list.
- Order preservation determines which two-pointer variant is appropriate.
- The solution achieves **O(N) time and O(1) extra space**.

---

## 📚 DSA Concepts Used

- Two Pointers
- In-Place Array Manipulation
- Array Compaction
- Swap / Replace with Last Element
- Effective Array Length
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
#RemoveElement
#Easy
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(N)  
**Extra Space Complexity:** O(1)  
**Pattern:** Two Pointers / In-Place Array Manipulation
