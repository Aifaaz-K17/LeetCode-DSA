# 9. Palindrome Number

[**LeetCode Problem**](https://leetcode.com/problems/palindrome-number/)

**Difficulty:** Easy  
**Topics:** Math

## Problem Statement

Given an integer `x`, return `true` if `x` is a **palindrome**, and `false` otherwise.

A palindrome number reads the same from left to right and right to left.

---

## Approach

### Reverse Only Half of the Number

Instead of converting the integer into a string or reversing the entire number, we can reverse **only half of the digits**.

We use:

```python
reversed_half
```

to store the reversed digits from the right side of `x`.

### Step 1 — Handle Invalid Cases

Negative numbers cannot be palindromes:

```text
-121 → 121-
```

So:

```python
if x < 0:
    return False
```

A number ending in `0` cannot be a palindrome unless the number itself is `0`.

For example:

```text
10 → 01
```

Therefore:

```python
if x % 10 == 0 and x != 0:
    return False
```

Combined:

```python
if x < 0 or (x % 10 == 0 and x != 0):
    return False
```

---

## Step 2 — Reverse Half of the Number

We repeatedly take the last digit of `x`:

```python
x % 10
```

and add it to `reversed_half`:

```python
reversed_half = (reversed_half * 10) + (x % 10)
```

Then remove the last digit from `x`:

```python
x //= 10
```

We stop when:

```python
x <= reversed_half
```

At this point, we have processed approximately half of the digits.

---

## Step 3 — Handle Odd and Even Digit Counts

For an even number of digits:

```text
1221
```

After reversing half:

```text
x = 12
reversed_half = 12
```

So:

```python
x == reversed_half
```

For an odd number of digits:

```text
12321
```

After reversing half:

```text
x = 12
reversed_half = 123
```

The middle digit does not need to be compared, so we remove it using:

```python
reversed_half // 10
```

Therefore:

```python
x == reversed_half or x == reversed_half // 10
```

---

## Python Solution

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:

        if x < 0 or (x % 10 == 0 and x != 0):
            return False

        reversed_half = 0

        while x > reversed_half:
            reversed_half = (reversed_half * 10) + (x % 10)
            x //= 10

        return x == reversed_half or x == reversed_half // 10
```

---

## Example 1

### Input

```text
x = 121
```

Process:

```text
x = 121
reversed_half = 0

→ take 1
x = 12
reversed_half = 1

→ take 2
x = 1
reversed_half = 12

```

Now:

```text
x = 1
reversed_half = 12
```

Since this is an odd-length number, ignore the middle digit:

```text
reversed_half // 10 = 1
```

Therefore:

```text
x == reversed_half // 10
1 == 1
```

### Output

```text
true
```

---

## Example 2

### Input

```text
x = -121
```

Since:

```python
x < 0
```

the function immediately returns:

```text
false
```

---

## Example 3

### Input

```text
x = 10
```

The number ends with `0` and is not `0` itself:

```python
x % 10 == 0 and x != 0
```

Therefore:

```text
false
```

---

## Complexity Analysis

- **Time Complexity:** `O(log₁₀(n))`
  - We process approximately half of the digits of `x`.

- **Space Complexity:** `O(1)`
  - Only a few integer variables are used.

---

## Key Takeaways

- Negative numbers cannot be palindromes.
- Numbers ending in `0` cannot be palindromes unless the number is `0`.
- Only **half of the digits** need to be reversed.
- For even-length numbers, compare:
  ```python
  x == reversed_half
  ```
- For odd-length numbers, ignore the middle digit:
  ```python
  x == reversed_half // 10
  ```

### Pattern

**Mathematics → Digit Manipulation → Reverse Half**

This approach avoids converting the integer to a string and uses **O(1) extra space**.rr
