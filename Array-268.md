# 268. Missing Number

[**LeetCode Problem**](https://leetcode.com/problems/missing-number/)

**Difficulty:** Easy  
**Topics:** Array, Math

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the **only number missing** from the array.

---

## Approach

### Sum Formula

The array contains `n` numbers, but the complete range contains:

```text
0, 1, 2, ..., n
```

Therefore, the expected sum of all numbers from `0` to `n` can be calculated using:

```text
Sum = n × (n + 1) / 2
```

We then calculate the actual sum of the elements in `nums`.

The missing number is:

```text
Missing Number = Expected Sum - Actual Sum
```

### Example

For:

```text
nums = [3,0,1]
```

The length is:

```text
n = 3
```

Expected numbers:

```text
[0,1,2,3]
```

Expected sum:

```text
3 × (3 + 1) / 2 = 6
```

Actual sum:

```text
3 + 0 + 1 = 4
```

Therefore:

```text
6 - 4 = 2
```

So the missing number is:

```text
2
```

---

## Python Solution

```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        result = 0
        n = len(nums)

        total = n * (n + 1) // 2

        for i in nums:
            result += i

        val = total - result

        return val
```

### Simplified Version

The same approach can be written more concisely using Python's `sum()`:

```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        n = len(nums)
        total = n * (n + 1) // 2

        return total - sum(nums)
```

---

## Why `//` Instead of `/`?

Your original code uses:

```python
total = len(nums) * (len(nums) + 1) / 2
```

In Python, `/` produces a floating-point number.

For example:

```python
6 / 2
```

returns:

```text
3.0
```

Since we are working with integers, integer division is more appropriate:

```python
6 // 2
```

returns:

```text
3
```

Therefore, `round()` is unnecessary.

---

## Example

### Input

```text
nums = [9,6,4,2,3,5,7,0,1]
```

### Step 1 — Find `n`

```text
n = 9
```

### Step 2 — Calculate expected sum

```text
9 × (9 + 1) / 2
= 45
```

### Step 3 — Calculate actual sum

```text
9 + 6 + 4 + 2 + 3 + 5 + 7 + 0 + 1
= 37
```

### Step 4 — Find missing number

```text
45 - 37 = 8
```

### Output

```text
8
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - We traverse the array once to calculate the actual sum.

- **Space Complexity:** `O(1)`
  - Only a few variables are used.
  - No additional data structure is created.

---

## Key Takeaways

- Use the mathematical sum formula to find the expected total.
- Subtract the actual array sum from the expected sum.
- The difference is the missing number.
- This satisfies the follow-up requirement of **O(n) time** and **O(1) extra space**.

### Pattern

**Array + Mathematics → Sum Difference**

```text
Expected Sum - Actual Sum = Missing Number
```

### Formula

```text
n × (n + 1) / 2
```
