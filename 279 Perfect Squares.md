Given a positive integer ```n```, find the least number of perfect square numbers (for example, ```1, 4, 9, 16, ...```) which sum to ```n```.

**Example:**
```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

## Solution
### 方法一：暴力枚举法

这个问题要求我们找出由完全平方数组合成给定数字的最小个数。我们将问题重新表述成：

给定一个完全平方数列表和正整数 ```n```，求出完全平方数组合成 ```n``` 的组合，要求组合中的解拥有完全平方数的最小个数。

注：可以重复使用列表中的完全平方数。

从上面对这个问题的叙述来看，它似乎是一个组合问题，对于这个问题，一个直观的解决方案是使用暴力枚举法，我们枚举所有可能的组合，并找到完全平方数的个数最小的一个。

我们可以用下面的公式来表述这个问题：

numSquares(n) = min(numSquares(n-k) + 1) ∀k ∈ square numbers

从上面的公式中，我们可以将其转换为递归解决方案。

```python
class Solution(object):
    def numSquares(self, n):
        square_nums = [i**2 for i in range(1, int(math.sqrt(n))+1)]

        def minNumSquares(k):
            """ recursive solution """
            # bottom cases: find a square number
            if k in square_nums:
                return 1
            min_num = float('inf')

            # Find the minimal value among all possible solutions
            for square in square_nums:
                if k < square:
                    break
                new_num = minNumSquares(k-square) + 1
                min_num = min(min_num, new_num)
            return min_num

        return minNumSquares(n)
```
### 方法二：动态规划

使用暴力枚举法会超出时间限制的原因很简单，因为我们重复的计算了中间解。我们以前的公式仍然是有效的。我们只需要一个更好的方法实现这个公式。

解决递归中堆栈溢出的问题的一个思路就是使用动态规划（DP）技术，该技术建立在重用中间解的结果来计算终解的思想之上。

#### 算法

- 几乎所有的动态规划解决方案，首先会创建一个一维或多维数组 DP 来保存中间子解的值，以及通常数组最后一个值代表最终解。
- 在主要步骤中，我们从数字 ```1``` 循环到 ```n```，计算每个数字 ```i``` 的解（即 ```numSquares(i)```）。每次迭代中，我们将 ```numSquares(i)``` 的结果保存在 ```dp[i]``` 中。
- 在循环结束时，我们返回数组中的最后一个元素作为解决方案的结果。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMubGVldGNvZGUtY24uY29tL0ZpZ3VyZXMvMjc5LzI3OV9kcC5wbmc?x-oss-process=image/format,png)

```python
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        square_nums = [i**2 for i in range(0, int(math.sqrt(n))+1)]
        
        dp = [float('inf')] * (n+1)
        # bottom case
        dp[0] = 0
        
        for i in range(1, n+1):
            for square in square_nums:
                if i < square:
                    break
                dp[i] = min(dp[i], dp[i-square] + 1)
        
        return dp[-1]
```

#### 复杂度分析
时间复杂度：\mathcal{O}(n\cdot\sqrt{n})O(n⋅ n)，在主步骤中，我们有一个嵌套循环，其中外部循环是 n 次迭代，而内部循环最多需要 \sqrt{n} 迭代。
空间复杂度：\mathcal{O}(n)O(n)，使用了一个一维数组 dp。

#### 提交
执行用时：5780 ms, 在所有 Python3 提交中击败了17.56%的用户
内存消耗：13.4 MB, 在所有 Python3 提交中击败了76.21%的用户

Attention:  
- 

