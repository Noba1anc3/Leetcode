链接：https://www.nowcoder.com/questionTerminal/24c2045f2cce40a5bf410a369a001da8?f=discussion
来源：牛客网

将整数 n 分成 k 份，且每份不能为空，任意两个方案不能相同(不考虑顺序)。

例如： n=7, k=3 下面三种分法被认为是相同的。

1，1，5;

1，5，1;

5，1，1;

问有多少种不同的分法, 答案对 10^9+7取模。

输入： n，k

6<n≤5000，2≤k≤1000

输出：一个整数，即不同的分法。


## Solution - Backtrack

```c++
class Solution {
private:
    set<vector<int>> ans;
    vector<int> tmp;
    vector<int> sorted_tmp;

public:
    void backtrack(int n, int k){
        if (k == 1){
            sorted_tmp.clear();
            sorted_tmp.insert(sorted_tmp.end(), tmp.begin(), tmp.end());
            sorted_tmp.push_back(n);
            sort(sorted_tmp.begin(), sorted_tmp.end());
            ans.insert(sorted_tmp);
        }
        else
            for (int i = 1; i <= n - k + 1; i++){
                tmp.push_back(i);
                backtrack(n-i, k-1);
                tmp.pop_back();
            }

    }

    int divide(int n, int k) {
        backtrack(n, k);
        return ans.size();
    }
};
```

## Solution - DP

```c++
class Solution {
public:
    int divide(int n, int k) {
        int dp[n+1][k+1];
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= min(k, i); j++)
                dp[i][j] = (dp[i-j][j] + dp[i-1][j-1]);

        return dp[n][k];
    }
};
```
