There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
Return the minimum number of candies you need to have to distribute the candies to the children.

 
Example:
```
Example 1:

Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

Example 2:

Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

## Solution

```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> dp(ratings.size(), 1);
        for (int i = 1; i < ratings.size(); i++)
            if (ratings[i] > ratings[i-1])
                dp[i] = dp[i-1] + 1;

        for (int i = ratings.size() - 2; i >= 0; i--)
            if (ratings[i] > ratings[i+1])
                dp[i] = max(dp[i], dp[i+1] + 1);

        return accumulate(dp.begin(), dp.end(), 0);
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了74.13%的用户

内存消耗：17.1 MB, 在所有 C++ 提交中击败了88.97%的用户
