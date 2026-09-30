# 2149. Rearrange Array Elements by Sign

**Difficulty:** Medium
**Platform:** LeetCode
**Problem:** [Rearrange Array Elements by Sign](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

---

## 📌 Problem Statement

You are given a **0-indexed integer array** `nums` of even length.

The array contains an **equal number of positive and negative integers**.

Rearrange the elements so that:

1. Every consecutive pair has **opposite signs**.
2. The relative order of positive numbers is preserved.
3. The relative order of negative numbers is preserved.
4. The rearranged array starts with a **positive integer**.

Return the rearranged array.

---

## 🧪 Example 1

```text
Input:
nums = [3,1,-2,-5,2,-4]

Output:
[3,-2,1,-5,2,-4]
```

### Explanation

Positive numbers in the original array:

```text
[3, 1, 2]
```

Negative numbers:

```text
[-2, -5, -4]
```

We place them alternately:

```text
3   -2   1   -5   2   -4
↑    ↑   ↑    ↑   ↑    ↑
+    -   +    -   +    -
```

The order of positive numbers remains:

```text
3 → 1 → 2
```

The order of negative numbers remains:

```text
-2 → -5 → -4
```

---

## 🧪 Example 2

```text
Input:
nums = [-1,1]

Output:
[1,-1]
```

There is one positive and one negative number, so the only valid arrangement is:

```text
[1,-1]
```

---

# 🧠 Key Observation

The result must:

```text
Positive, Negative, Positive, Negative, ...
```

Since Python arrays are **0-indexed**:

```text
Index:   0   1   2   3   4   5
         +   -   +   -   +   -
```

Therefore:

* Positive numbers should go to **even indices**.
* Negative numbers should go to **odd indices**.

We can maintain two separate pointers:

```text
pos = 0
neg = 1
```

Whenever we encounter:

```text
positive → result[pos]
negative → result[neg]
```

After placing a number, move its pointer by `2`.

---

# 💡 Approach

We use an **auxiliary result array** and two position pointers.

### Step 1 — Create the result array

```python
result = [0] * n
```

This creates an array with the same size as `nums`.

---

### Step 2 — Initialize pointers

```python
pos = 0
neg = 1
```

Why?

Because the result must start with a positive number:

```text
Index 0 → Positive
Index 1 → Negative
Index 2 → Positive
Index 3 → Negative
...
```

---

### Step 3 — Traverse the input

For every element:

```python
if nums[i] >= 0:
```

place the positive number at the current positive position:

```python
result[pos] = nums[i]
pos += 2
```

For a negative number:

```python
result[neg] = nums[i]
neg += 2
```

---

# 🔎 Example Walkthrough

Consider:

```text
nums = [3,1,-2,-5,2,-4]
```

Initially:

```text
result = [0,0,0,0,0,0]

pos = 0
neg = 1
```

### Step 1

`3` is positive.

```text
result[0] = 3
```

Now:

```text
result = [3,0,0,0,0,0]
pos = 2
```

---

### Step 2

`1` is positive.

```text
result[2] = 1
```

Now:

```text
result = [3,0,1,0,0,0]
pos = 4
```

---

### Step 3

`-2` is negative.

```text
result[1] = -2
```

Now:

```text
result = [3,-2,1,0,0,0]
neg = 3
```

---

### Step 4

`-5` is negative.

```text
result[3] = -5
```

Now:

```text
result = [3,-2,1,-5,0,0]
neg = 5
```

---

### Step 5

`2` is positive.

```text
result[4] = 2
```

Now:

```text
result = [3,-2,1,-5,2,0]
pos = 6
```

---

### Step 6

`-4` is negative.

```text
result[5] = -4
```

Final result:

```text
[3,-2,1,-5,2,-4]
```

---

# 📊 Visual Representation

```text
Input:

[ 3,  1, -2, -5,  2, -4]
  +   +   -   -   +   -

        ↓ Rearrange

Index:   0    1    2    3    4    5
         +    -    +    -    +    -

Result:

[ 3,  -2,   1,  -5,   2,  -4]
```

The two pointers work independently:

```text
Positive pointer:
0 → 2 → 4

Negative pointer:
1 → 3 → 5
```

---

# 🧩 Algorithm

```text
1. Create a result array of size n.
2. Set pos = 0.
3. Set neg = 1.
4. Traverse nums from left to right.
5. If the current number is positive:
      place it at result[pos]
      move pos by 2
6. Otherwise:
      place it at result[neg]
      move neg by 2
7. Return result.
```

---

# 💻 Python Solution

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

# 🧠 Why Does This Preserve Order?

This is an important part of the problem.

Suppose the positive numbers are:

```text
[3, 1, 2]
```

We encounter them in exactly this order while traversing `nums`.

They are placed at:

```text
result[0] = 3
result[2] = 1
result[4] = 2
```

Therefore their relative order remains:

```text
3 → 1 → 2
```

Similarly, negative numbers:

```text
[-2, -5, -4]
```

are placed at:

```text
result[1] = -2
result[3] = -5
result[5] = -4
```

So their relative order remains:

```text
-2 → -5 → -4
```

This works because we traverse the original array **from left to right**.

---

# ⏱️ Complexity Analysis

## Time Complexity

```text
O(n)
```

We traverse the array exactly once.

For every element, we perform constant-time operations.

Therefore:

```text
Time = O(n)
```

---

## Space Complexity

```text
O(n)
```

We create a separate `result` array of size `n`.

Therefore:

```text
Space = O(n)
```

The problem explicitly states that an in-place modification is **not required**, so using an additional array is appropriate.

---

# 🚫 Why Not Use Sorting?

Sorting would destroy the original relative order of elements.

For example:

```text
[3, 1, -2, -5, 2, -4]
```

Sorting could produce:

```text
[-5, -4, -2, 1, 2, 3]
```

which violates the required ordering and sign arrangement.

The problem is not asking us to sort the values.

It asks us to **rearrange them while preserving relative order within each sign**.

---

# 🚫 Alternative Approach: Separate Positive and Negative Arrays

Another possible solution is:

```python
positive = []
negative = []
```

Then store positive and negative numbers separately and merge them alternately.

For example:

```text
positive = [3,1,2]
negative = [-2,-5,-4]

↓

[3,-2,1,-5,2,-4]
```

This works, but requires additional arrays for both groups.

The two-pointer result-array approach is cleaner because we can directly place each number into its final position.

---

# 🔄 Approach Comparison

| Approach                          |       Time | Extra Space | Preserves Order |
| --------------------------------- | ---------: | ----------: | --------------- |
| Sorting                           | O(n log n) |     Depends | ❌               |
| Separate positive/negative arrays |       O(n) |        O(n) | ✅               |
| Two position pointers             |   **O(n)** |    **O(n)** | **✅**           |

---

# 🧠 Pattern Learned

### Pattern: Two Pointers / Fixed Index Placement

This problem demonstrates a useful array technique:

> **Use separate pointers to place different categories of elements into predetermined positions.**

Here:

```text
Even indices → Positive
Odd indices  → Negative
```

The pointers advance by `2`:

```text
Positive:
0 → 2 → 4 → 6 → ...

Negative:
1 → 3 → 5 → 7 → ...
```

This technique can be useful whenever a problem specifies that certain types of elements must occupy specific positions.

---

# 🎯 Interview Explanation

A concise explanation for an interview:

> "Because the result must start with a positive number and alternate signs, positive numbers must occupy even indices and negative numbers must occupy odd indices. I maintain two pointers, `pos = 0` and `neg = 1`. While traversing the original array from left to right, I place each positive number at `pos` and each negative number at `neg`, then increment the corresponding pointer by 2. Since the input is processed from left to right, the relative order within positive and negative elements is preserved. The algorithm takes O(n) time and O(n) extra space."

---

# 🧪 Edge Cases

### 1. Minimum Input

```text
[-1, 1]
```

Output:

```text
[1, -1]
```

---

### 2. Already Alternating

```text
[1,-2,3,-4]
```

Output:

```text
[1,-2,3,-4]
```

The algorithm still works correctly.

---

### 3. Positives Initially Together

```text
[1,2,3,-1,-2,-3]
```

Output:

```text
[1,-1,2,-2,3,-3]
```

The relative order is preserved.

---

### 4. Negatives Initially Together

```text
[-1,-2,-3,1,2,3]
```

Output:

```text
[1,-1,2,-2,3,-3]
```

Again, the relative order within each sign is preserved.

---

# 🔑 Key Takeaways

* Use **even indices for positive numbers**.
* Use **odd indices for negative numbers**.
* Increment each pointer by `2`.
* Traverse the input from left to right to preserve relative order.
* No sorting is necessary.
* The solution runs in **O(n)** time.
* The solution uses **O(n)** extra space.

---

# 📚 DSA Concepts

This problem reinforces:

* Arrays
* Two Pointers
* Index Manipulation
* Stable Ordering
* In-place vs Auxiliary Array
* Linear-Time Algorithms

---

## 🏷️ Tags

`Array` `Two Pointers` `Index Manipulation` `Greedy` `LeetCode` `Python` `DSA` `Medium` `Interview Preparation`

---

## 🔗 Problem

[LeetCode #2149 — Rearrange Array Elements by Sign](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

---

## ✅ Status

**Solved:** ✅
**Difficulty:** Medium
**Language:** Python
**Time Complexity:** O(n)
**Space Complexity:** O(n)
**Pattern:** Two Pointers / Fixed Index Placement
