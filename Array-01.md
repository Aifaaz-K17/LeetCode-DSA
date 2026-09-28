# 1. Two Sum

[**LeetCode Problem**](https://leetcode.com/problems/two-sum/)

**Difficulty:** Easy  
**Topics:** Array, Hash Table

## Problem Statement

Given an array of integers `nums` and an integer `target`, return the **indices of the two numbers** such that they add up to `target`.

You may assume:

- Each input has **exactly one solution**.
- The same element cannot be used twice.
- The answer can be returned in any order.

---

## Approach

### Hash Map

We can solve this problem in **O(n)** time using a hash map.

Instead of checking every possible pair, we calculate the value that we need for each element.

For a current value:

```text
v
```

the required value is:

```text
diff = target - v
```

We then check whether this `diff` has already appeared in the hash map.

If it exists, we have found the two numbers.

### Example

```text
nums = [2,7,11,15]
target = 9
```

Start with:

```text
v = 2
diff = 9 - 2 = 7
```

`7` is not in the hash map, so store:

```text
{2: 0}
```

Next:

```text
v = 7
diff = 9 - 7 = 2
```

`2` already exists in the hash map at index `0`.

Therefore:

```text
[0, 1]
```

---

## Python Solution

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:

        hmap = {}

        for i, v in enumerate(nums):

            diff = target - v

            if diff in hmap:
                return [hmap[diff], i]

            hmap[v] = i
```

---

## Step-by-Step Execution

For:

```text
nums = [2,7,11,15]
target = 9
```

### Iteration 1

```text
i = 0
v = 2

diff = 9 - 2 = 7
```

`7` is not present.

Store:

```text
hmap = {
    2: 0
}
```

---

### Iteration 2

```text
i = 1
v = 7

diff = 9 - 7 = 2
```

`2` exists in the hash map:

```text
hmap[2] = 0
```

Therefore:

```text
return [0, 1]
```

---

## Why Store the Value After Checking?

The order is important:

```python
if diff in hmap:
    return [hmap[diff], i]

hmap[v] = i
```

We check for the complement **before** storing the current value.

This prevents using the same element twice.

For example:

```text
nums = [3,3]
target = 6
```

First `3`:

```text
diff = 3
```

It isn't in the map, so we store:

```text
{3: 0}
```

Second `3`:

```text
diff = 3
```

Now `3` is already in the map at index `0`, so:

```text
[0,1]
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - We traverse the array once.
  - Hash map lookup and insertion are `O(1)` on average.

- **Space Complexity:** `O(n)`
  - In the worst case, the hash map can contain `n` elements.

---

## Brute Force vs Hash Map

### Brute Force

Check every possible pair:

```text
O(n²) time
O(1) space
```

### Hash Map

Store previously seen values and look for the required complement:

```text
O(n) time
O(n) space
```

The hash map approach satisfies the follow-up requirement of achieving **less than O(n²)** time.

---

## Key Takeaways

The core idea is the **complement pattern**:

```text
required value = target - current value
```

For every element:

```text
current value → calculate complement → search hash map
```

### Pattern

**Array → Hash Map → Complement Lookup**

This is one of the most important patterns for solving **pair-sum problems** efficiently.
