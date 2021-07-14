
You are given an array of binary strings `strs` and two integers `m` and `n`.

Return the size of the largest subset of `strs` such that there are **at most** `m` `0`'s and `n` `1`'s in the subset.

A set `x` is a subset of a set `y` if all elements of `x` are also elements of `y`.


Examples:

```
Example 1

Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.


Example 2

Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.
```

## Idea - Phase I

#### 第一步，要明确两点，[状态]和[选择]。

状态有三个， [背包对1的容量]、[背包对0的容量]和 [可选择的字符串]；选择就是把字符串[装进背包]或者[不装进背包]。

明白了状态和选择，只要往这个框架套就完事儿了:

```c
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 计算(选择1，选择2...)
```

#### 第二步，要明确dp数组的定义：

首先，[状态]有三个，所以需要一个三维的dp数组。

`dp[i][j][k]`的定义如下:

若只使用前`i`个物品，当背包容量为`j`个0，`k`个1时，能够容纳的最多字符串数。

经过以上的定义，可以得到:

base case为`dp[0][..][..] = 0`, `dp[..][0][0] = 0`。因为如果不使用任何一个字符串，则背包能装的字符串数就为0；如果背包对0，1的容量都为0，它能装的字符串数也为0。

我们最终想得到的答案就是`dp[N][zeroNums][oneNums]`，其中N为字符串的的数量。

#### 第三步，根据选择，思考状态转移的逻辑:

注意，这是一个0-1背包问题，每个字符串只有一个选择机会，要么选择装，要么选择不装。

如果你**不能**把这第 `i` 个物品装入背包（等同于容量不足，装不下去），也就是说你不使用`strs[i]`这一个字符串，那么当前的字符串数`dp[i][j][k]`应该等于`dp[i - 1][j][k]`,继承之前的结果。

如果你**可以**把这第 `i` 个物品装入了背包(此时背包容量是充足的，因此要选择装或者不装)，也就是说你能使用 `strs[i]` 这个字符串， `dp[i][j] = max(dp[i - 1][j][k], dp[i - 1][j - cnt[0]][k - cnt[1]] + 1)`. max函数里的两个式子，分别是装和不装`strs[i]`的字符串数量。`cnt` 是根据`strs[i]`计算出来的)

## Solution - Phase I

c++
```c++
class Solution {
public:
    vector<int> num_zero_one(string str){
        vector<int> zero_one(2,0);

        for(const char& ch : str)
            if (ch == '0') zero_one[0] += 1;
            else zero_one[1] += 1;

        return zero_one;
    }

    int findMaxForm(vector<string>& strs, int m, int n) {
        int dp[strs.size()+1][m+1][n+1];

        for (int i = 0; i <= m; i++)
            for (int j = 0; j <= n; j++)
                dp[0][i][j] = 0;
        for (int i = 0; i <= strs.size(); i++)
            dp[i][0][0] = 0;

        for (int i = 1; i <= strs.size(); i++){
            vector<int> zero_one = num_zero_one(strs[i-1]);
            for(int j = 0; j <= m; j++)
                for (int k = 0; k <= n; k++)
                    if (zero_one[0] > j || zero_one[1] > k)
                        dp[i][j][k] = dp[i-1][j][k];
                    else
                        dp[i][j][k] = max(dp[i-1][j][k], 1 + dp[i-1][j-zero_one[0]][k-zero_one[1]]);  
        }

        return dp[strs.size()][m][n];
    }
};
```

执行用时：184 ms, 在所有 C++ 提交中击败了84.61%的用户

内存消耗：31.7 MB, 在所有 C++ 提交中击败了36.02%的用户

## Solution - Phase II

```c++
class Solution {
public:

    vector<int> num_zero_one(string str){
        vector<int> zero_one(2,0);

        for(auto& ch : str){
            if (ch == '0')
                zero_one[0] += 1;
            else
                zero_one[1] += 1;
        }

        return zero_one;
    }

    int findMaxForm(vector<string>& strs, int m, int n) {
        int dp[m+1][n+1];
        
        for (int i = 0; i <= m; i++){
            for (int j = 0; j <= n; j++){
                dp[i][j] = 0;
            }
        }
        
        for (int i = 1; i <= strs.size(); i++){
            vector<int> zero_one = num_zero_one(strs[i-1]);

            for(int j = m; j >= 0; j--)
                for (int k = n; k >= 0; k--)
                    if (zero_one[0] <= j and zero_one[1] <= k)
                        dp[j][k] = max(dp[j][k], 1 + dp[j-zero_one[0]][k-zero_one[1]]); 
                    // before optimize
                    // if (zero_one[0] > j or zero_one[1] > k)
                    //     dp[j][k] = dp[j][k];
                    // else
                    //     dp[j][k] = max(dp[j][k], 1 + dp[j-zero_one[0]][k-zero_one[1]]); 
        }
        
        return dp[m][n];
    }
};
```

执行用时：344 ms, 在所有 C++ 提交中击败了35.48%的用户

内存消耗：9.1 MB, 在所有 C++ 提交中击败了84.39%的用户
