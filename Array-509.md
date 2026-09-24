# 509. Fibonacci Number

[**LeetCode Problem**](https://leetcode.com/problems/fibonacci-number/)

**Difficulty:** Easy  
**Topics:** Math, Recursion

## Problem Statement

The **Fibonacci numbers** form a sequence where each number is the sum of the two preceding numbers.

The sequence starts with:

```text
F(0) = 0
F(1) = 1
```

For `n > 1`:

```text
F(n) = F(n - 1) + F(n - 2)
```

Given an integer `n`, calculate and return `F(n)`.

---

## Approach

### Recursion

The Fibonacci definition itself is recursive:

```text
F(n) = F(n - 1) + F(n - 2)
```

So we can directly implement this mathematical definition using recursion.

### Base Case

For:

```text
n = 0 → F(0) = 0
n = 1 → F(1) = 1
```

Therefore:

```python
if n <= 1:
    return n
```

### Recursive Case

For `n > 1`:

```python
return self.fib(n - 1) + self.fib(n - 2)
```

The function keeps breaking the problem down until it reaches the base cases.

---

## Python Solution

```python
class Solution:
    def fib(self, n: int) -> int:

        if n <= 1:
            return n

        return self.fib(n - 1) + self.fib(n - 2)
```

---

## Example

### Input

```text
n = 4
```

The recursive calls calculate:

```text
F(4)
= F(3) + F(2)

= (F(2) + F(1)) + (F(1) + F(0))

= ((F(1) + F(0)) + 1) + (1 + 0)

= ((1 + 0) + 1) + 1

= 3
```

### Output

```text
3
```

---

## Recursion Tree

For:

```text
fib(4)
```

The recursion looks like:

```text
                    fib(4)
                   /      \
              fib(3)      fib(2)
             /    \       /    \
         fib(2)  fib(1) fib(1) fib(0)
         /   \
     fib(1) fib(0)
```

The base cases return:

```text
fib(0) = 0
fib(1) = 1
```

These values are then combined to calculate the final answer.

---

## Complexity Analysis

### Time Complexity

**O(2ⁿ)**

The recursive solution repeatedly calculates the same Fibonacci values.

For example:

```text
fib(5)
```

calculates `fib(2)`, `fib(1)`, etc. multiple times.

This results in exponential time complexity.

### Space Complexity

**O(n)**

The maximum recursion depth is `n`, so the call stack requires `O(n)` space.

---

## Key Takeaways

- Fibonacci is a classic example of **recursion**.
- Every recursive solution needs a **base case**.
- The recursive formula directly matches the mathematical definition.
- This implementation is simple but performs repeated calculations.

### Pattern

**Recursion → Base Case + Recursive Case**

```text
Base Case:
F(0) = 0
F(1) = 1

Recursive Case:
F(n) = F(n-1) + F(n-2)
```

---

## Optimization Note

Although the recursive solution is useful for understanding recursion, it is not the most efficient approach.

An iterative solution can calculate Fibonacci numbers in:

```text
Time:  O(n)
Space: O(1)
```

This avoids recalculating the same Fibonacci values repeatedly.
