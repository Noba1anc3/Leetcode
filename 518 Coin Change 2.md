You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.



**Example:**

```
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.

Input: amount = 10, coins = [10] 
Output: 1
```

## Idea

题目可以转化为完全背包问题，即:
一个背包容量为amount，有n种物品coins，每种物品的重量为coins[i]，每种物品的数量无限，问最多存在几种能够恰好将背包装满的装法？

### 数组定义

`dp[i][j] = x`，表示对于前i个物品，当背包容量为j时，此时最多有x种装法
即只使用coins中的前i个硬币的面值，想凑出金额j，有x种凑法

### 选择与状态变化

1. 选择: 装进背包 或 不装进背包
2. 状态: 背包的容量 和 可选择的物品（有两个状态，所以dp用一个二维数组）
3. 状态转移

- 如果把第i个物品装入背包(即使用coins[i-1]这个面值的硬币)，`dp[i][j]=dp[i][j-coins[i-1]]` 表示当前背包的容量j减去当前i的重量coins[i-1];
- 如果不把第i个物品装入背包(即不使用coins[i-1]这个面值的硬币)，`dp[i][j]= dp[i-1][j]`，表示和之前状态的结果一样

```c++
// 当选择的第i个硬币的金额比想凑的金额大时，即只有选择不装
if (j < coins[i - 1]) {
    dp[i][j] = dp[i - 1][j];
} else {
    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
}
```

### 边缘情况

- `dp[0][..] = 0`
- `dp[..][0] = 1`

## Solution

### c++

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int size = coins.size();
        int dp[size+1][amount+1];
        
        for (int i = 0; i <= amount; i++)
            dp[0][i] = 0;
        for (int i = 0; i <= size; i++)
            dp[i][0] = 1;
        
        for (int i = 1; i <= size; i++){
            for (int j = 1; j <= amount; j++){
                int coin = coins[i-1];
                if (j < coin) 
                    dp[i][j] = dp[i - 1][j];
                else 
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coin];
            }
        }
        
        return dp[size][amount];
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了41.00%的用户

内存消耗：11.5 MB, 在所有 C++ 提交中击败了19.87%的用户

**复杂度分析**

- 时间复杂度：O(n*amount)
- 空间复杂度：O(n*amount)

## Idea - Low Space Complexity

### c++

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int dp[amount+1];
        memset(dp, 0, sizeof(dp));
        dp[0] = 1;
        
        for (const int& coin : coins)
            for (int j = coin; j <= amount; j++)
                dp[j] += dp[j - coin];
        
        return dp[amount];
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了99.66%的用户

内存消耗：6.6 MB, 在所有 C++ 提交中击败了99.25%的用户

Attention:
- 降低空间复杂度
- 先coin再amount是组合数，先amount再coin是排列数
