Given a 2d grid map of ```'1'```s (land) and ```'0'```s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

## Solution - I DFS

### DFS 的基本结构
网格结构要比二叉树结构稍微复杂一些，它其实是一种简化版的图结构。要写好网格上的 DFS 遍历，我们首先要理解二叉树上的 DFS 遍历方法，再类比写出网格结构上的 DFS 遍历。我们写的二叉树 DFS 遍历一般是这样的：

```java
void traverse(TreeNode root) {
    // 判断 base case
    if (root == null) {
        return;
    }
    // 访问两个相邻结点：左子结点、右子结点
    traverse(root.left);
    traverse(root.right);
}
```

可以看到，二叉树的 DFS 有两个要素：**「访问相邻结点」**和**「判断 base case」**。

第一个要素是访问相邻结点。二叉树的相邻结点非常简单，只有左子结点和右子结点两个。二叉树本身就是一个递归定义的结构：一棵二叉树，它的左子树和右子树也是一棵二叉树。那么我们的 DFS 遍历只需要递归调用左子树和右子树即可。

第二个要素是 判断 base case。一般来说，二叉树遍历的 base case 是 root == null。这样一个条件判断其实有两个含义：一方面，这表示 root 指向的子树为空，不需要再往下遍历了。另一方面，在 root == null 的时候及时返回，可以让后面的 root.left 和 root.right 操作不会出现空指针异常。

对于网格上的 DFS，我们完全可以参考二叉树的 DFS，写出网格 DFS 的两个要素：

首先，网格结构中的格子有多少相邻结点？答案是上下左右四个。对于格子 (r, c) 来说（r 和 c 分别代表行坐标和列坐标），四个相邻的格子分别是 (r-1, c)、(r+1, c)、(r, c-1)、(r, c+1)。换句话说，网格结构是「四叉」的。

![](https://pic.leetcode-cn.com/63f5803e9452ccecf92fa64f54c887ed0e4e4c3434b9fb246bf2b410e4424555.jpg)


其次，网格 DFS 中的 base case 是什么？从二叉树的 base case 对应过来，应该是网格中不需要继续遍历、`grid[r][c]` 会出现数组下标越界异常的格子，也就是那些超出网格范围的格子。

![](https://pic.leetcode-cn.com/5a91ec351bcbe8e631e7e3e44e062794d6e53af95f6a5c778de369365b9d994e.jpg)

这样，我们得到了网格 DFS 遍历的框架代码：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    // 如果坐标 (r, c) 超出了网格范围，直接返回
    if (!inArea(grid, r, c)) {
        return;
    }
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

### 如何避免重复遍历

以岛屿问题为例，我们需要在所有值为 1 的陆地格子上做 DFS 遍历。每走过一个陆地格子，就把格子的值改为 2，这样当我们遇到 2 的时候，就知道这是遍历过的格子了。也就是说，每个格子可能取三个值：

- 0 —— 海洋格子
- 1 —— 陆地格子（未遍历过）
- 2 —— 陆地格子（已遍历过）

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    if (!inArea(grid, r, c)) {
        return;
    }
    // 如果这个格子不是岛屿，直接返回
    if (grid[r][c] != 1) {
        return;
    }
    grid[r][c] = 2; // 将格子标记为「已遍历过」
    
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

### 岛屿问题的解法

#### 例题 1：岛屿的最大面积

LeetCode 695. Max Area of Island （Medium）

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
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 1) {
                    int area = get_area(grid, i, j);
                    max_area = max(max_area, area);
                }
            }
        }
        return max_area;
    }
};
}
```

#### 例题 2：填海造陆问题

LeetCode 827. Making A Large Island （Hard）

这道题是岛屿最大面积问题的升级版。现在我们有填海造陆的能力，可以把一个海洋格子变成陆地格子，进而让两块岛屿连成一块。那么填海造陆之后，最大可能构造出多大的岛屿呢？

我们先计算出所有岛屿的面积，在所有的格子上标记出岛屿的面积。然后搜索哪个海洋格子相邻的两个岛屿面积最大。

例如下图中红色方框内的海洋格子，上边、左边都与岛屿相邻，我们可以计算出它变成陆地之后可以连接成的岛屿面积为 7 + 1 + 2 = 10.

![](https://pic.leetcode-cn.com/3a993272977112d82a37676380bf723f3f6b919a20da322e9a5c5afda07fa397.jpg)


然而，这种做法可能遇到一个问题。如下图中红色方框内的海洋格子，它的上边、左边都与岛屿相邻，这时候连接成的岛屿面积难道是 7 + 1 + 7？  
显然不是。这两个 ```7``` 来自同一个岛屿，所以填海造陆之后得到的岛屿面积应该只有 7 + 1 = 8。

![](https://pic.leetcode-cn.com/f519da076eb48fc993c7c71a0fa091b53bc6a1661163549eab60010606ee0e1c.jpg)

可以看到，要让算法正确，我们得能区分一个海洋格子相邻的两个 ```7``` 是不是来自同一个岛屿。那么，我们不能在方格中标记岛屿的面积，而应该标记岛屿的索引（下标），另外用一个数组记录每个岛屿的面积，如下图所示。这样我们就可以发现红色方框内的海洋格子，它的「两个」相邻的岛屿实际上是同一个。

![](https://pic.leetcode-cn.com/56ec808215d4ff3014476ef22297522b3731602266f9a069a82daf41001f904c.jpg)

可以看到，这道题实际上是对网格做了两遍 DFS：第一遍 DFS 遍历陆地格子，计算每个岛屿的面积并标记岛屿；第二遍 DFS 遍历海洋格子，观察每个海洋格子相邻的陆地格子。


#### 例题 3：岛屿的周长

LeetCode 463. Island Perimeter （Easy）

岛屿的周长是计算岛屿全部的「边缘」，而这些边缘就是我们在 DFS 遍历中，dfs 函数返回的位置。观察题目示例，我们可以将岛屿的周长中的边分为两类，如下图所示。黄色的边是与网格边界相邻的周长，而蓝色的边是与海洋格子相邻的周长。

![](https://pic.leetcode-cn.com/66d817362c1037ebe7705aacfbc6546e321c2b6a2e4fec96791f47604f546638.jpg)

当我们的 dfs 函数因为「坐标 (r, c) 超出网格范围」返回的时候，实际上就经过了一条黄色的边；而当函数因为「当前格子是海洋格子」返回的时候，实际上就经过了一条蓝色的边。这样，我们就把岛屿的周长跟 DFS 遍历联系起来了。


```python
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

#### 本题：岛屿的个数

##### python

```python
class Solution:
    def dfs(self, grid, r, c):
        if not self.inArea(grid, r, c):
            return

        if grid[r][c] != '1':
            return
            
        if grid[r][c] == '1':
            grid[r][c] = '2'

        self.dfs(grid, r - 1, c)
        self.dfs(grid, r + 1, c)
        self.dfs(grid, r, c - 1)
        self.dfs(grid, r, c + 1)
        
        return grid

    def inArea(self, grid, r, c):
        return 0 <= r and r < len(grid) and 0 <= c and c < len(grid[0])

    def numIslands(self, grid: List[List[str]]) -> int:
        num = 0
        for i in range(len(grid)):
            for j in range(len(grid[i])):
                if grid[i][j] == '1':
                    grid = self.dfs(grid, i, j)
                    num += 1
        
        return num
```

执行用时：84 ms, 在所有 Python3 提交中击败了44.62%的用户

内存消耗：18.7 MB, 在所有 Python3 提交中击败了9.17%的用户

Attention:
- 递归函数返回的情况

##### c++

```c++
class Solution {
private:
    int num = 0;

public:
    bool InArea(std::vector<std::vector<char>>& grid, int i, int j){
        return i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size();
    }

    void dfs(vector<vector<char>>& grid, int i, int j){       
        if (!InArea(grid, i, j) || grid[i][j] != '1')
            return;
       
        grid[i][j] = '2';

        dfs(grid, i, j-1);
        dfs(grid, i, j+1);
        dfs(grid, i-1, j);
        dfs(grid, i+1, j);
    }

    int numIslands(std::vector<std::vector<char>>& grid) {
        for (int i = 0; i < grid.size(); i++)
            for(int j = 0; j < grid[0].size(); j++)
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    num++;
                }

        return num;
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了80.11%的用户

内存消耗：9.2 MB, 在所有 C++ 提交中击败了96.99%的用户

## Solution - II Union-Find Set

c++

```c++
class Solution {
private:
    vector<int> parent, height;
    std::set<int> keys;
    int row, col;

public:
    bool InArea(int i, int j) {
        return i >= 0 && i < row && j >= 0 && j < col;
    }

    int Find_Set(int x) {
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    void Union(int i, int j, int m, int n){
        int key1 = i*col + j;
        int key2 = m*col + n;

        int ROOT1 = Find_Set(key1);
        int ROOT2 = Find_Set(key2);

        if (height[ROOT1] <= height[ROOT2]) {
            if (height[ROOT1] == height[ROOT2])
                height[ROOT2]++;
            parent[ROOT1] = ROOT2;
        }
        else
            parent[ROOT2] = ROOT1;
    }

    void Create_Set(int i, int j) {
        int key = i*col + j;
        if (parent[key] != 0) return;
        parent[key] = key; 
        height[key] = 1;
    }

    int numIslands(std::vector<std::vector<char>>& grid) {
        row = grid.size();
        col = grid[0].size();
        parent.resize(row * col);
        height.resize(row * col);
        
        for (int i = 0; i < row; i++)
            for(int j = 0; j < col; j++)
                if (grid[i][j] == '1') Create_Set(i, j);

        for (int i = 0; i < row; i++)
            for(int j = 0; j < col; j++)
                if (grid[i][j] == '1') {
                    if (InArea(i-1, j) && grid[i-1][j] == '1')
                        Union(i, j, i-1, j);
                    if (InArea(i+1, j) && grid[i+1][j] == '1') 
                        Union(i, j, i+1, j);                  
                    if (InArea(i, j-1) && grid[i][j-1] == '1') 
                        Union(i, j, i, j-1);
                    if (InArea(i, j+1) && grid[i][j+1] == '1') 
                        Union(i, j, i, j+1);
                }

        for (int i = 0; i < row; i++)
            for(int j = 0; j < col; j++)
                if (grid[i][j] == '1')
                    if (keys.find(Find_Set(i*col + j)) == keys.end())
                        keys.insert(Find_Set(i*col + j));

        return keys.size();
    }
};
```

Attention

```c++
vector<int>::iterator it = find(keys.begin(), keys.end(), key)
if (it == keys.end()) // not found
```

执行用时：16 ms, 在所有 C++ 提交中击败了84.32%的用户

内存消耗：9.9 MB, 在所有 C++ 提交中击败了22.24%的用户
