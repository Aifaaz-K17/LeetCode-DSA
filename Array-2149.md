# 2149. Rearrange Array Elements by Sign

[**LeetCode Problem**](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

**Difficulty:** Medium  
**Topics:** Array, Two Pointers, Simulation

---

## Problem Statement

You are given a **0-indexed** integer array `nums` of even length containing an equal number of positive and negative integers.

Rearrange the array so that:

1. Every consecutive pair of integers has **opposite signs**.
2. The **relative order** of positive integers is preserved.
3. The **relative order** of negative integers is preserved.
4. The rearranged array **starts with a positive integer**.

Return the rearranged array.

### Example 1

```text
Input:  nums = [3,1,-2,-5,2,-4]

Output: [3,-2,1,-5,2,-4]

Explanation:
Positive integers: [3,1,2]
Negative integers: [-2,-5,-4]

The positive and negative elements are placed alternately while
preserving their original relative order.
````

### Example 2

```text
Input:  nums = [-1,1]

Output: [1,-1]
```

---

## Approach

### Separate Positions for Positive and Negative Numbers

Since the answer must:

* Start with a positive number.
* Alternate positive and negative numbers.
* Preserve the relative order within each sign.

We can directly assign positions based on the sign.

Initialize:

```python
pos = 0
neg = 1
```

Here:

* `pos` points to the next available **even index** for a positive number.
* `neg` points to the next available **odd index** for a negative number.

The indices will therefore look like:

```text
Positive positions:  0 → 2 → 4 → 6 → ...
Negative positions:  1 → 3 → 5 → 7 → ...
```

For every element in `nums`:

* If it is positive, place it at `result[pos]` and increase `pos` by `2`.
* If it is negative, place it at `result[neg]` and increase `neg` by `2`.

Because we process `nums` from left to right, the relative order of positive and negative elements is automatically preserved.

---

## Python Solution

```python
class Solution:
    def rearrangeArray(self, nums: list[int]) -> list[int]:
        n = len(nums)
        result = [0] * n

        pos, neg = 0, 1

        for i in range(n):
            if nums[i] >= 0:
                result[pos] = nums[i]
                pos += 2
            else:
                result[neg] = nums[i]
                neg += 2

        return result
```

---

## Example Walkthrough

Consider:

```text
nums = [3, 1, -2, -5, 2, -4]
```

Initial positions:

```text
result = [0, 0, 0, 0, 0, 0]

Positive index = 0
Negative index = 1
```

Process each element:

| Element | Sign     | Position Used | Result             |
| ------: | -------- | ------------- | ------------------ |
|     `3` | Positive | `0`           | `[3,0,0,0,0,0]`    |
|     `1` | Positive | `2`           | `[3,0,1,0,0,0]`    |
|    `-2` | Negative | `1`           | `[3,-2,1,0,0,0]`   |
|    `-5` | Negative | `3`           | `[3,-2,1,-5,0,0]`  |
|     `2` | Positive | `4`           | `[3,-2,1,-5,2,0]`  |
|    `-4` | Negative | `5`           | `[3,-2,1,-5,2,-4]` |

Final result:

```text
[3,-2,1,-5,2,-4]
```

---

## Why Does This Preserve the Order?

Suppose the positive numbers are:

```text
[3, 1, 2]
```

They are encountered in exactly this order while traversing `nums`.

They are placed at:

```text
index 0 → 3
index 2 → 1
index 4 → 2
```

Therefore their relative order remains:

```text
3 → 1 → 2
```

The same applies to the negative numbers.

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

The array is traversed exactly once.

### Space Complexity

```text
O(n)
```

A separate `result` array of size `n` is created.

---

## Key Takeaways

* Use **even indices** for positive numbers.
* Use **odd indices** for negative numbers.
* Start positive placement at index `0`.
* Increment position pointers by `2` to maintain alternating signs.
* Traversing the original array from left to right preserves the relative order.
* Because the problem guarantees an equal number of positive and negative elements, every required position can be filled.

---

## Pattern

**Array + Index Placement**

### Core Pattern

```text
Positive → 0, 2, 4, 6, ...
Negative → 1, 3, 5, 7, ...
```

This is a useful technique when a problem requires elements to be placed according to a fixed positional pattern.

```
```
