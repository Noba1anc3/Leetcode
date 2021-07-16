在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？



**Example:**
```
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

## Solution - I DP

```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if (grid.empty()) return 0;

        vector<vector<int>> dp(grid.size()+1, vector<int>(grid[0].size()+1, 0));
        for (int i = 1; i <= grid.size(); i++)
            for (int j = 1; j <= grid[0].size(); j++)
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]) + grid[i-1][j-1];

        return dp[grid.size()][grid[0].size()];

    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：9.3 MB, 在所有 C++ 提交中击败了5.36%的用户

### java

```java
class Solution {
    public int maxValue(int[][] grid) {
        int[][] dp = new int[grid.length+1][grid[0].length+1];

        for (int i = 0; i <= grid.length; i++){
            dp[i][0] = 0;
        }
        for (int i = 0; i <= grid[0].length; i++){
            dp[0][i] = 0;
        }

        for (int i = 1; i <= grid.length; i++){
            for (int j = 1; j <= grid[0].length; j++){
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]) + grid[i-1][j-1];
            }
        }

        return dp[grid.length][grid[0].length];
    }
}
```

执行用时：3 ms, 在所有 Java 提交中击败了80.27%的用户

内存消耗：40.9 MB, 在所有 Java 提交中击败了93.38%的用户

**Space Complexity** : O(mn)

## Solution - II Space Reduced DP

利用原始数组作为dp数组

```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if (grid.empty()) return 0;

        for (int i = 0; i < grid.size(); i++)
            for (int j = 0; j < grid[0].size(); j++){
                int up = i >= 1 ? grid[i-1][j] : 0;
                int left = j >= 1 ? grid[i][j-1] : 0;
                grid[i][j] = max(left, up) + grid[i][j];
            }

        return grid[grid.size()-1][grid[0].size()-1];

    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了86.13%的用户

内存消耗：9.3 MB, 在所有 C++ 提交中击败了58.51%的用户

**Space Complexity** : O(1)

Attention
- 借用原始数组降低空间复杂度
