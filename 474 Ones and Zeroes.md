Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.



Examples:

```
Example 1:

Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].

Example 2:

Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

## Idea - Phase I

第一步，要明确两点，[状态]和[选择]。

状态有三个， [背包对1的容量]、[背包对0的容量]和 [可选择的字符串]；选择就是把字符串[装进背包]或者[不装进背包]。

明白了状态和选择，只要往这个框架套就完事儿了：


for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 计算(选择1，选择2...)
第二步，要明确dp数组的定义：

首先，[状态]有三个，所以需要一个三维的dp数组。

dp[i][j][k]的定义如下：

若只使用前i个物品，当背包容量为j个0，k个1时，能够容纳的最多字符串数。

经过以上的定义，可以得到：

base case为dp[0][..][..] = 0, dp[..][0][0] = 0。因为如果不使用任何一个字符串，则背包能装的字符串数就为0；如果背包对0，1的容量都为0，它能装的字符串数也为0。

我们最终想得到的答案就是dp[N][zeroNums][oneNums]，其中N为字符串的的数量。

第三步，根据选择，思考状态转移的逻辑：

注意，这是一个0-1背包问题，每个字符串只有一个选择机会，要么选择装，要么选择不装。

如果你不能把这第 i 个物品装入背包（等同于容量不足，装不下去），也就是说你不使用strs[i]这一个字符串，那么当前的字符串数dp[i][j][k]应该等于dp[i - 1][j][k],继承之前的结果。

如果你可以把这第 i 个物品装入了背包(此时背包容量是充足的，因此要选择装或者不装)，也就是说你能使用 strs[i] 这个字符串，那么 dp[i][j] 应该等于 Max(dp[i - 1][j][k], dp[i - 1][j - cnt[0]][k - cnt[1]] + 1)。 Max函数里的两个式子，分别是装和不装strs[i的字符串数量。(cnt 是根据strs[i]计算出来的。)

比如说，如果你想把一个cnt = [1,2]的字符串装进背包(在容量足够的前提下)，只需要找到容量为

[j - 1][k - 2]时候的字符串数再加上1，就可以得到装入后的字符串数了。

由于我们求的是最大值，所以我们要求的是装和不装中能容纳的字符串总数更大的那一个

作者：dong-men
链接：https://leetcode-cn.com/problems/ones-and-zeroes/solution/dong-tai-gui-hua-0-1bei-bao-wen-ti-labuladongdong-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

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

传递性优化前:

- 执行用时：972 ms, 在所有 C++ 提交中击败了10.68%的用户

- 内存消耗：11.9 MB, 在所有 C++ 提交中击败了30.89%的用户

传递性优化后:

- 执行用时：584 ms, 在所有 C++ 提交中击败了39.69%的用户

- 内存消耗：12.1 MB, 在所有 C++ 提交中击败了25.28%的用户

Attention:
- accumulate(nums.begin(), nums.end(), 0);
- *max_element(nums.begin(), nums.end());

- vector<vector<bool>> dp(size, vector<bool>(target+1, false));

## Idea - Phase II

上述代码的空间复杂度是 `O(n×target)`。但是可以发现在计算 `dp` 的过程中，每一行的 `dp` 值都只与上一行的 `dp` 值有关，因此只需要一个一维数组即可将空间复杂度降到 `O(target)`。此时的转移方程为:

`dp[j] = dp[j] ∣ dp[j−nums[i]]`

且需要注意的是第二层的循环我们需要**从大到小**计算，因为如果我们从小到大更新 `dp` 值，那么在计算 `dp[j]` 值的时候，`dp[j−nums[i]]` 已经是被更新过的状态，不再是上一行的 `dp` 值.

## Solution - Phase II

```c++
class Solution {
public:
    bool judge(int size, int target, int maxElement){
        if (target % 2 == 1 or 2*maxElement > target or size == 1)
            return true;
        return false;
    }

    bool canPartition(vector<int>& nums) {
        int size = nums.size();
        int target = accumulate(nums.begin(), nums.end(), 0);
        int maxElement = *max_element(nums.begin(), nums.end());

        if (judge(size, target, maxElement))
            return false;

        target /= 2;
        vector<bool> dp(target + 1, false);

        dp[0] = true;
        for (int i = 0; i < size; i++) {
            int num = nums[i];
            for (int j = target; j >= num; j--) {
                dp[j] |= dp[j - num];
            }
        }

        return dp[target];

    }
};
```

执行用时：96 ms, 在所有 C++ 提交中击败了85.31%的用户

内存消耗：10 MB, 在所有 C++ 提交中击败了52.08%的用户

**复杂度**

- 时间复杂度：`O(n×target)`，其中 `n` 是数组的长度，`target` 是整个数组的元素和的一半。需要计算出所有的状态，每个状态在进行转移时的时间复杂度为 O(1).

- 空间复杂度：`O(target)`， `target` 是整个数组的元素和的一半。空间复杂度取决于 `dp` 数组，在不进行空间优化的情况下，空间复杂度是 `O(n×target)`，进行空间优化的情况下，空间复杂度可以降到 `O(target)`.

