# 26. Remove Duplicates from Sorted Array

[**LeetCode Problem**](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

**Difficulty:** Easy  
**Topics:** Array, Two Pointers

## Problem Statement

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** so that each unique element appears only once.

Return the number of unique elements `k`.

The first `k` elements of `nums` must contain the unique elements in sorted order. Elements after index `k - 1` can be ignored.

---

## Approach

### Two Pointer Technique

Since the array is already sorted, duplicate values will always be next to each other.

We use two pointers:

- `slow` → points to the position of the last unique element.
- `fast` → scans through the array looking for new unique elements.

### Algorithm

1. If the array is empty, return `0`.
2. Initialize `slow = 0`.
3. Start `fast` from index `1`.
4. If `nums[fast] != nums[slow]`:
   - Move `slow` one position forward.
   - Copy `nums[fast]` to `nums[slow]`.
5. Continue until `fast` reaches the end.
6. Return `slow + 1`, which represents the number of unique elements.

---

## Python Solution

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0

        slow = 0

        for fast in range(1, len(nums)):
            if nums[fast] != nums[slow]:
                slow += 1
                nums[slow] = nums[fast]

        return slow + 1
```

---

## Example

### Input

```text
nums = [0,0,1,1,1,2,2,3,3,4]
```

### Execution

```text
slow = 0
fast = 1

0 == 0 → duplicate → skip

0 != 1
slow = 1
nums[1] = 1

0,1,1,1,1,2,2,3,3,4
    ↑
   slow
```

The process continues until all unique values are placed at the beginning of the array.

### Output

```text
k = 5
nums = [0,1,2,3,4,_,_,_,_,_]
```

Only the first `k` elements matter.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - The `fast` pointer traverses the array exactly once.

- **Space Complexity:** `O(1)`
  - The array is modified **in-place** and no additional data structure is used.

---

## Key Takeaway

The important pattern here is the **Two Pointer technique**.

Because the array is sorted, we don't need a `set` or another data structure to identify duplicates. We can compare the current element with the most recently stored unique element and overwrite duplicates in-place.

**Pattern:** `Two Pointers → Slow & Fast`
