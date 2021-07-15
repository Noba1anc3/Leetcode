You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5

Explanation: 
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

## Idea

### 定义状态

搞清楚需要输出的结果后，就可以来想办法画一个表格，也就是定义dp数组的含义。根据背包问题的经验，可以将dp[i][j]定义为从数组nums中 0 - i 的元素进行加减可以得到 j 的方法数量。

### 状态转移方程

搞清楚状态以后，我们就可以根据状态去考虑如何根据子问题的转移从而得到整体的解。这道题的关键不是nums[i]的选与不选，而是nums[i]是加还是减，那么我们就可以将方程定义为：

dp[ i ][ j ] = dp[ i - 1 ][ j - nums[ i ] ] + dp[ i - 1 ][ j + nums[ i ] ]
可以理解为nums[i]这个元素我可以执行加，还可以执行减，那么dp[i][j]的结果值就是加/减之后对应位置的和。

### 表格

表格的每一行的长度t就可以表示为：t=(sum*2)+1，其中一个sum表示nums中执行全部执行加/减能达到的数，而加的1显然是中间的0.具体表格如下图所示：

![](https://pic.leetcode-cn.com/05f8151bbb0f1818723710b2455695f01c33d75a38653eeee181ab61217e8f16-image.png)

## Solution

### c++

```c++

```

### java

```java
public static int findTargetSumWays(int[] nums, int s) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        // 绝对值范围超过了sum的绝对值范围则无法得到
        if (Math.abs(s) > Math.abs(sum)) return 0;

        int len = nums.length;
        // - 0 +
        int t = sum * 2 + 1;
        int[][] dp = new int[len][t];
        // 初始化
        if (nums[0] == 0) {
            dp[0][sum] = 2;
        } else {
            dp[0][sum + nums[0]] = 1;
            dp[0][sum - nums[0]] = 1;
        }

        // 或者
        //  dp[0][sum + nums[0]] += 1;
        //  dp[0][sum - nums[0]] += 1;

        for (int i = 1; i < len; i++) {
            for (int j = 0; j < t; j++) {
                // 边界
                int l = (j - nums[i]) >= 0 ? j - nums[i] : 0;
                int r = (j + nums[i]) < t ? j + nums[i] : 0;
                dp[i][j] = dp[i - 1][l] + dp[i - 1][r];
            }
        }
        return dp[len - 1][sum + s];
    }
```

执行用时：13 ms, 在所有 Java 提交中击败了58.03%的用户  
内存消耗：38.9 MB, 在所有 Java 提交中击败了19.83%的用户

Attention:
- 动态规划时．确认最后要的东西，以及为了得到最终结果，怎样递推过去
- 确定表格轴的最大值
- 可以确认一些让程序提前返回的情况，避免白忙和一场
- 表格初始化注意极端条件




