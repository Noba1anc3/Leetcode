Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.



**Example:**
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
             

Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

## Idea

假如计划在第 i 天卖出股票，那么最大利润的差值一定是在[0, i-1] 之间选最低点买入；

所以遍历数组，依次求每个卖出时机的的最大差值，从中取最大值，而后更新当前最低值。

## Solution

### c++

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = prices[0], maxProfit = INT_MIN;
        for (const int& price : prices){
            maxProfit = max(maxProfit, price - minPrice);
            minPrice = min(minPrice, price);
        }
        return maxProfit;
    }
};
```

执行用时：88 ms, 在所有 C++ 提交中击败了99.81%的用户

内存消耗：91.1 MB, 在所有 C++ 提交中击败了75.20%的用户

### java

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0)
            return 0;
        
        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 0; i < prices.length; i++){
            int price = prices[i];
            maxProfit = Math.max(maxProfit, price - minPrice);
            minPrice = Math.min(minPrice, price);
        }

        return maxProfit;                
    }
}
```

执行用时：2 ms, 在所有 Java 提交中击败了62.24%的用户

内存消耗：38.4 MB, 在所有 Java 提交中击败了63.46%的用户

Attention
- list.length
- Math.max(a, b)
- Math.min(a, b)

**复杂度分析**

- 时间复杂度：O(n)

- 空间复杂度：O(1)
