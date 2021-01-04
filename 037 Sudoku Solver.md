Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

Each of the digits `1-9` must occur exactly once in each row.
Each of the digits `1-9` must occur exactly once in each column.
Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.
The `'.'` character indicates empty cells.



![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Example:**

```
Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:
```

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

Constraints:

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit or `'.'`.
- It is guaranteed that the input board has only one solution.

## Solution

c++


```c++
class Solution {
private:
    int unfill = 0;

public:
    void fillCount(vector<vector<char>>& board){
        for (int i = 0; i < 9; i++)
            for (int j = 0; j < 9; j++)
                if (board[i][j] == '.')
                    unfill++;
    }

    bool check(vector<vector<char>>& board, int row, int col, int num){
        for (int i = 0; i < 9; i++)
            if (board[i][col] - '0' == num || board[row][i] - '0' == num)
                return false;

        int rowBlock = row / 3;
        int colBlock = col / 3;
        for (int i = rowBlock * 3; i < (rowBlock + 1) * 3; i++)
            for (int j = colBlock * 3; j < (colBlock + 1) * 3; j++)
                if (board[i][j] - '0' == num)
                    return false;

        return true;
    }

    void backtrack(vector<vector<char>>& board){
        for (int i = 0; i < 9; i++){
            for (int j = 0; j < 9; j++){
                if (board[i][j] == '.'){
                    for (int k = 1; k < 10; k++){
                        if (check(board, i, j, k)){
                            // 改变现场
                            board[i][j] = k + '0';
                            unfill--;
                            // 进入下一层
                            backtrack(board);
                            // 退出递归的条件
                            if (unfill == 0)
                                return;
                            // 恢复现场
                            board[i][j] = '.';
                            unfill++;
                        }
                    }
                    // 提前返回，进行回溯
                    if (board[i][j] == '.')
                        return;
                }
            }
        }
    }

    void solveSudoku(vector<vector<char>>& board) {
        fillCount(board);
        backtrack(board);
    }
};
```

Attention

- char -> int : `'c' - '0'`
- 回溯函数三要素
  - 返回的条件
  - 回溯的条件（找到最小的可回溯条件，在本问题中如若一个位置1-10都不能成立则进行回溯）
  - 遍历情况，进行递归

执行用时：36 ms, 在所有 C++ 提交中击败了32.28%的用户

内存消耗：6.6 MB, 在所有 C++ 提交中击败了78.87%的用户