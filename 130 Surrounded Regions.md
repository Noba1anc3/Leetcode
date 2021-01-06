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
    int row, col;
    vector<int> parent;
    vector<int> keys;

public:
    void Create_Set(int x){
        if (parent[x] != 0)
            return;
        parent[x] = x;
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

        parent[ROOT2] = ROOT1; // 反过来慢很多
        parent[y] = ROOT1;
        parent[x] = ROOT1;
    }

    void solve(vector<vector<char>>& board) {
        if (board.empty())
            return;

        row = board.size();
        col = board[0].size();

        parent.resize(row * col);
        
        for (int i = 0; i < row; i++){
            for (int j = 0; j < col; j++){
                if (board[i][j] == 'O'){
                    Create_Set(i*col + j);
                    if (i > 0 && board[i - 1][j] == 'O'){
                        Create_Set(i*col - col + j);
                        Union(i*col + j, i*col - col + j);
                    }
                    if (i < row - 1 && board[i + 1][j] == 'O'){
                        Create_Set(i*col + col + j);
                        Union(i*col + j, i*col + col + j);
                    }
                    if (j > 0 && board[i][j - 1] == 'O'){
                        Create_Set(i*col - 1 + j);
                        Union(i*col + j, i*col - 1 + j);
                    }
                    if (j < col - 1 && board[i][j + 1] == 'O'){
                        Create_Set(i*col + 1 + j);
                        Union(i*col + j, i*col + 1 + j);
                    }
                }
            }
        }

        int key;
        vector<int>::iterator it;
        for (int i = 0; i < col; i++){
            if (board[0][i] == 'O'){
                key = Find_Set(i);
                it = find(keys.begin(), keys.end(), key);
                if (it == keys.end())
                	keys.push_back(key);
            }
            if (board[row - 1][i] == 'O'){
                key = Find_Set((row-1)*col + i);
                it = find(keys.begin(), keys.end(), key);
                if (it == keys.end())
                	keys.push_back(key);
            }
        }
        for (int i = 0; i < row; i++){
            if (board[i][0] == 'O'){
                key = Find_Set(i*col);
                it = find(keys.begin(), keys.end(), key);
                if (it == keys.end())
                	keys.push_back(key);
            }
            if (board[i][col - 1] == 'O'){
                key = Find_Set(i*col + col - 1);
                it = find(keys.begin(), keys.end(), key);
                if (it == keys.end())
                	keys.push_back(key);
            }
        }

        for (int i = 1; i < row - 1; i++){
            for (int j = 1; j < col - 1; j++){
                key = Find_Set(i*col + j);
                if (board[i][j] == 'O'){
                    it = find(keys.begin(), keys.end(), key);
                    if (it == keys.end())
                        board[i][j] = 'X';
                }
            }
        }
    }
};
```

执行用时：28 ms, 在所有 C++ 提交中击败了54.30%的用户

内存消耗：10.3 MB, 在所有 C++ 提交中击败了41.66%的用户

Attention:

- 路径压缩
- 添加key之前确保没有重复