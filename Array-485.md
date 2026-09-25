# 485. Max Consecutive Ones

[**LeetCode Problem**](https://leetcode.com/problems/max-consecutive-ones/)

**Difficulty:** Easy  
**Topics:** Array

## Problem Statement

Given a binary array `nums`, return the **maximum number of consecutive `1`s** in the array.

A binary array contains only:

```text
0 or 1
```

---

## Approach

### Counting Consecutive Ones

We use two variables:

- `count` → stores the current number of consecutive `1`s.
- `max_count` → stores the maximum number of consecutive `1`s found so far.

### Algorithm

1. Initialize:
   ```python
   count = 0
   max_count = 0
   ```

2. Traverse the array.
3. If the current element is `1`:
   - Increment `count`.
4. If the current element is `0`:
   - Compare `count` with `max_count`.
   - Reset `count` to `0`.
5. After the loop, compare `count` with `max_count` one final time.

The final comparison is important because the longest sequence of `1`s may occur **at the end of the array**.

---

## Python Solution

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: list[int]) -> int:

        count = 0
        max_count = 0

        for i in range(0, len(nums)):
            if nums[i] == 1:
                count += 1
            else:
                max_count = max(max_count, count)
                count = 0

        return max(max_count, count)
```

---

## Example

### Input

```text
nums = [1,1,0,1,1,1]
```

### Execution

```text
1 → count = 1
1 → count = 2
0 → max_count = 2, count = 0
1 → count = 1
1 → count = 2
1 → count = 3
```

The array ends with `1`s, so `count` must be checked after the loop:

```text
max(2, 3) = 3
```

### Output

```text
3
```

---

## Another Example

### Input

```text
nums = [1,0,1,1,0,1]
```

Tracking the consecutive `1`s:

```text
1 → count = 1
0 → max_count = 1, count = 0

1 → count = 1
1 → count = 2
0 → max_count = 2, count = 0

1 → count = 1
```

Final comparison:

```text
max(2, 1) = 2
```

### Output

```text
2
```

---

## Why Do We Need the Final `max()`?

Consider:

```text
nums = [0,1,1,1]
```

During the loop, `max_count` is only updated when a `0` is encountered.

Since the array ends with `1`s:

```text
max_count = 0
count = 3
```

Therefore, we need:

```python
return max(max_count, count)
```

Without this final comparison, the answer would incorrectly be `0`.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Each element is visited exactly once.

- **Space Complexity:** `O(1)`
  - Only two variables are used.

---

## Key Takeaways

This problem demonstrates a simple **counting / linear scan pattern**.

The important idea is:

```text
1 → increase count
0 → reset count
```

while continuously keeping track of the maximum:

```python
max_count = max(max_count, count)
```

### Pattern

**Array → Linear Traversal → Counting Consecutive Elements**

```text
Current streak → count
Best streak    → max_count
```
