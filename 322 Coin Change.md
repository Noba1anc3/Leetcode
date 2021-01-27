You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.




Examples:

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

Input: coins = [2], amount = 3
Output: -1

Input: coins = [1], amount = 0
Output: 0

Input: coins = [1], amount = 1
Output: 1

Input: coins = [1], amount = 2
Output: 2
```

## Idea

我们采用自下而上的方式进行思考。仍定义 `F(i)` 为组成金额 `i` 所需最少的硬币数量，假设在计算 `F(i)` 之前，我们已经计算出 `F(0)-F(i-1)` 的答案。 则 `F(i)` 对应的转移方程应为 `F(i)= min F(i -c_j) + 1`
其中 `c_j` 代表的是第 `j` 枚硬币的面值，即我们枚举最后一枚硬币面额是 `c_j`，那么需要从 `i-c_j` 这个金额的状态 `F(i-c_j)`转移过来，再算上枚举的这枚硬币数量 `1` 的贡献，由于要硬币数量最少，所以 `F(i)` 为前面能转移过来的状态的最小值加上枚举的硬币数量 `1` 。

## Solution

c++
```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        
        for (int i = 1; i <= amount; i++)
            for (const int& coin : coins)
                if (coin <= i)
                    dp[i] = min(dp[i], dp[i - coin] + 1);

        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
};
```

执行用时：72 ms, 在所有 C++ 提交中击败了90.01%的用户

内存消耗：13.6 MB, 在所有 C++ 提交中击败了82.66%的用户



Change Vector to List can speed up time and cost lower space

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int dp[amount + 1];
    
        dp[0] = 0;
        for (int i = 1; i <= amount; i++)
            dp[i] = amount + 1;
            
        for (int i = 1; i <= amount; i++)
            for (const int& coin : coins)
                if (coin <= i)
                    dp[i] = min(dp[i], dp[i - coin] + 1);

        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

执行用时：44 ms, 在所有 C++ 提交中击败了92.44%的用户

内存消耗：9.7 MB, 在所有 C++ 提交中击败了98.67%的用户

**复杂度分析**

- 时间复杂度：`O(Sn)`，其中 `S` 是金额，`n` 是面额数。我们一共需要计算 `O(S)` 个状态，`S` 为题目所给的总金额。对于每个状态，每次需要枚举 `n` 个面额来转移状态，所以一共需要 `O(Sn)` 的时间复杂度。
- 空间复杂度：`O(S)`。DP 数组需要开长度为总金额 `S` 的空间。