# 189. Rotate Array

[**LeetCode Problem**](https://leetcode.com/problems/rotate-array/)

**Difficulty:** Medium  
**Topics:** Array, Math, Two Pointers

## Problem Statement

Given an integer array `nums`, rotate the array to the **right by `k` steps**, where `k` is non-negative.

---

## Approach

### Array Reversal Technique

We can solve this problem in-place using **three reversals**.

For example:

```text
nums = [1,2,3,4,5,6,7]
k = 3
```

We want:

```text
[5,6,7,1,2,3,4]
```

### Step 1 — Reverse the entire array

```text
[1,2,3,4,5,6,7]

        ↓

[7,6,5,4,3,2,1]
```

### Step 2 — Reverse the first `k` elements

```text
[7,6,5,4,3,2,1]
 ─────
   k=3

        ↓

[5,6,7,4,3,2,1]
```

### Step 3 — Reverse the remaining elements

```text
[5,6,7,4,3,2,1]
       ─────────
       
        ↓

[5,6,7,1,2,3,4]
```

This gives the required rotated array.

---

## Important Optimization

Before performing the reversals:

```python
k %= n
```

This is necessary because rotating an array `n` times brings it back to its original state.

For example:

```text
n = 7
k = 10

10 % 7 = 3
```

So rotating by `10` positions is equivalent to rotating by `3` positions.

---

## Python Solution

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)

        if n <= 1:
            return

        k %= n

        def reverse(start: int, end: int) -> None:
            while start < end:
                nums[start], nums[end] = nums[end], nums[start]
                start += 1
                end -= 1

        reverse(0, n - 1)
        reverse(0, k - 1)
        reverse(k, n - 1)
```

---

## Example

### Input

```text
nums = [1,2,3,4,5,6,7]
k = 3
```

### Execution

```text
Original:
[1,2,3,4,5,6,7]

Reverse entire array:
[7,6,5,4,3,2,1]

Reverse first k elements:
[5,6,7,4,3,2,1]

Reverse remaining elements:
[5,6,7,1,2,3,4]
```

### Output

```text
[5,6,7,1,2,3,4]
```

---

## Why Does the Reversal Method Work?

Consider the array as two parts:

```text
A = [1,2,3,4]
B = [5,6,7]
```

The required rotation is:

```text
B + A
```

We start with:

```text
A + B
```

After reversing the entire array:

```text
reverse(B) + reverse(A)
```

Then reversing each part individually gives:

```text
B + A
```

Therefore, three reversals produce the required rotation.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Every element is involved in a constant number of reversals.

- **Space Complexity:** `O(1)`
  - The array is modified in-place.
  - No additional array is created.

---

## Key Takeaways

### Pattern

**Array Reversal + Two Pointers**

The `reverse()` function uses two pointers:

```text
start →        ← end
```

They move toward each other while swapping elements:

```python
nums[start], nums[end] = nums[end], nums[start]
```

### Important Concepts

- In-place array manipulation
- Two-pointer technique
- Array reversal
- Modulo operation
- `O(n)` time
- `O(1)` extra space

---

## Other Possible Approaches

This problem can also be solved using:

1. **Extra Array** — simple but requires `O(n)` space.
2. **Repeated Rotation** — simple conceptually but can take `O(n × k)` time.
3. **Cyclic Replacements** — achieves `O(n)` time and `O(1)` extra space.
4. **Array Reversal** — the approach used here.

**Chosen Approach:** Array Reversal  
**Time:** `O(n)`  
**Space:** `O(1)`
````
