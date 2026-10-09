# 9. Palindrome Number

**Difficulty:** Easy  
**Language:** Python  
**DSA Pattern:** Math / Digit Manipulation

---

## 📌 Problem Statement

Given an integer `x`, return `True` if `x` is a **palindrome**, and `False` otherwise.

A palindrome number reads the same from left to right and right to left.

### Example 1

```text
Input:  x = 121
Output: True
```

Explanation: `121` reads as `121` in both directions.

### Example 2

```text
Input:  x = -121
Output: False
```

Explanation: Reversing the digits gives `121-`, which is not equal to `-121`.

### Example 3

```text
Input:  x = 10
Output: False
```

Explanation: Reversing the digits gives `01`, which is not equal to `10`.

### Constraints

```text
-2^31 <= x <= 2^31 - 1
```

---

## 💡 Key Observation

A number is a palindrome if its digits are symmetrical.

For example:

```text
x = 1221

Original:       1  2 | 2  1
                     ↑
               Middle boundary
```

We could convert the number into a string and compare it with its reverse. However, there is a more space-efficient mathematical approach.

**Reverse only half of the number and compare the two halves.**

This avoids:
- Converting the integer into a string.
- Reversing the entire number.
- Storing additional arrays or strings.

---

## 🧠 Approach — Reverse Half of the Number

### Step 1: Handle Special Cases

```python
if x < 0 or (x % 10 == 0 and x != 0):
    return False
```

**Case 1: Negative numbers**

Negative numbers cannot be palindromes because the minus sign appears only on the left.

```text
x = -121
Reversed representation = 121-
```

Therefore, return `False`.

**Case 2: Numbers ending in zero**

A nonzero number ending in zero cannot be a palindrome because its reverse would begin with zero.

```text
x = 10
Reverse = 01
```

Therefore, return `False`.

The number `0` is an exception because it is a palindrome.

### Step 2: Initialize the Reversed Half

```python
reversed_half = 0
```

This variable stores the digits removed from the right side of `x`, in reverse order.

### Step 3: Reverse Half the Digits

```python
while x > reversed_half:
    reversed_half = (reversed_half * 10) + (x % 10)
    x //= 10
```

Two operations are essential:

- `x % 10` extracts the last digit.
- `x //= 10` removes the last digit.

The expression:

```python
reversed_half = reversed_half * 10 + x % 10
```

appends the extracted digit to the reversed half.

We continue until `reversed_half` has at least as many digits as the remaining `x`.

### Step 4: Compare the Halves

```python
return x == reversed_half or x == reversed_half // 10
```

There are two possibilities:

- **Even number of digits:** Both halves must be equal.
- **Odd number of digits:** Ignore the middle digit in `reversed_half` by dividing it by `10`.

For example, in `12321`, the middle digit is `3`. It does not affect whether the number is a palindrome.

---

## 🔎 Detailed Walkthrough

Consider:

```text
x = 1221
```

Initial state:

```text
x = 1221
reversed_half = 0
```

| Iteration | `x` | `x % 10` | `reversed_half` after update | `x` after division |
|---:|---:|---:|---:|---:|
| 1 | 1221 | 1 | 1 | 122 |
| 2 | 122 | 2 | 12 | 12 |

Now:

```text
x = 12
reversed_half = 12
```

The condition `x > reversed_half` becomes false, so the loop stops.

Final comparison:

```python
x == reversed_half
12 == 12
```

Result:

```text
True
```

Therefore, `1221` is a palindrome.

### Walkthrough for an Odd-Length Number

Consider:

```text
x = 12321
```

The algorithm processes the number as follows:

| Iteration | Remaining `x` | `reversed_half` |
|---:|---:|---:|
| 1 | 1232 | 1 |
| 2 | 123 | 12 |
| 3 | 12 | 123 |

The loop stops because `12 > 123` is false.

The middle digit is included at the end of `reversed_half`:

```text
Remaining half:  12
Reversed half:  123
```

Ignore the middle digit:

```python
reversed_half // 10
```

Result:

```text
123 // 10 = 12
```

Compare:

```python
x == reversed_half // 10
12 == 12
```

Therefore, the answer is `True`.

---

## 🧩 Algorithm

```text
1. If x is negative, return False.

2. If x is nonzero and ends in zero, return False.

3. Initialize reversed_half = 0.

4. While x > reversed_half:
   a. Extract the last digit using x % 10.
   b. Append it to reversed_half.
   c. Remove the last digit from x using x //= 10.

5. Return True if:
   a. x == reversed_half, or
   b. x == reversed_half // 10.

6. Otherwise, return False.
```

---

## 💻 Python Solution

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

## 🔍 Understanding the Important Operations

### 1. Modulo Operator `%`

```python
x % 10
```

Returns the last digit of a positive integer.

Example:

```python
1221 % 10  # 1
122 % 10   # 2
12 % 10    # 2
```

### 2. Integer Division `//`

```python
x // 10
```

Removes the last digit of a positive integer.

Example:

```python
1221 // 10  # 122
122 // 10   # 12
12 // 10    # 1
```

### 3. Appending a Digit

```python
reversed_half = reversed_half * 10 + digit
```

Suppose:

```text
reversed_half = 12
digit = 3
```

Then:

```text
12 * 10 + 3 = 123
```

The new digit is appended to the right.

### 4. Why `while x > reversed_half`?

We stop when the reversed half has reached or passed the remaining half in digit count.

This ensures we process approximately half the digits rather than reversing the entire number.

---

## ⏱️ Complexity Analysis

Let `d` be the number of digits in `x`.

### Time Complexity

**O(log₁₀ |x|)**

Each iteration removes one digit from `x`. Since only half the digits are processed, the number of iterations is proportional to the number of digits.

The number of digits in a positive integer is proportional to `log₁₀ x`.

### Space Complexity

**O(1)**

Only a constant number of integer variables are used. No strings, arrays, or additional data structures are created.

---

## 🐢 Alternative Approach — String Reversal

A simpler approach is to convert the number into a string and compare it with its reverse.

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False

        s = str(x)
        return s == s[::-1]
```

### Complexity

- **Time:** O(d)
- **Space:** O(d)

Here, `d` is the number of digits. String slicing creates a reversed string, requiring additional space.

This solution is easy to understand, but the reverse-half technique avoids extra string storage.

---

## 📊 Approach Comparison

| Approach | Time | Extra Space | Main Idea |
|---|---|---|---|
| String reversal | O(d) | O(d) | Convert and reverse |
| Full integer reversal | O(d) | O(1) | Reverse every digit |
| Reverse half | O(d) | O(1) | Reverse only half the digits |

**Recommended approach:** Reverse half of the number. It is efficient and demonstrates mathematical digit manipulation.

---

## ⚠️ Edge Cases

| Input | Output | Reason |
|---:|:---:|---|
| `0` | `True` | Zero is a palindrome |
| `7` | `True` | Single-digit numbers are palindromes |
| `121` | `True` | Odd-length palindrome |
| `1221` | `True` | Even-length palindrome |
| `123` | `False` | First and last digits differ |
| `-121` | `False` | Negative number |
| `10` | `False` | Nonzero number ends in zero |
| `100` | `False` | Reverse would begin with zero |

---

## 🎯 DSA Pattern

### Math / Digit Manipulation

This problem uses arithmetic operations to process an integer digit by digit.

Important concepts:

- Modulo (`%`) to extract digits.
- Integer division (`//`) to remove digits.
- Reversing digits mathematically.
- Comparing the two halves of a number.
- Handling even- and odd-length digit sequences.

### When to Recognize This Pattern

Use digit manipulation when a problem asks you to:
- Reverse an integer.
- Check whether a number is a palindrome.
- Find the sum or product of digits.
- Count digits.
- Extract digits without converting to a string.

---

## 🧠 Interview Explanation

> "I first reject negative numbers and nonzero numbers ending in zero because neither can be a palindrome. Then I reverse only half of the number using modulo to extract the last digit and integer division to remove it. Once the reversed half becomes greater than or equal to the remaining half, I compare them. For odd-length numbers, I remove the middle digit before comparing. The solution takes O(log₁₀ |x|) time and O(1) extra space."

---

## 🔑 Key Takeaways

- Negative integers cannot be palindromes.
- A nonzero integer ending in zero cannot be a palindrome.
- `% 10` extracts the last digit.
- `// 10` removes the last digit.
- Reverse only half the digits to avoid unnecessary work.
- For even-length numbers, compare both halves directly.
- For odd-length numbers, ignore the middle digit using `reversed_half // 10`.
- The optimal approach uses **O(1) extra space**.

---

## 📚 DSA Concepts Used

- Mathematics
- Digit Manipulation
- Modulo Operator
- Integer Division
- Number Reversal
- Time and Space Complexity

---

## 🏷️ Tags

```text
#LeetCode
#Python
#DSA
#Math
#DigitManipulation
#Palindrome
#Easy
#InterviewPreparation
#ProblemSolving
```

---

## ✅ Status

**Solved:** ✅  
**Optimal Approach:** ✅  
**Time Complexity:** O(log₁₀ |x|)  
**Space Complexity:** O(1)  
**Pattern:** Math / Digit Manipulation
