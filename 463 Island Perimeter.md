You are given ```row x col``` ```grid``` representing a map where ```grid[i][j] = 1``` represents land and ```grid[i][j] = 0``` represents water.

Grid cells are connected horizontally/vertically (not diagonally). The ```grid``` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example:**
![](https://assets.leetcode.com/uploads/2018/10/12/island.png)

```
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image above.

Input: grid = [[1]]
Output: 4

Input: grid = [[1,0]]
Output: 4

```

## Solution

### java

```java
public int islandPerimeter(int[][] grid) {
    for (int r = 0; r < grid.length; r++) {
        for (int c = 0; c < grid[0].length; c++) {
            if (grid[r][c] == 1) {
                // 题目限制只有一个岛屿，计算一个即可
                return dfs(grid, r, c);
            }
        }
    }
    return 0;
}

int dfs(int[][] grid, int r, int c) {
    // 函数因为「坐标 (r, c) 超出网格范围」返回，对应一条黄色的边
    if (!inArea(grid, r, c)) {
        return 1;
    }
    // 函数因为「当前格子是海洋格子」返回，对应一条蓝色的边
    if (grid[r][c] == 0) {
        return 1;
    }
    // 函数因为「当前格子是已遍历的陆地格子」返回，和周长没关系
    if (grid[r][c] != 1) {
        return 0;
    }
    grid[r][c] = 2;
    return dfs(grid, r - 1, c)
        + dfs(grid, r + 1, c)
        + dfs(grid, r, c - 1)
        + dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}

```

Time : 11ms  
Memory : 39.9MB  

### c++
```c++
class Solution {
private:
    int row, col;

public:
    bool inArea(int i, int j, std::vector<std::vector<int>>& grid) {
        if (0 <= i && i < row && 0 <= j && j < col)
            return true;
        return false;
    }

    int getPerimeter(int i, int j, std::vector<std::vector<int>>& grid) {
        if (!inArea(i, j, grid))
            return 1;
        if (grid[i][j] == 0)
            return 1;
        if (grid[i][j] == 2)
            return 0;
            
        grid[i][j] = 2;

        return getPerimeter(i, j-1, grid) + getPerimeter(i, j+1, grid) + getPerimeter(i-1, j, grid) + getPerimeter(i+1, j, grid);
    }

    int islandPerimeter(vector<vector<int>>& grid) {
        row = grid.size();
        col = grid[0].size();

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 1)
                    return getPerimeter(i, j, grid);
            }
        }

        return 0;
    }
};
```

执行用时：136 ms, 在所有 C++ 提交中击败了40.40%的用户

内存消耗：94 MB, 在所有 C++ 提交中击败了26.20%的用户


