The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.



**Example:**

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above

Input: n = 1
Output: [["Q"]]
```

`1 <= n <= 9`

## Solution

c++


```c++
class Solution {
private:
    int N;
    vector<string> res;
    vector<vector<string>> ans;
    vector<vector<int>> chess;

public:
    bool putCheck(int row, int col){
        if (chess[row][col] != 0)
            return false;

        for(int i = 0; i < N; i++)
            if (chess[i][col] == 1 || chess[row][i] == 1)
                return false;
        for(int i = row, j = col; i < N && j < N; i++, j++)
            if (chess[i][j] == 1)
                return false;
        for(int i = row, j = col; i >= 0 && j >= 0; i--, j--)
            if (chess[i][j] == 1)
                return false;
        for(int i = row, j = col; i >= 0 && j < N; i--, j++)
            if (chess[i][j] == 1)
                return false;
        for(int i = row, j = col; i < N && j >= 0; i++, j--)
            if (chess[i][j] == 1)
                return false;
    
        return true;
    }

    void chessIn(int row, int col){
        for(int i = 0; i < N; i++){
            chess[i][col] = -1;
            chess[row][i] = -1;
        }
        for(int i = row + 1, j = col + 1; i < N && j < N; i++, j++)
            chess[i][j] = -1;
        for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            chess[i][j] = -1;
        for(int i = row - 1, j = col + 1; i >= 0 && j < N; i--, j++)
            chess[i][j] = -1;
        for(int i = row + 1, j = col - 1; i < N && j >= 0; i++, j--)
            chess[i][j] = -1;   

        chess[row][col] = 1;   
    }

    void chessOut(int row){
        int col = 0;
        for (int i = 0; i < N; i++)
            if (chess[row][i] == 1)
                col = i;
                
        for(int i = 0; i < N; i++){
            chess[i][col] = 0;
            chess[row][i] = 0;
        }
        for(int i = row + 1, j = col + 1; i < N && j < N; i++, j++)
            chess[i][j] = 0;
        for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            chess[i][j] = 0;
        for(int i = row - 1, j = col + 1; i >= 0 && j < N; i--, j++)
            chess[i][j] = 0;
        for(int i = row + 1, j = col - 1; i < N && j >= 0; i++, j--)
            chess[i][j] = 0;   

        chess[row][col] = 0;   
    }

    void backtrack(int row){
        if (row == N){
            ans.push_back(res);
            return;
        }
        for (int i = 0; i < N; i++){
            if (putCheck(row, i)){
                res.push_back(to_string(row) + to_string(i));
                chessIn(row, i);
                backtrack(row+1);
                res.pop_back();
                chessOut(row);
            }
        }

    }

    vector<vector<string>> ansProcess(){
        vector<vector<string>> results;
        for (vector<string> res: ans){
            vector<string> result;
            for(string point : res){
                string str = "";
                for (int i = 0; i < point[1] - '0'; i++){
                    str += ".";
                }
                str += "Q";
                for (int i = 0; i <  N - 1 - (point[1] - '0'); i++){
                    str += ".";
                }
                result.push_back(str);
            }
            results.push_back(result);
        }

        return results;
    }

    vector<vector<string>> solveNQueens(int n) {
        N = n;
        chess.resize(n, (vector<int>(n, 0)));
        
        backtrack(0);

        vector<vector<string>> result;
        result = ansProcess();

        return result;
    }
};
```

执行用时：32 ms, 在所有 C++ 提交中击败了29.17%的用户

内存消耗：8.7 MB, 在所有 C++ 提交中击败了30.73%的用户