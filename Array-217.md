# 217. Contains Duplicate

[**LeetCode Problem**](https://leetcode.com/problems/contains-duplicate/)

**Difficulty:** Easy  
**Topics:** Array, Hash Table

## Problem Statement

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array.

Return `false` if every element in the array is distinct.

---

## Approach

### Using a Set

Python's `set` stores only **unique elements**.

We can compare:

```python
len(set(nums))
```

with:

```python
len(nums)
```

If the array contains duplicates, the set will contain fewer elements than the original array.

Therefore:

```python
len(set(nums)) < len(nums)
```

means that at least one duplicate exists.

### Example

For:

```text
nums = [1,2,3,1]
```

The set becomes:

```text
{1,2,3}
```

Lengths:

```text
len(nums)     = 4
len(set(nums)) = 3
```

Since:

```text
3 < 4
```

the array contains a duplicate.

---

## Python Solution

```python
class Solution:
    def containsDuplicate(self, nums: list[int]) -> bool:
        return len(set(nums)) < len(nums)
```

---

## Example 1

### Input

```text
nums = [1,2,3,1]
```

Unique elements:

```text
{1,2,3}
```

Comparison:

```text
3 < 4
```

### Output

```text
true
```

---

## Example 2

### Input

```text
nums = [1,2,3,4]
```

Unique elements:

```text
{1,2,3,4}
```

Comparison:

```text
4 < 4
```

This is false.

### Output

```text
false
```

---

## Example 3

### Input

```text
nums = [1,1,1,3,3,4,3,2,4,2]
```

The set contains:

```text
{1,2,3,4}
```

Comparison:

```text
4 < 10
```

Therefore, duplicates exist.

### Output

```text
true
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Creating the set requires traversing the array.

- **Space Complexity:** `O(n)`
  - In the worst case, the set stores all `n` elements.

---

## Key Takeaways

The main idea is to use the property of a **set**:

```text
Set → Stores only unique values
```

Therefore:

```text
len(set(nums)) < len(nums)
        ↓
   Duplicate exists
```

### Pattern

**Array → Hash Set → Duplicate Detection**

This is a concise Python solution using the built-in `set` data structure.
