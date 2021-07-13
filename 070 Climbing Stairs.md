You are climbing a staircase. It takes `n` steps to reach the top. 

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## Idea

我们用 f(x) 表示爬到第 x 级台阶的方案数，考虑最后一步可能跨了一级台阶，也可能跨了两级台阶，所以我们可以列出如下式子：

f(x) = f(x - 1) + f(x - 2)

它意味着爬到第 x 级台阶的方案数是爬到第 x - 1 级台阶的方案数和爬到第 x - 2 级台阶的方案数的和。很好理解，因为每次只能爬 1 级或 2 级，所以 f(x) 只能从 f(x - 1) 和 f(x - 2) 转移过来，而这里要统计方案总数，我们就需要对这两项的贡献求和。

以上是动态规划的转移方程，下面我们来讨论边界条件。我们是从第 0 级开始爬的，所以从第 0 级爬到第 0 级我们可以看作只有一种方案，即 f(0) = 1. 从第 0 级到第 1 级也只有一种方案，即爬一级，f(1) = 1. 这两个作为边界条件就可以继续向后推导出第 n 级的正确结果。我们不妨写几项来验证一下，根据转移方程得到 f(2) = 2，f(3) = 3，f(4) = 5......我们把这些情况都枚举出来，发现计算的结果是正确的。

我们不难通过转移方程和边界条件给出一个时间复杂度和空间复杂度都是 O(n) 的实现，但是由于这里的 f(x) 只和 f(x - 1) 与 f(x - 2) 有关，所以我们可以用「滚动数组思想」把空间复杂度优化成 O(1)。

## Solution - I DP Based on List

c++

```c++
class Solution {
public:
    int climbStairs(int n) {
        int dp[n+1];
        dp[0] = dp[1] = 1;
        for (int i = 2; i <= n; i++)
            dp[i] = dp[i-1] + dp[i-2];
        
        return dp[n];
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.9 MB, 在所有 C++ 提交中击败了61.39%的用户

## Solution - II DP Based on Integer

### c++

```c++
class Solution {
public:
    int numWays(int n) {
        int a = 1, b = 1, tmp;
        for (int i = 2; i <= n; i++){
            tmp = (a + b) % int(1e9+7);
            a = b;
            b = tmp;
        }
        return b;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.1 MB, 在所有 C++ 提交中击败了50.23%的用户

复杂度分析
- 时间复杂度：循环执行 n 次，每次花费常数的时间代价，故渐进时间复杂度为 O(n)
- 空间复杂度：这里只用了常数个变量作为辅助空间，故渐进空间复杂度为 O(1)

Attention:
- 降低空间复杂度

## Solution - III Matrix

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrEJsCHKY1iEhfQKAr9fXM1PmVCetFcNCstJLcGHTD07rPaneWwT4enQlMcslUqLHcp.pGFS*Txpdm2exiuJR8EjQ!/r)

![](http://r.photo.store.qq.com/psc?/V50VqFfH2A6OlZ2gWBDL0uxzNK4WmFgm/TmEUgtj9EK6.7V8ajmQrELQQF0HdGbsd0E8oAa9Q*iU4UdUrGCdFBQKZ1ArI9S6Q90LqJwMbWMWHyPGyuQ4P3oyn9R6ToJ6kM5tUTHJcne4!/r)

## Extended Idea

如果我们把问题泛化，不再是固定的1,2，而是任意给定台阶数，例如1,2,5呢？

我们只需要修改我们的DP方程DP[i] = DP[i-1] + DP[i-2] + DP[i-5], 也就是DP[i] = DP[i] + DP[i-j] ,j =1,2,5

在原来的基础上，我们的代码可以做这样子修改

```c++
class Solution {
public:
    int climbStairs(int n) {
        int steps[2] = {1,2}, DP[n+1];
        memset(DP, 0, sizeof(DP));
        DP[0] = 1;
		int stepLen = sizeof(steps) / sizeof(int);
        
        for (int i = 1; i <= n; i++){
            for (int j = 0; j < stepLen; j++){
                int step = steps[j];
                if ( i < step ) continue; // 台阶少于跨越的步数
                DP[i] += DP[i-step];
            }
        }

        return DP[n];
    }
};
```

