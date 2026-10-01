# 128. Longest Consecutive Sequence

**Difficulty:** Medium  
**Platform:** LeetCode  
**Problem:** [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

---

## 📌 Problem Statement

Given an unsorted array of integers `nums`, return the length of the **longest consecutive elements sequence**.

The sequence does not need to appear consecutively in the original array.

The solution must run in:

```text
O(n)
```

time complexity.

---

## 🧪 Example 1

```text
Input:
nums = [100,4,200,1,3,2]

Output:
4
```

The longest consecutive sequence is:

```text
[1, 2, 3, 4]
```

Therefore:

```text
Length = 4
```

---

## 🧪 Example 2

```text
Input:
nums = [0,3,7,2,5,8,4,6,0,1]

Output:
9
```

The longest consecutive sequence is:

```text
[0,1,2,3,4,5,6,7,8]
```

Therefore:

```text
Length = 9
```

Notice that `0` appears twice, but duplicates do not affect the sequence.

---

## 🧪 Example 3

```text
Input:
nums = [1,0,1,2]

Output:
3
```

The longest consecutive sequence is:

```text
[0,1,2]
```

The duplicate `1` is ignored.

---

# 🧠 Key Observation

The array is **unsorted**, so checking consecutive elements directly will not work.

For example:

```text
[100, 4, 200, 1, 3, 2]
```

The values are not arranged in consecutive order.

A simple solution would be to sort the array first:

```text
[1,2,3,4,100,200]
```

but sorting takes:

```text
O(n log n)
```

The problem requires:

```text
O(n)
```

So we need another approach.

### The key idea:

Put every number into a **Hash Set**.

```python
numSet = set(nums)
```

Hash sets allow average:

```text
O(1)
```

lookup.

We can therefore quickly ask:

```text
Does num - 1 exist?
Does num + 1 exist?
Does num + 2 exist?
...
```

---

# 💡 Main Insight — Find Sequence Starts

This is the most important part of the solution.

For every number `num`, check:

```python
if (num - 1) not in numSet:
```

If `num - 1` does **not** exist, then `num` must be the **start of a consecutive sequence**.

For example:

```text
[1,2,3,4]
```

For `1`:

```text
0 does not exist
```

Therefore:

```text
1 = sequence start
```

For `2`:

```text
1 exists
```

So `2` is **not** a sequence start.

For `3`:

```text
2 exists
```

So `3` is not a sequence start.

For `4`:

```text
3 exists
```

So `4` is not a sequence start.

Therefore, we only begin counting from `1`.

---

# 🔎 Why Is This Important?

Consider:

```text
nums = [1,2,3,4,5]
```

Without the sequence-start check, we might do:

```text
Start at 1 → 1,2,3,4,5
Start at 2 → 2,3,4,5
Start at 3 → 3,4,5
Start at 4 → 4,5
Start at 5 → 5
```

The same sequence gets scanned multiple times.

But with:

```python
if (num - 1) not in numSet:
```

only `1` starts the scan:

```text
1 → 2 → 3 → 4 → 5
```

This is what allows the solution to achieve **O(n) average time**.

---

# 🧩 Approach

The algorithm has three main steps.

### Step 1 — Convert the array into a set

```python
numSet = set(nums)
```

This:

- Removes duplicates.
- Provides average O(1) lookup.

Example:

```text
nums = [1,2,3,4,4,5]

numSet = {1,2,3,4,5}
```

---

### Step 2 — Find sequence starts

For each number:

```python
for num in numSet:
```

check whether its predecessor exists:

```python
if (num - 1) not in numSet:
```

If not, `num` is the beginning of a sequence.

---

### Step 3 — Count the sequence

Once we find a sequence start:

```python
length = 1
```

Then check:

```python
num + 1
num + 2
num + 3
...
```

using:

```python
while (num + length) in numSet:
    length += 1
```

Finally:

```python
longest = max(longest, length)
```

stores the longest sequence found.

---

# 🔬 Detailed Walkthrough

Consider:

```text
nums = [100,4,200,1,3,2]
```

Convert it to a set:

```text
numSet = {1,2,3,4,100,200}
```

Initially:

```text
longest = 0
```

---

### Check `1`

Is:

```text
1 - 1 = 0
```

in the set?

```text
0 ∉ numSet
```

Therefore `1` is a sequence start.

Now count:

```text
1 → 2 → 3 → 4
```

So:

```text
length = 4
```

Update:

```text
longest = 4
```

---

### Check `2`

Is:

```text
2 - 1 = 1
```

in the set?

Yes.

Therefore:

```text
2 is not a sequence start
```

Skip it.

---

### Check `3`

Is:

```text
3 - 1 = 2
```

in the set?

Yes.

Skip it.

---

### Check `4`

Is:

```text
4 - 1 = 3
```

in the set?

Yes.

Skip it.

---

### Check `100`

Is:

```text
100 - 1 = 99
```

in the set?

No.

So `100` is a sequence start.

Check:

```text
101 ∉ numSet
```

Therefore:

```text
length = 1
```

`longest` remains `4`.

---

### Check `200`

Similarly:

```text
199 ∉ numSet
```

So:

```text
length = 1
```

Again:

```text
longest = 4
```

Final answer:

```text
4
```

---

# 📊 Visual Representation

```text
nums:

[100, 4, 200, 1, 3, 2]

        ↓

Hash Set:

{1, 2, 3, 4, 100, 200}


Sequence starts:

1 → 2 → 3 → 4
↑
Start because 0 doesn't exist

100
↑
Start because 99 doesn't exist

200
↑
Start because 199 doesn't exist
```

Longest sequence:

```text
1 → 2 → 3 → 4

Length = 4
```

---

# 💻 Python Solution

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:

        numSet = set(nums)
        longest = 0

        for num in numSet:

            # Start counting only if num is the beginning
            # of a consecutive sequence.
            if (num - 1) not in numSet:

                length = 1

                while (num + length) in numSet:
                    length += 1

                longest = max(length, longest)

        return longest
```

---

# 🧠 Line-by-Line Explanation

### Create the Hash Set

```python
numSet = set(nums)
```

Stores all unique numbers.

Example:

```text
[1,2,2,3,4]

↓

{1,2,3,4}
```

---

### Initialize the answer

```python
longest = 0
```

This stores the maximum sequence length found.

---

### Iterate through unique numbers

```python
for num in numSet:
```

We iterate over the set instead of the original array because duplicates don't matter.

---

### Find the beginning of a sequence

```python
if (num - 1) not in numSet:
```

This is the core condition.

If the previous number doesn't exist, `num` is the first number of a consecutive sequence.

---

### Start counting

```python
length = 1
```

The current number itself counts as one element.

---

### Continue while consecutive numbers exist

```python
while (num + length) in numSet:
    length += 1
```

For example, if:

```text
num = 1
```

the loop checks:

```text
2
3
4
5
...
```

until the next number is missing.

---

### Update the longest sequence

```python
longest = max(length, longest)
```

Keep the largest sequence discovered so far.

---

# ⏱️ Complexity Analysis

## Time Complexity

```text
O(n) average
```

There are two important parts.

### Building the set

```python
set(nums)
```

takes average:

```text
O(n)
```

### Finding sequences

Although there is a `for` loop and a nested `while` loop, the algorithm is still **O(n) average**.

Why?

The `while` loop only starts for numbers that are the **beginning of a sequence**.

Each consecutive number is effectively processed as part of its sequence.

For example:

```text
1 → 2 → 3 → 4 → 5
```

is scanned once from its starting point `1`.

The sequence is not repeatedly scanned from `2`, `3`, `4`, etc., because those numbers have a predecessor.

Therefore:

```text
Average Time = O(n)
```

> Python set operations have average O(1) lookup time. In the theoretical worst case, hash-table operations can degrade, but the standard complexity analysis for this solution is O(n) average.

---

## Space Complexity

```text
O(n)
```

We create:

```python
numSet = set(nums)
```

which can contain up to `n` unique elements.

Therefore:

```text
Space = O(n)
```

---

# 🚫 Brute Force Approach

One possible approach is to check every number and repeatedly search for its consecutive values in the array.

For example:

```text
For each number:
    Search for num + 1
    Search for num + 2
    Search for num + 3
    ...
```

Searching an unsorted array takes:

```text
O(n)
```

per lookup.

This can result in approximately:

```text
O(n²)
```

time complexity.

This is too slow for:

```text
n = 100,000
```

---

# 🚫 Sorting Approach

Another common solution is sorting.

```python
nums.sort()
```

After sorting:

```text
[100,4,200,1,3,2]

↓

[1,2,3,4,100,200]
```

We can then scan the sorted array.

However, sorting requires:

```text
O(n log n)
```

time.

The problem specifically asks for:

```text
O(n)
```

so the Hash Set approach is preferred.

---

# 🔄 Approach Comparison

| Approach | Time | Space | Meets O(n) Requirement? |
|---|---:|---:|---|
| Brute Force | O(n²) | O(1) | ❌ |
| Sorting | O(n log n) | Depends on implementation | ❌ |
| Hash Set + Sequence Start | **O(n) average** | **O(n)** | **✅** |

---

# 🧠 Pattern Learned

## Pattern: Hash Set + Sequence Start Detection

This problem teaches an important Hash Set pattern:

```text
Put everything into a set
        ↓
Check whether the previous value exists
        ↓
If not → start of sequence
        ↓
Count consecutive values
        ↓
Track maximum
```

The critical condition is:

```python
if num - 1 not in numSet:
```

This prevents unnecessary work.

---

# 🔑 Why `num - 1` Matters

Suppose:

```text
numSet = {1,2,3,4,5}
```

For `1`:

```text
0 does not exist
```

So:

```text
1 → sequence start
```

For `2`:

```text
1 exists
```

Not a start.

For `3`:

```text
2 exists
```

Not a start.

For `4`:

```text
3 exists
```

Not a start.

For `5`:

```text
4 exists
```

Not a start.

Only one sequence scan is required:

```text
1 → 2 → 3 → 4 → 5
```

---

# 🧪 Edge Cases

## 1. Empty Array

```text
nums = []
```

The set is empty.

No sequence exists.

```text
Output = 0
```

The implementation correctly handles this case.

---

## 2. Single Element

```text
nums = [10]
```

The longest sequence contains one element.

```text
Output = 1
```

---

## 3. Duplicate Values

```text
nums = [1,0,1,2]
```

Set becomes:

```text
{0,1,2}
```

Sequence:

```text
0 → 1 → 2
```

Output:

```text
3
```

---

## 4. No Consecutive Numbers

```text
nums = [10,20,30,40]
```

Every number forms a sequence of length `1`.

```text
Output = 1
```

---

## 5. Negative Numbers

The approach also works with negative values.

```text
nums = [-3,-2,-1,0,5]
```

Sequence:

```text
-3 → -2 → -1 → 0
```

Output:

```text
4
```

---

## 6. Very Large Values

The constraints allow:

```text
-10⁹ ≤ nums[i] ≤ 10⁹
```

The Hash Set approach does not depend on the numerical range of the values, only on the number of elements.

---

# 🎯 Interview Explanation

A concise interview explanation:

> "I use a Hash Set to get O(1) average-time lookups. For every unique number, I check whether `num - 1` exists. If it does, the number is part of a sequence that has already been started, so I skip it. If `num - 1` doesn't exist, then `num` is the beginning of a sequence, so I repeatedly check `num + 1`, `num + 2`, and so on. I track the maximum length. This gives O(n) average time and O(n) space."

---

# 📚 DSA Concepts

This problem reinforces:

- Hash Sets
- Hash Table Lookup
- Array Traversal
- Sequence Detection
- Duplicate Handling
- Greedy-style scanning
- Complexity Optimization

---

# 🔥 Most Important Takeaway

The most important line in this solution is:

```python
if (num - 1) not in numSet:
```

Don't just memorize it.

Understand the reason:

> **Only start counting a sequence from its smallest element.**

That single observation prevents repeated work and allows the algorithm to achieve **O(n) average time** without sorting.

---

## 🏷️ Tags

`Array` `Hash Set` `Hash Table` `Greedy` `Sequence` `LeetCode` `Python` `DSA` `Medium` `Interview Preparation`

---

## 🔗 Problem

[LeetCode #128 — Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

---

## ✅ Status

**Solved:** ✅  
**Difficulty:** Medium  
**Language:** Python  
**Time Complexity:** O(n) average  
**Space Complexity:** O(n)  
**Pattern:** Hash Set + Sequence Start Detection
