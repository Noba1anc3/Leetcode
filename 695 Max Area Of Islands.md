Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example:**
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]

Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.


[[0,0,0,0,0,0,0,0]]

Given the above grid, return 0.
Note: The length of each dimension in the given grid does not exceed 50.

```

## Solution

### c++

```c++
class Solution {
private:
    int row, col;
    int max_area = 0;

public:
    bool inArea(std::vector<std::vector<int>>& grid, int i, int j) {
        if (0 <= i && i < row && 0 <= j && j < col) return true;
        return false;
    }

    int get_area(std::vector<std::vector<int>>& grid, int i, int j) {
        if (!inArea(grid, i, j)) return 0;
        if (grid[i][j] != 1) return 0;
        grid[i][j] = 2;
        return 1 + get_area(grid, i, j-1) + get_area(grid, i, j+1) + get_area(grid, i-1, j) + get_area(grid, i+1, j);
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
        row = grid.size(), col = grid[0].size();
        for (int i = 0; i < row; i++)
            for (int j = 0; j < col; j++)
                if (grid[i][j] == 1) {
                    int area = get_area(grid, i, j);
                    max_area = max(max_area, area);
                }

        return max_area;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了97.49%的用户

内存消耗：22.3 MB, 在所有 C++ 提交中击败了83.75%的用户

### java

```java
public int maxAreaOfIsland(int[][] grid) {
    int max_area = 0;
    for (int r = 0; r < grid.length; r++) {
        for (int c = 0; c < grid[0].length; c++) {
            if (grid[r][c] == 1) {
                int a = area(grid, r, c);
                max_area = Math.max(max_area, a);
            }
        }
    }
    return max_area;
}

int area(int[][] grid, int r, int c) {
    if (!inArea(grid, r, c)) {
        return 0;
    }
    if (grid[r][c] != 1) {
        return 0;
    }
    grid[r][c] = 2;
    
    return 1 
        + area(grid, r - 1, c)
        + area(grid, r + 1, c)
        + area(grid, r, c - 1)
        + area(grid, r, c + 1);
}

boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}

```

Time : 2ms  
Memory : 40.4MB
