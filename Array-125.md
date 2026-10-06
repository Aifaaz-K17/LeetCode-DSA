# 125. Valid Palindrome

**Difficulty:** Easy  
**Language:** Python  
**DSA Pattern:** Two Pointers / String Processing

---

## 📌 Problem Statement

A phrase is a **palindrome** if, after:

1. Converting uppercase letters to lowercase.
2. Removing all non-alphanumeric characters.

It reads the same forward and backward.

Alphanumeric characters include:

- Letters: `A-Z`, `a-z`
- Numbers: `0-9`

Given a string `s`, return `True` if it is a palindrome, otherwise return `False`.

---

## 📝 Examples

### Example 1

```text
Input:
s = "A man, a plan, a canal: Panama"

Output:
True
```

After removing non-alphanumeric characters and converting to lowercase:

```text
"amanaplanacanalpanama"
```

It reads the same from both directions.

---

### Example 2

```text
Input:
s = "race a car"

Output:
False
```

Processed string:

```text
"raceacar"
```

This is not a palindrome.

---

### Example 3

```text
Input:
s = " "

Output:
True
```

After removing non-alphanumeric characters:

```text
""
```

An empty string is considered a palindrome.

---

# 💡 Key Observation

A palindrome reads the same from both directions.

Instead of creating a new cleaned string, we can use **two pointers**:

```text
l → starts from the left
r → starts from the right
```

At every step:

1. Skip non-alphanumeric characters from the left.
2. Skip non-alphanumeric characters from the right.
3. Compare the two characters ignoring case.
4. If they differ → return `False`.
5. Move both pointers toward the center.

If all valid characters match, the string is a palindrome.

---

# 🧠 Approach — Two Pointers

Initialize:

```python
l, r = 0, len(s) - 1
```

Visually:

```text
A m a n , a   m a n a p l a n a
↑                         ↑
l                         r
```

We then move the pointers toward each other.

### Left Pointer

If `s[l]` is not alphanumeric:

```python
while l < r and not self.alphaNum(s[l]):
    l += 1
```

Move `l` forward until it points to a valid character.

---

### Right Pointer

Similarly:

```python
while r > l and not self.alphaNum(s[r]):
    r -= 1
```

Move `r` backward until it points to a valid character.

---

### Compare Characters

Once both pointers point to valid characters:

```python
if s[l].lower() != s[r].lower():
    return False
```

If they are different, the string cannot be a palindrome.

If they match:

```python
l, r = l + 1, r - 1
```

Move both pointers toward the center.

---

# 🔎 Detailed Walkthrough

Consider:

```text
s = "A man, a plan, a canal: Panama"
```

The relevant characters are:

```text
A m a n a p l a n a c a n a l P a n a m a
```

Ignoring case:

```text
a m a n a p l a n a c a n a l p a n a m a
```

Now compare from both ends:

```text
a ↔ a
m ↔ m
a ↔ a
n ↔ n
a ↔ a
p ↔ p
l ↔ l
...
```

Every pair matches.

Eventually:

```text
l >= r
```

Therefore:

```text
True
```

---

# 🧩 Algorithm

```text
1. Set l = 0 and r = len(s) - 1.

2. While l < r:

   a. Move l forward while s[l] is not alphanumeric.

   b. Move r backward while s[r] is not alphanumeric.

   c. Convert both characters to lowercase and compare them.

   d. If they are different:
          return False

   e. Move l forward and r backward.

3. Return True.
```

---

# 💻 Python Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            while l < r and not self.alphaNum(s[l]):
                l += 1

            while r > l and not self.alphaNum(s[r]):
                r -= 1

            if s[l].lower() != s[r].lower():
                return False

            l, r = l + 1, r - 1

        return True

    def alphaNum(self, c):
        return (
            ord('A') <= ord(c) <= ord('Z') or
            ord('a') <= ord(c) <= ord('z') or
            ord('0') <= ord(c) <= ord('9')
        )
```

---

# 🔍 Understanding `alphaNum()`

Your helper function determines whether a character is alphanumeric.

```python
def alphaNum(self, c):
    return (
        ord('A') <= ord(c) <= ord('Z') or
        ord('a') <= ord(c) <= ord('z') or
        ord('0') <= ord(c) <= ord('9')
    )
```

The `ord()` function converts a character into its Unicode/ASCII code.

For example:

```python
ord('A')  # 65
ord('Z')  # 90
```

Therefore:

```python
ord('A') <= ord(c) <= ord('Z')
```

checks whether `c` is an uppercase letter.

Similarly:

```python
ord('a') <= ord(c) <= ord('z')
```

checks lowercase letters.

And:

```python
ord('0') <= ord(c) <= ord('9')
```

checks digits.

So the complete condition means:

```text
Is c a letter OR a number?
```

---

# ⚡ Why Two Pointers?

A straightforward approach would be to create a cleaned string first:

```text
Original String
      ↓
Remove non-alphanumeric characters
      ↓
Convert to lowercase
      ↓
Compare with reversed string
```

For example:

```python
cleaned = ...
return cleaned == cleaned[::-1]
```

This works, but it requires creating another string.

Your approach instead processes the original string directly:

```text
Original String
      ↓
← l           r →
      ↓
Compare valid characters
      ↓
Move inward
```

Therefore, we avoid storing the cleaned string.

---

# 📊 Approach Comparison

| Approach | Time | Extra Space | Notes |
|---|---:|---:|---|
| Clean + Reverse | O(n) | O(n) | Simple |
| Two Pointers | O(n) | O(1) | Optimal |
| Two Pointers + `isalnum()` | O(n) | O(1) | Cleaner Python implementation |

Your implementation uses the **optimal space complexity**.

---

# ⏱️ Complexity Analysis

Let `n` be the length of the input string.

### Time Complexity

```text
O(n)
```

Each character is examined at most a constant number of times by the two pointers.

Although there are nested `while` loops, the pointers only move forward or backward. They never move backward after progressing.

Therefore, the total work remains linear.

---

### Space Complexity

```text
O(1)
```

Only a few variables are used:

```text
l
r
```

No cleaned string, array, or other data structure is created.

---

# ⚠️ Edge Cases

### 1. Empty String

```text
s = ""
```

Output:

```text
True
```

There are no characters that violate the palindrome condition.

---

### 2. Only Spaces / Symbols

```text
s = " "
```

After ignoring non-alphanumeric characters:

```text
""
```

Output:

```text
True
```

---

### 3. Single Character

```text
s = "a"
```

Output:

```text
True
```

---

### 4. Case Differences

```text
s = "Aa"
```

Comparison:

```text
a == a
```

Output:

```text
True
```

---

### 5. Numbers

```text
s = "1a2a1"
```

Comparison:

```text
1 ↔ 1
a ↔ a
2
```

Output:

```text
True
```

Numbers are valid alphanumeric characters and must be considered.

---

### 6. Special Characters Between Letters

```text
s = "a,b,a"
```

Comma is ignored:

```text
aba
```

Output:

```text
True
```

---

# 🐢 Alternative Python Solution

Python provides built-in methods that can make the implementation shorter:

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = ''.join(c.lower() for c in s if c.isalnum())
        return s == s[::-1]
```

This solution is valid and easy to understand.

However, it creates a new string, so its extra space complexity is:

```text
O(n)
```

Your two-pointer solution is preferable when the interviewer specifically asks for **O(1) extra space**.

---

# 🚀 Cleaner Two-Pointer Version

Python's `isalnum()` can replace the custom `alphaNum()` helper:

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            while l < r and not s[l].isalnum():
                l += 1

            while l < r and not s[r].isalnum():
                r -= 1

            if s[l].lower() != s[r].lower():
                return False

            l += 1
            r -= 1

        return True
```

This has the same:

```text
Time:  O(n)
Space: O(1)
```

and is more concise in Python.

Your original `alphaNum()` implementation is still useful for understanding **how character classification works internally**.

---

# 🎯 DSA Pattern

## Two Pointers

This problem is a classic **Two Pointer** problem.

The general pattern is:

```text
left → → → 
          ← ← ← right
```

Both pointers move toward each other.

This pattern is especially useful for:

- Palindrome problems
- Pair-sum problems
- Sorted arrays
- String comparisons
- Removing duplicates
- In-place array manipulation

### General Recognition Rule

When a problem involves comparing:

```text
First ↔ Last
Second ↔ Second Last
Third ↔ Third Last
```

think:

> **Two Pointers**

---

# 🧠 Interview Explanation

A concise interview explanation:

> "I use two pointers, one at the beginning and one at the end of the string. I skip any non-alphanumeric characters and compare the remaining characters case-insensitively. If any pair differs, I return false. Otherwise, I continue moving both pointers toward the center. If all pairs match, the string is a palindrome. This runs in O(n) time and uses O(1) extra space."

---

# 🔑 Key Takeaways

- A palindrome can be checked from both ends.
- **Two Pointers** avoids unnecessary string construction.
- Non-alphanumeric characters should be skipped.
- Comparisons should be case-insensitive.
- `ord()` can be used to manually identify letters and digits.
- Python's `isalnum()` provides a simpler alternative.
- The optimal solution runs in **O(n)** time.
- The two-pointer approach uses **O(1) extra space**.
- Nested pointer loops can still have **O(n)** total complexity because each pointer only moves in one direction.

---

# 📚 DSA Concepts Used

- Two Pointers
- String Traversal
- Character Classification
- ASCII / Unicode Character Codes
- Case Conversion
- In-Place Processing
- Time & Space Complexity

---

# 🏷️ Tags

```text
#LeetCode
#Python
#DSA
#TwoPointers
#Strings
#Palindrome
#Easy
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(n)  
**Space Complexity:** O(1)  
**Pattern:** Two Pointers
