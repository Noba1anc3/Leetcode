Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).



**Example:**
```
Example 1:

Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].

Example 2:

Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

## Idea - I 基于记忆的DFS

将矩阵看成一个**有向图**，每个单元格对应图中的一个节点，如果相邻的两个单元格的值不相等，则在**相邻的两个单元格之间存在一条从较小值指向较大值的有向边**。问题转化成在**有向图中寻找最长路径**。

深度优先搜索是非常直观的方法。从一个单元格开始进行深度优先搜索，即可找到从该单元格开始的最长递增路径。对每个单元格分别进行深度优先搜索之后，即可得到矩阵中的最长递增路径的长度。

但是如果使用朴素深度优先搜索，时间复杂度是指数级，会超出时间限制，因此必须加以优化。

朴素深度优先搜索的时间复杂度过高的原因是进行了**大量的重复计算**，同一个单元格会被访问多次，每次访问都要重新计算。由于同一个单元格对应的最长递增路径的长度是固定不变的，因此可以使用记忆化的方法进行优化。用矩阵 memo 作为缓存矩阵，已经计算过的单元格的结果存储到缓存矩阵中。

使用记忆化深度优先搜索，当访问到一个单元格 (i,j) 时，如果 `memo[i][j] !=0`，说明该单元格的结果已经计算过，则直接从缓存中读取结果，如果 `memo[i][j] = 0`，说明该单元格的结果尚未被计算过，则进行搜索，并将计算得到的结果存入缓存中。

遍历完矩阵中的所有单元格之后，即可得到矩阵中的最长递增路径的长度。

## Solution - I

```c++
class Solution {
public:
    int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int rows, cols, ans = 0;

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0)
            return 0;

        rows = matrix.size();
        cols = matrix[0].size();
        vector<vector<int>> memo(rows, vector<int>(cols));

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                ans = max(ans, dfs(matrix, i, j, memo));

        return ans;
    }

    int dfs(vector<vector<int>>& matrix, int row, int col, vector<vector<int>>& memo) {
        if (memo[row][col] != 0)
            return memo[row][col];

        memo[row][col] = 1;
        for (int i = 0; i < 4; i++) {
            int newRow = row + dirs[i][0], newColumn = col + dirs[i][1];
            if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < cols && matrix[newRow][newColumn] > matrix[row][col]) {
                memo[row][col] = max(memo[row][col], dfs(matrix, newRow, newColumn, memo) + 1);
            }
        }
        
        return memo[row][col];
    }
};
```

执行用时：56 ms, 在所有 C++ 提交中击败了55.63%的用户

内存消耗：15.5 MB, 在所有 C++ 提交中击败了75.23%的用户

复杂度分析

- 时间复杂度：O(mn)，其中 m 和 n 分别是矩阵的行数和列数。深度优先搜索的时间复杂度是 O(V+E)，其中 V 是节点数，E 是边数。在矩阵中，O(V)=O(mn)，O(E)≈O(4mn)=O(mn)。

- 空间复杂度：O(mn)，其中 m 和 n 分别是矩阵的行数和列数。空间复杂度主要取决于缓存和递归调用深度，缓存的空间复杂度是 O(mn)，递归调用深度不会超过 mn。

## Idea - II 基于BFS的拓扑排序

从方法一可以看到，每个单元格对应的最长递增路径的结果只和相邻单元格的结果有关，那么是否可以使用动态规划求解？

根据方法一的分析，动态规划的状态定义和状态转移方程都很容易得到。方法一中使用的缓存矩阵 memo 即为状态值，状态转移方程如下：

`memo[i][j] = max{memo[x][y]} + 1`  其中(x, y)与(i, j)在矩阵中相邻，并且`matrix[x][y] > matrix[i][j]`

动态规划除了状态定义和状态转移方程，还需要考虑边界情况。这里的边界情况是什么呢？

如果一个单元格的值比它的所有相邻单元格的值都要大，那么这个单元格对应的最长递增路径是 1，这就是边界条件。

仍然使用方法一的思想，将矩阵看成一个有向图，计算每个单元格对应的出度，即有多少条边从该单元格出发。对于作为边界条件的单元格，该单元格的值比所有的相邻单元格的值都要大，因此**作为边界条件的单元格的出度都是 0**。

基于**出度**的概念，可以使用**拓扑排序**求解。从所有出度为 0 的单元格开始**广度优先搜索**，每一轮搜索都会遍历当前层的所有单元格，更新其余单元格的出度，并将出度变为 0 的单元格加入下一层搜索。当搜索结束时，**搜索的总层数即为矩阵中的最长递增路径的长度**。

## Solution - II

```c++
class Solution {
public:
    int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int rows, cols, level = 0;

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0)
            return 0;

        rows = matrix.size();
        cols = matrix[0].size();
        vector<vector<int>> outdegrees(rows, vector<int>(cols, 0));
        
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                for (int k = 0; k < 4; k++) {
                    int newRow = i + dirs[k][0], newCol = j + dirs[k][1];
                    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && matrix[newRow][newCol] > matrix[i][j])
                        outdegrees[i][j]++;
                }
        
        queue<pair<int, int>> Q;
        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                if (outdegrees[i][j] == 0)
                    Q.push(make_pair(i, j));
                    
        while (!Q.empty()) {
            level++;
            int n = Q.size(); // 由于遍历过程中会继续添加节点，因此需要暂存队列长度
            for (int i = 0; i < n; i++) {
                pair<int, int> vertex = Q.front(); Q.pop();
                int row = vertex.first, col = vertex.second;
                for (int k = 0; k < 4; k++) {
                    int newRow = row + dirs[k][0], newCol = col + dirs[k][1];
                    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && matrix[newRow][newCol] < matrix[row][col])
                        if (--outdegrees[newRow][newCol] == 0) Q.push(make_pair(newRow, newCol));
                }
            }
        }

        return level;
    }
};
```

执行用时：56 ms, 在所有 C++ 提交中击败了55.63%的用户

内存消耗：17.3 MB, 在所有 C++ 提交中击败了18.87%的用户
