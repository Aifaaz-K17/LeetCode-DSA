# 242. Valid Anagram

**Difficulty:** Easy  
**Language:** Python  
**DSA Pattern:** Hash Map / Frequency Counting

---

## 📌 Problem Statement

Given two strings `s` and `t`, return `True` if `t` is an **anagram** of `s`, and `False` otherwise.

An **anagram** is a word or string formed by rearranging all the characters of another string while using each character the same number of times.

### Example 1

```text
Input:
s = "anagram"
t = "nagaram"

Output:
True
```

### Example 2

```text
Input:
s = "rat"
t = "car"

Output:
False
```

### Constraints

- `1 <= s.length, t.length <= 5 * 10^4`
- `s` and `t` consist of lowercase English letters.

---

# 💡 Key Observation

Two strings are anagrams if and only if:

1. They have the **same length**.
2. Every character appears the **same number of times** in both strings.

For example:

```text
s = "anagram"
t = "nagaram"
```

Frequency of characters:

```text
a → 3
n → 1
g → 1
r → 1
m → 1
```

Both strings have exactly the same character frequencies, so they are anagrams.

Instead of sorting the strings, we can count the frequency of every character using a **Hash Map (`dict`)**.

---

# 🧠 Approach

We use two dictionaries:

```python
countS = {}
countT = {}
```

`countS` stores the frequency of characters in `s`.

`countT` stores the frequency of characters in `t`.

### Step 1 — Check Length

If the lengths are different, they cannot be anagrams.

```python
if len(s) != len(t):
    return False
```

For example:

```text
s = "abc"
t = "abcd"
```

They have different lengths, so immediately:

```text
False
```

This also prevents unnecessary processing.

---

### Step 2 — Count Character Frequencies

For every index `i`, count:

```python
s[i]
t[i]
```

The expression:

```python
countS[s[i]] = 1 + countS.get(s[i], 0)
```

means:

> Increase the frequency of `s[i]` by 1.

For example:

```text
s = "aab"
```

The dictionary becomes:

```python
{
    "a": 2,
    "b": 1
}
```

We do the same for `t`.

---

### Step 3 — Compare the Dictionaries

Finally:

```python
return countS == countT
```

If both dictionaries contain identical character frequencies, the strings are anagrams.

---

# 🔎 Detailed Walkthrough

Consider:

```text
s = "anagram"
t = "nagaram"
```

### Initial state

```python
countS = {}
countT = {}
```

### Process characters

| Index | `s[i]` | `t[i]` | `countS` | `countT` |
|---:|:---:|:---:|---|---|
| 0 | a | n | `{a:1}` | `{n:1}` |
| 1 | n | a | `{a:1,n:1}` | `{n:1,a:1}` |
| 2 | a | g | `{a:2,n:1}` | `{n:1,a:1,g:1}` |
| 3 | g | a | `{a:2,n:1,g:1}` | `{n:1,a:2,g:1}` |
| 4 | r | r | `{a:2,n:1,g:1,r:1}` | `{n:1,a:2,g:1,r:1}` |
| 5 | a | a | `{a:3,n:1,g:1,r:1}` | `{n:1,a:3,g:1,r:1}` |
| 6 | m | m | `{a:3,n:1,g:1,r:1,m:1}` | `{n:1,a:3,g:1,r:1,m:1}` |

Finally:

```python
countS == countT
```

is:

```text
True
```

Therefore:

```text
True
```

---

# 🧩 Algorithm

```text
1. If len(s) != len(t):
       return False

2. Create two empty hash maps:
       countS
       countT

3. Traverse both strings simultaneously.

4. Increment the frequency of each character in its
   respective hash map.

5. Compare countS and countT.

6. Return the result.
```

---

# 💻 Python Solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)

        return countS == countT
```

---

# ✨ How `dict.get()` Works

This line is important:

```python
countS[s[i]] = 1 + countS.get(s[i], 0)
```

Suppose:

```python
countS = {}
```

and:

```python
s[i] = "a"
```

Then:

```python
countS.get("a", 0)
```

returns:

```text
0
```

So:

```python
1 + 0
```

becomes:

```text
1
```

Dictionary:

```python
{"a": 1}
```

If `"a"` appears again:

```python
countS.get("a", 0)
```

returns:

```text
1
```

Therefore:

```text
1 + 1 = 2
```

Dictionary:

```python
{"a": 2}
```

This allows us to count frequencies without explicitly checking whether the key already exists.

---

# ⏱️ Complexity Analysis

Let `n` be the length of the strings.

### Time Complexity

```text
O(n)
```

We traverse both strings once.

Dictionary insertion and lookup are **O(1) average**.

Therefore:

```text
O(n)
```

### Space Complexity

```text
O(k)
```

where `k` is the number of distinct characters.

Because the problem only allows lowercase English letters:

```text
k <= 26
```

So for these specific constraints, the auxiliary space is effectively:

```text
O(1)
```

However, the general Hash Map approach is commonly described as:

```text
O(k)
```

space.

---

# 🐢 Brute Force / Alternative Approaches

## Approach 1 — Sorting

A simple solution is to sort both strings and compare them.

```python
return sorted(s) == sorted(t)
```

Example:

```text
s = "anagram"
t = "nagaram"

sorted(s) → ['a','a','a','g','m','n','r']
sorted(t) → ['a','a','a','g','m','n','r']
```

Therefore:

```text
True
```

### Complexity

```text
Time:  O(n log n)
Space: O(n)
```

depending on the sorting implementation.

---

## Approach 2 — Hash Map Frequency Counting

Our approach:

```python
countS = {}
countT = {}
```

Count every character and compare frequencies.

### Complexity

```text
Time:  O(n)
Space: O(k)
```

This is more efficient than sorting.

---

# 📊 Approach Comparison

| Approach | Time | Space | Notes |
|---|---:|---:|---|
| Sorting | O(n log n) | O(n) | Simple but slower |
| Two Hash Maps | O(n) | O(k) | Efficient and intuitive |
| One Hash Map | O(n) | O(k) | Can reduce auxiliary structures |

For interview purposes, **frequency counting with a Hash Map** is the standard optimal approach.

---

# 🚀 Possible Optimization: One Hash Map

Because the strings must contain exactly the same frequencies, we can use a single dictionary.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        count = {}

        for i in range(len(s)):
            count[s[i]] = count.get(s[i], 0) + 1
            count[t[i]] = count.get(t[i], 0) - 1

        return all(value == 0 for value in count.values())
```

### Idea

For every character:

```text
Character in s → +1
Character in t → -1
```

If the strings are anagrams, every character's final frequency must be:

```text
0
```

This reduces the number of dictionaries from two to one.

Your original solution, however, is **very clear and completely acceptable for interviews**.

---

# ⚠️ Edge Cases

### 1. Identical strings

```text
s = "abc"
t = "abc"
```

Output:

```text
True
```

---

### 2. Different lengths

```text
s = "abc"
t = "ab"
```

Output:

```text
False
```

Handled immediately by:

```python
if len(s) != len(t):
    return False
```

---

### 3. Same characters but different order

```text
s = "listen"
t = "silent"
```

Output:

```text
True
```

Order does not matter for anagrams.

---

### 4. Same characters but different frequencies

```text
s = "aab"
t = "abb"
```

Frequency comparison:

```text
s → a:2, b:1
t → a:1, b:2
```

Output:

```text
False
```

---

### 5. Single character

```text
s = "a"
t = "a"
```

Output:

```text
True
```

---

# 🎯 DSA Pattern

## Hash Map / Frequency Counting

This problem is a classic example of the **frequency counting pattern**.

Whenever a problem asks:

- Are two strings equivalent?
- Do two collections contain the same elements?
- How many times does each element occur?
- Find duplicates
- Find characters with specific frequencies
- Compare character distributions

Think:

```text
Hash Map → Frequency Counting
```

---

# 🧠 Interview Explanation

A concise interview explanation:

> "First, I check whether the strings have the same length. If they don't, they cannot be anagrams. Then I use hash maps to count the frequency of every character in both strings. Finally, I compare the two frequency maps. If they are equal, both strings contain exactly the same characters with the same frequencies, so they are anagrams. This takes O(n) average time and O(k) space."

---

# 🔑 Key Takeaways

- Anagrams depend on **character frequency**, not character order.
- A **Hash Map** provides efficient frequency counting.
- Checking lengths first is a useful early-exit optimization.
- Python's `dict.get()` makes frequency counting concise.
- Hash Map approach achieves **O(n)** average time.
- With only 26 lowercase English letters, the auxiliary space is effectively **O(1)**.
- This is one of the fundamental problems for learning **Hash Maps**.

---

# 📚 DSA Concepts Used

- Hash Map
- Dictionary
- Frequency Counting
- String Traversal
- Hashing
- Time & Space Complexity
- Early Exit Optimization

---

# 🏷️ Tags

```text
#LeetCode
#Python
#DSA
#HashMap
#FrequencyCounting
#Strings
#Easy
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(n)  
**Space Complexity:** O(k)  
**Pattern:** Hash Map / Frequency Counting
