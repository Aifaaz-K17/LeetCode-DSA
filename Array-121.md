# 121. Best Time to Buy and Sell Stock

[**LeetCode Problem**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

**Difficulty:** Easy  
**Topics:** Array, Dynamic Programming, Greedy

---

## Problem Statement

You are given an array `prices` where `prices[i]` represents the price of a stock on the `ith` day.

You need to choose:

- One day to **buy** the stock.
- A different day **after the buying day** to **sell** the stock.

Return the **maximum profit** possible.

If no profit can be made, return `0`.

### Example 1

```text
Input:  prices = [7,1,5,3,6,4]
Output: 5

Explanation:
Buy at price 1 and sell at price 6.

Profit = 6 - 1 = 5
