# 121. Best Time to Buy and Sell Stock

**Difficulty:** Easy
**Platform:** LeetCode
**Problem:** [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

---

## 📌 Problem Statement

You are given an array `prices` where:

```text
prices[i] = stock price on the ith day
```

You can choose:

* One day to **buy** one stock.
* A different day in the **future** to **sell** that stock.

Your goal is to maximize the profit.

Return the maximum profit possible.

If no profitable transaction is possible, return `0`.

### Example 1

```text
Input:
prices = [7,1,5,3,6,4]

Output:
5
```

Explanation:

```text
Buy at 1
Sell at 6

Profit = 6 - 1 = 5
```

### Example 2

```text
Input:
prices = [7,6,4,3,1]

Output:
0
```

Explanation:

The stock price continuously decreases, so no profitable transaction is possible.

---

# 🧠 Key Observation

The main challenge is that we must:

> **Buy before selling.**

Therefore, for every selling price, we need to know the **lowest price that appeared before it**.

Instead of checking every possible buy/sell combination, we can solve the problem in a single pass.

We maintain:

```text
buy → lowest stock price seen so far
max_profit → maximum profit found so far
```

For every price:

```text
profit = current_price - lowest_buy_price
```

Then update the maximum profit.

---

# 💡 Approach

We use a **Greedy + One-Pass** approach.

### Step 1 — Initialize the buying price

Start with the first price:

```python
buy = prices[0]
```

This represents the cheapest buying price seen so far.

---

### Step 2 — Iterate through the prices

For every price, treat it as a possible selling price.

```python
profit = sell - buy
```

If this profit is greater than the previous maximum:

```python
high_profit = max(high_profit, profit)
```

update the answer.

---

### Step 3 — Find a cheaper buying opportunity

If the current price is lower than our current buying price:

```python
if buy > sell:
    buy = sell
```

we update `buy`.

Why?

Because a lower buying price can potentially produce a larger profit in the future.

---

# 🔎 Example Walkthrough

Consider:

```text
prices = [7, 1, 5, 3, 6, 4]
```

Initially:

```text
buy = 7
high_profit = 0
```

### Price = 7

```text
profit = 7 - 7 = 0
high_profit = 0
```

No change.

---

### Price = 1

```text
profit = 1 - 7 = -6
```

Since `1 < 7`, update:

```text
buy = 1
```

---

### Price = 5

```text
profit = 5 - 1 = 4
high_profit = 4
```

---

### Price = 3

```text
profit = 3 - 1 = 2
```

Maximum remains:

```text
high_profit = 4
```

---

### Price = 6

```text
profit = 6 - 1 = 5
high_profit = 5
```

---

### Price = 4

```text
profit = 4 - 1 = 3
```

Maximum remains:

```text
high_profit = 5
```

Final answer:

```text
5
```

---

# 📊 Visual Representation

```text
Price:

7 ─────┐
       │
1 ─────┘ ← Best Buy
          \
           5
            \
             3
              \
               6 ← Best Sell
                \
                 4

Buy = 1
Sell = 6

Maximum Profit = 6 - 1 = 5
```

---

# 🧩 Algorithm

```text
1. Set buy = first price.
2. Set max_profit = 0.
3. Traverse the prices array.
4. Treat the current price as the selling price.
5. Calculate:
       profit = sell - buy
6. Update max_profit.
7. If the current price is lower than buy:
       update buy.
8. Return max_profit.
```

---

# 💻 Python Solution

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        profit = 0
        high_profit = 0
        buy = prices[0]

        for sell in prices:
            profit = sell - buy
            high_profit = max(high_profit, profit)

            if buy > sell:
                buy = sell

        return high_profit
```

---

# ✨ Cleaner Version

The same approach can be written more concisely using `min()`:

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        buy = prices[0]
        max_profit = 0

        for sell in prices:
            max_profit = max(max_profit, sell - buy)
            buy = min(buy, sell)

        return max_profit
```

Both implementations use the same algorithm.

---

# ⏱️ Complexity Analysis

### Time Complexity

```text
O(n)
```

We traverse the array only once.

If there are `n` prices, we perform approximately `n` iterations.

---

### Space Complexity

```text
O(1)
```

We only use a few variables:

```text
buy
profit
high_profit
```

No additional data structure is required.

---

# 🧪 Edge Cases

### 1. Continuously decreasing prices

```text
[7,6,5,4,3]
```

No profitable transaction exists.

```text
Output = 0
```

---

### 2. Only one price

```text
[5]
```

There is no future day to sell.

```text
Output = 0
```

---

### 3. Increasing prices

```text
[1,2,3,4,5]
```

Best transaction:

```text
Buy = 1
Sell = 5

Profit = 4
```

---

### 4. Same prices

```text
[5,5,5,5]
```

No profit can be made.

```text
Output = 0
```

---

### 5. Large input

The constraints allow up to:

```text
n = 10⁵
```

An `O(n)` solution efficiently handles this input size.

---

# 🧠 Pattern Learned

### Pattern: Minimum So Far + Maximum Difference

This problem teaches an important array pattern:

```text
Track the minimum value so far
             ↓
Calculate current difference
             ↓
Track the maximum difference
```

General form:

```python
minimum = ...
answer = ...

for value in array:
    answer = max(answer, value - minimum)
    minimum = min(minimum, value)
```

This pattern can be useful for problems involving:

* Maximum difference
* Buy/sell optimization
* Best time to perform an action
* Finding maximum gain between two positions

---

# 🚫 Brute Force Approach

A straightforward solution would check every possible pair:

```text
Buy on day i
Sell on day j
where j > i
```

For example:

```python
for i in range(len(prices)):
    for j in range(i + 1, len(prices)):
        profit = prices[j] - prices[i]
```

### Complexity

```text
Time: O(n²)
Space: O(1)
```

For `n = 100,000`, this becomes inefficient.

The one-pass approach improves:

```text
O(n²) → O(n)
```

---

# 🔄 Brute Force vs Optimized

| Approach        |     Time |    Space |
| --------------- | -------: | -------: |
| Brute Force     |    O(n²) |     O(1) |
| One-Pass Greedy | **O(n)** | **O(1)** |

The optimized solution avoids checking every possible buy/sell pair.

---

# 🎯 Interview Explanation

A concise way to explain the solution in an interview:

> "For every day, I treat the current price as the selling price and maintain the minimum price seen before that day as the buying price. I calculate the profit using `sell - buy` and keep track of the maximum profit. Whenever I encounter a lower price, I update the buying price. This allows me to solve the problem in one pass with O(n) time and O(1) space."

---

# 🔑 Takeaways

* Maintain the **minimum price seen so far**.
* Treat every current price as a potential selling price.
* Calculate the profit using:

```text
current price - minimum price
```

* Keep track of the maximum profit.
* A greedy one-pass solution reduces the brute-force complexity from **O(n²) to O(n)**.
* The key pattern is **minimum-so-far + maximum difference**.

---

## 🏷️ Tags

`Array` `Greedy` `One Pass` `Optimization` `Maximum Profit` `LeetCode` `Python` `DSA` `Interview Preparation`

---

## 🔗 Problem

[LeetCode #121 — Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

---

### Status

✅ Solved
**Difficulty:** Easy
**Language:** Python
**Time Complexity:** O(n)
**Space Complexity:** O(1)
