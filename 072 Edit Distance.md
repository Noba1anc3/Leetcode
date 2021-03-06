Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character



Examples:

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

## Solution

c++

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int len1 = word1.size();
        int len2 = word2.size();
        int dp[len1+1][len2+1];
        int c = 1;

        for (int i = 0; i <= len1; i++)
            dp[i][0] = i;
        for (int i = 0; i <= len2; i++)
            dp[0][i] = i;

        for (int i = 1; i <= len1; i++){
            for (int j = 1; j <= len2; j++){
                if (word1[i-1] == word2[j-1])
                    c = 0;
                else
                    c = 1;
                dp[i][j] = min(min(dp[i][j-1], dp[i-1][j]) + 1, dp[i-1][j-1] + c);
            }
        }

        return dp[len1][len2];
    }
};
```
执行用时：12 ms, 在所有 C++ 提交中击败了89.51%的用户

内存消耗：7.4 MB, 在所有 C++ 提交中击败了83.82%的用户