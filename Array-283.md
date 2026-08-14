# 283. Move Zeroes

[**LeetCode Problem**](https://leetcode.com/problems/move-zeroes/)

**Difficulty:** Easy  
**Topics:** Array, Two Pointers

## Problem Statement

Given an integer array `nums`, move all `0`s to the end of the array while maintaining the **relative order of the non-zero elements**.

The operation must be performed **in-place**, meaning we cannot create a copy of the array.

---

## Approach

### Two Pointer Technique

We use two pointers:

- `fast` → scans through every element in the array.
- `slow` → keeps track of the position where the next non-zero element should be placed.

### Algorithm

1. Initialize `slow = 0`.
2. Traverse the array using `fast`.
3. Whenever `nums[fast]` is non-zero:
   - Place it at `nums[slow]`.
   - Increment `slow`.
4. After all non-zero elements have been moved to the front, fill the remaining positions with `0`.
5. The relative order of all non-zero elements remains unchanged.

---

## Python Solution

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:

        slow = 0

        for fast in range(0, len(nums)):
            if nums[fast] != 0:
                nums[slow] = nums[fast]
                slow += 1

        for i in range(slow, len(nums)):
            nums[i] = 0
```

---

## Example

### Input

```text
nums = [0,1,0,3,12]
```

### Step 1 — Move non-zero elements

The `fast` pointer scans the array and places every non-zero element at the `slow` position.

```text
Original:
[0, 1, 0, 3, 12]

After moving non-zero elements:
[1, 3, 12, 3, 12]
          ↑
         slow
```

The first `slow` positions now contain all non-zero elements in their original order.

### Step 2 — Fill remaining positions with zero

```text
[1, 3, 12, 0, 0]
```

### Output

```text
[1,3,12,0,0]
```

---

## Pointer Visualization

For:

```text
nums = [0, 1, 0, 3, 12]
```

The `fast` pointer scans every element:

```text
fast
 ↓
[0, 1, 0, 3, 12]
```

When a non-zero value is found, it is placed at the `slow` pointer:

```text
       fast
        ↓
[1, 1, 0, 3, 12]
 ↑
slow
```

Then:

```text
          fast
           ↓
[1, 3, 0, 3, 12]
    ↑
   slow
```

Finally:

```text
              fast
               ↓
[1, 3, 12, 3, 12]
       ↑
      slow
```

The remaining positions are replaced with zero:

```text
[1, 3, 12, 0, 0]
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - The array is traversed twice in the worst case.

- **Space Complexity:** `O(1)`
  - No additional array or data structure is created.

---

## Key Takeaway

This problem demonstrates the **Two Pointer / Slow-Fast Pointer pattern**.

The key idea is to separate the array into two logical regions:

```text
[ Non-zero elements | Remaining positions ]
         ↑
        slow
```

The `fast` pointer searches for non-zero elements, while `slow` determines where those elements should be placed.

**Pattern:** `Two Pointers → Slow & Fast`

**Important:** This solution preserves the relative order of all non-zero elements and modifies the array **in-place**.
