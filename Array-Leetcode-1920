# 1920. Build Array from Permutation

## Problem Statement

Given a **zero-based permutation** `nums` (0-indexed), build an array `ans` of the same length where:

```text
ans[i] = nums[nums[i]]
```

Return the resulting array.

A zero-based permutation means `nums` contains every integer from `0` to `n - 1` exactly once.

---

## Approach

1. Create an empty list `ans`.
2. Traverse the array `nums`.
3. For each index `i`, access the element at index `nums[i]`, i.e., `nums[nums[i]]`.
4. Append the value to `ans`.
5. Return the completed array.

---

## Python Solution

```python
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        ans = []
        for i in range(len(nums)):
            ans.append(nums[nums[i]])
        return ans
```

---

## Example

### Input

```text
nums = [0,2,1,5,3,4]
```

### Output

```text
[0,1,2,4,5,3]
```

### Explanation

```text
ans[0] = nums[nums[0]] = nums[0] = 0
ans[1] = nums[nums[1]] = nums[2] = 1
ans[2] = nums[nums[2]] = nums[1] = 2
ans[3] = nums[nums[3]] = nums[5] = 4
ans[4] = nums[nums[4]] = nums[3] = 5
ans[5] = nums[nums[5]] = nums[4] = 3

Result: [0,1,2,4,5,3]
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - We iterate through the array once.

- **Space Complexity:** `O(n)`
  - An additional array `ans` is used to store the result.

---

## Key Takeaways

- Uses direct indexing to build the required permutation.
- Simple one-pass solution.
- Efficient with linear time complexity.
