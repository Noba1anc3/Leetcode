Say you have an array `prices` for which the `ith` element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).




Examples:

```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.

Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

- `1 <= prices.length <= 3 * 10 ^ 4`
- `0 <= prices[i] <= 10 ^ 4`

## Idea

下面有图

![](http://r.photo.store.qq.com/psc?/fef49446-40e0-48c4-adcc-654c5015022c/TmEUgtj9EK6.7V8ajmQrEPHkpU34EX2.Cs1KpDwmc.NCqt.qeUzatthKxVzv7lLy.pFlFOw13yVFRss*1sA6.aGUhTh1e2y2KbaXoYNrQgI!/r)

## Solution

c++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        for (int i = 1; i < prices.size(); i++)
            profit = max(profit, profit + prices[i] - prices[i-1]);
        return profit;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了40.61%的用户

内存消耗：7.6 MB, 在所有 C++ 提交中击败了42.80%的用户

