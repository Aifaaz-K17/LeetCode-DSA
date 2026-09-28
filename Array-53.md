# 53. Maximum Subarray

[**LeetCode Problem**](https://leetcode.com/problems/maximum-subarray/)

**Difficulty:** Medium  
**Topics:** Array, Dynamic Programming, Kadane's Algorithm

## Problem Statement

Given an integer array `nums`, find the **subarray with the largest sum** and return its sum.

A subarray must contain **at least one element** and consist of consecutive elements from the original array.

---

## Approach

### Kadane's Algorithm

This problem can be solved efficiently using **Kadane's Algorithm** in `O(n)` time.

We maintain two values:

- `total` → the sum of the current subarray.
- `highest` → the largest subarray sum found so far.

For every element:

1. Add the current element to `total`.
2. Update `highest` with the maximum of:
   ```python
   highest
   total
   ```
3. If `total` becomes negative, reset it to `0`.

### Why Reset When `total < 0`?

If the current subarray has a negative sum:

```text
total < 0
```

carrying this sum into the next elements can only make their sum smaller.

Therefore, we discard the current subarray and start a new one from the next element.

---

## Python Solution

```python
class Solution:
    def maxSubArray(self, nums: list[int]) -> int:

        highest = float("-inf")
        total = 0

        for i in nums:
            total += i
            highest = max(highest, total)

            if total < 0:
                total = 0

        return highest
```

---

## Example

### Input

```text
nums = [-2,1,-3,4,-1,2,1,-5,4]
```

Tracking `total` and `highest`:

```text
i    total    highest
---------------------
-2    -2        -2
 1    -1        -1
-3    -3        -1
 4     4         4
-1     3         4
 2     5         5
 1     6         6
-5     1         6
 4     5         6
```

The maximum sum found is:

```text
6
```

The corresponding subarray is:

```text
[4,-1,2,1]
```

### Output

```text
6
```

---

## Why Initialize `highest` with Negative Infinity?

We use:

```python
highest = float("-inf")
```

instead of:

```python
highest = 0
```

because the array can contain **only negative numbers**.

For example:

```text
nums = [-5,-2,-8]
```

The correct answer is:

```text
-2
```

If `highest` started at `0`, the algorithm would incorrectly return `0`.

Using negative infinity ensures that the algorithm correctly handles arrays containing only negative values.

---

## Important Detail

The order of these operations matters:

```python
total += i
highest = max(highest, total)

if total < 0:
    total = 0
```

We update `highest` **before** resetting `total`.

This ensures that negative values can still be the answer when every element is negative.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - The array is traversed exactly once.

- **Space Complexity:** `O(1)`
  - Only two variables are used.

---

## Key Takeaways

Kadane's Algorithm is based on a simple idea:

```text
If the current sum becomes negative,
discard it and start a new subarray.
```

The core logic is:

```python
total += i
highest = max(highest, total)

if total < 0:
    total = 0
```

### Pattern

**Array → Kadane's Algorithm → Maximum Subarray Sum**

### Complexity

```text
Time  → O(n)
Space → O(1)
```

This is the standard linear-time approach for the Maximum Subarray problem.
