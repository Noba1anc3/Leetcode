



Examples:

```

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

如果你**可以**把这第 `i` 个物品装入了背包(此时背包容量是充足的，因此要选择装或者不装)，也就是说你能使用 `strs[i]` 这个字符串， `dp[i][j] = max(dp[i - 1][j][k], dp[i - 1][j - cnt[0]][k - cnt[1]] + 1)`. max函数里的两个式子，分别是装和不装`strs[i]`的字符串数量。(`cnt` 是根据`strs[i]`计算出来的)

### Thinking

这道题可以换一种表述：给定一个只包含正整数的非空数组 `nums`，判断是否可以从数组中选出一些数字，使得这些数字的和等于整个数组的元素和的一半。因此这个问题可以转换成「0-1背包问题」。这道题与传统的「0-1背包问题」的区别在于，传统的「0-1背包问题」要求选取的物品的重量之和**不能超过**背包的总容量，这道题则要求选取的数字的和**恰好等于**整个数组的元素和的一半。类似于传统的「0-1背包问题」，可以使用动态规划求解.

### Judge Before Algorithm

在使用动态规划求解之前，首先需要进行以下判断.

- 根据数组的长度 n 判断数组是否可以被划分。如果 n<2，则不可能将数组分割成元素和相等的两个子集，因此直接返回 false.

- 计算整个数组的元素和 `sum` 以及最大元素 `maxNum`。如果 `sum` 是奇数，则不可能将数组分割成元素和相等的两个子集，因此直接返回 `false`。如果 `sum` 是偶数，则令 `target`=sum/2 ，需要判断是否可以从数组中选出一些数字，使得这些数字的和等于`target`。如果 `maxNum ` > `target` ，则除了 `maxNum `以外的所有元素之和一定小于 `target`，因此不可能将数组分割成元素和相等的两个子集，直接返回 `false`.

### DP

#### Array Construction

创建二维数组 `dp`，包含 `n` 行 `target+1` 列，其中 `dp[i][j]` 表示从数组的 `[0,i]` 下标范围内选取若干个正整数（可以是 0 个），是否存在一种选取方案使得被选取的正整数的和等于 `j`.

#### Initialization

初始时，`dp` 中的全部元素都是 `false`.

#### Border Situations

在定义状态之后，需要考虑边界情况。以下两种情况都属于边界情况.

如果不选取任何正整数，则被选取的正整数等于 0。因此对于所有 `0≤i<n`，都有 `dp[i][0] =true`.

当 `i=0` 时，只有一个正整数 `nums[0]` 可以被选取，因此 `dp[0][nums[0]] = true`.

#### Equation of State Transition

对于 `i>0` 且 `j>0` 的情况，如何确定 `dp[i][j]` 的值？需要分别考虑以下两种情况。

- 如果 `j >= nums[i]`，则对于当前的数字 `nums[i]`，可以选取也可以不选取，两种情况只要有一个为 `true`，就有 `dp[i][j]=true`.
  - 如果不选取 `nums[i]`，则 `dp[i][j] = dp[i-1][j]`.
  - 如果选取 `nums[i]`，则  `dp[i][j] = dp[i-1][j-nums[i]]`.
- 如果 `j < nums[i]`，则在选取的数字的和等于 `j` 的情况下无法选取当前的数字 `nums[i]`，因此有 `dp[i][j] = dp[i-1][j]`.

## Solution - Phase I

c++
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
        int dp[strs.size()+1][m+1][n+1];
        for (int i = 0; i <= m; i++){
            for (int j = 0; j <= n; j++){
                dp[0][i][j] = 0;
            }
        }
        for (int i = 0; i <= strs.size(); i++){
            dp[i][0][0] = 0;
        }
        
        for (int i = 1; i <= strs.size(); i++){
            string str = strs[i-1];
            vector<int> zero_one = num_zero_one(str);

            for(int j = 0; j <= m; j++){
                for (int k = 0; k <= n; k++){
                    if (zero_one[0] > j or zero_one[1] > k)
                        dp[i][j][k] = dp[i-1][j][k];
                    else
                        dp[i][j][k] = max(dp[i-1][j][k], 1 + dp[i-1][j-zero_one[0]][k-zero_one[1]]);  
                }
            }
        }

        return dp[strs.size()][m][n];
    }
};
```

## Idea - Phase II



## Solution - Phase II

```c++

```

