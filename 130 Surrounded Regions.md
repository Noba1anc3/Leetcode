Given a 2D board containing `'X'` and `'O'` (the letter O), capture all regions surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.



**Example:**

```
Example:

X X X X
X O O X
X X O X
X O X X

After running your function, the board should be:

X X X X
X X X X
X X X X
X O X X
```

**Explanation**:

Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

## Solution


```c++
class Solution {
private:
    int row, col, key;
    std::vector<int> parent;
    std::vector<int> height;
    std::set<int> keys;

public:
    void Create_Set(int x){
        if (parent[x] != 0)
            return;
        parent[x] = x;
        height[x] = 1;
    }

    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x);
        int ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;

        if (height[ROOT1] <= height[ROOT2]) {
            parent[ROOT2] = ROOT1;
            parent[y] = ROOT1;
            height[ROOT2] = height[ROOT1] + 1;
            height[y] = height[ROOT1] + 1;
        }
        else {
            parent[ROOT1] = ROOT2;
            parent[x] = ROOT2;
            height[ROOT1] = height[ROOT2] + 1;
            height[x] = height[ROOT2] + 1;
        }
    }

    void solve(std::vector<std::vector<char>>& board) {
        if (board.empty())
            return;

        row = board.size();
        col = board[0].size();

        parent.resize(row * col);
        height.resize(row * col);
        
        for (int i = 0; i < row; i++)
            for (int j = 0; j < col; j++)
                if (board[i][j] == 'O'){
                    Create_Set(i*col + j);
                    if (i > 0 && board[i - 1][j] == 'O'){
                        Create_Set((i-1)*col + j);
                        Union(i*col + j, (i-1)*col + j);
                    }
                    if (i < row - 1 && board[i + 1][j] == 'O'){
                        Create_Set((i+1)*col + j);
                        Union(i*col + j, (i+1)*col + j);
                    }
                    if (j > 0 && board[i][j - 1] == 'O'){
                        Create_Set(i*col + j - 1);
                        Union(i*col + j, i*col + j - 1);
                    }
                    if (j < col - 1 && board[i][j + 1] == 'O'){
                        Create_Set(i*col + j + 1);
                        Union(i*col + j, i*col + j + 1);
                    }
                }

        for (int i = 0; i < col; i++) {
            if (board[0][i] == 'O') 
            	keys.insert(Find_Set(i));
            if (board[row - 1][i] == 'O') 
            	keys.insert(Find_Set((row-1)*col + i));
        }

        for (int i = 0; i < row; i++) {
            if (board[i][0] == 'O') 
            	keys.insert(Find_Set(i*col));
            if (board[i][col - 1] == 'O')
            	keys.insert(Find_Set(i*col + col - 1));
        }

        for (int i = 1; i < row - 1; i++)
            for (int j = 1; j < col - 1; j++)
                if (board[i][j] == 'O') {
                    key = Find_Set(i*col + j);
                    if (keys.find(key) == keys.end())
                        board[i][j] = 'X';
                }
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了32.31%的用户

内存消耗：10.2 MB, 在所有 C++ 提交中击败了18.99%的用户

Attention:

- 路径压缩
- 添加key之前确保没有重复
