In a 2D grid of 0s and 1s, we change at most one 0 to a 1.

After, what is the size of the largest island? (An island is a 4-directionally connected group of 1s).

**Example:**
```
Input: [[1, 0], [0, 1]]
Output: 3
Explanation: Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

Input: [[1, 1], [1, 0]]
Output: 4
Explanation: Change the 0 to 1 and make the island bigger, only one island with area = 4.

Input: [[1, 1], [1, 1]]
Output: 4
Explanation: Can't change any 0 to 1, only one island with area = 4.

```

## Solution

```python
class Solution(object):
    def largestIsland(self, grid):
        N = len(grid)

        def neighbors(r, c):
            for nr, nc in ((r-1, c), (r+1, c), (r, c-1), (r, c+1)):
                if 0 <= nr < N and 0 <= nc < N:
                    yield nr, nc

        def dfs(r, c, index):
            ans = 1
            grid[r][c] = index
            for nr, nc in neighbors(r, c):
                if grid[nr][nc] == 1:
                    ans += dfs(nr, nc, index)
            return ans

        area = {}
        index = 2
        for r in range(N):
            for c in range(N):
                if grid[r][c] == 1:
                    area[index] = dfs(r, c, index)
                    index += 1

        ans = max(area.values() or [0])
        for r in range(N):
            for c in range(N):
                if grid[r][c] == 0:
                    seen = {grid[nr][nc] for nr, nc in neighbors(r, c) if grid[nr][nc] > 1}
                    ans = max(ans, 1 + sum(area[i] for i in seen))
        return ans
```

Time : 156ms  
Memory : 16.2MB  

Attention:  
- 在递归时使用```1 +```来计算面积

### c++

```c++
class Solution {
public:
    bool inarea(vector<vector<int>>& grid, int i, int j){
        return i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size();
    }

    int dfs(vector<vector<int>>& grid, int i, int j, int index){
        if (!inarea(grid, i, j) || grid[i][j] != 1)
            return 0;
        
        grid[i][j] = index;

        return 1 + dfs(grid, i-1, j, index) 
                 + dfs(grid, i+1, j, index) 
                 + dfs(grid, i, j-1, index)
                 + dfs(grid, i, j+1, index);
    }

    int largestIsland(vector<vector<int>>& grid) {
        int ans = 0;
        int index = 2; // 降低空间复杂度
        vector<int> area = {0, 0};

        for (int i = 0; i < grid.size(); i++){
            for (int j = 0; j < grid[0].size(); j++){
                if (grid[i][j] == 1){
                    int size = dfs(grid, i, j, index++);
                    area.push_back(size);
                }
            }
        }

        for (const int& a : area)
            ans = max(ans, a); // 可能没有填海造陆的空间

        for (int i = 0; i < grid.size(); i++){
            for (int j = 0; j < grid[0].size(); j++){
                if (grid[i][j] == 0){
                    unordered_set<int> neighbors; // 避免重复
                    int neighbors_area = 0;

                    if (inarea(grid, i, j-1) && grid[i][j-1] > 1)
                        neighbors.insert(grid[i][j-1]);
                    if (inarea(grid, i, j+1) && grid[i][j+1] > 1)
                        neighbors.insert(grid[i][j+1]);
                    if (inarea(grid, i-1, j) && grid[i-1][j] > 1)
                        neighbors.insert(grid[i-1][j]);
                    if (inarea(grid, i+1, j) && grid[i+1][j] > 1)
                        neighbors.insert(grid[i+1][j]);

                    for (const int& neighbor : neighbors)
                        neighbors_area += area[neighbor];

                    ans = max(ans, 1 + neighbors_area);
                }
            }
        }

        return ans;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了87.64%的用户

内存消耗：12.9 MB, 在所有 C++ 提交中击败了49.25%的用户