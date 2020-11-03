输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

Examples:

```
Example 1:
输入: [10,2]
输出: "102"

Example 2:
输入: [3,30,34,5,9]
输出: "3033459"
```

## Idea

思路描述：首先我们要明白就是

- 无论这些数字怎么取排列，形成的数字的位数是不变的

那么就是高位的数字肯定是越小越好。

我们这里说的排列大小比较和字符串大小有点区别，比如 3 和 30，明显 30 排在前面比较好，所以我们要重构比较，我们组合 s1 和 s2 ，如果 s1 + s2 > s2 + s1，那么 s1 > s2

## Solution

c++

```c++
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string>strs;
        string ans;
        for(int i = 0; i < nums.size(); i++){
            strs.push_back(to_string(nums[i]));
        }
        sort(strs.begin(), strs.end(), [](string& s1, string& s2){return s1 + s2 < s2 + s1;});
        for(int i = 0; i < strs.size(); i++)
            ans += strs[i];
        return ans;
    }
};
```

OR

```c++
class Solution {
public:
    static bool compare(string s1, string s2){
        return s1 + s2 < s2 + s1;
    }

    string minNumber(vector<int>& nums) {
        vector<string>strs;
        string ans;
        for(int i = 0; i < nums.size(); i++){
            strs.push_back(to_string(nums[i]));
        }
        sort(strs.begin(), strs.end(), compare);
        for(int i = 0; i < strs.size(); i++)
            ans += strs[i];
        return ans;
    }
};
```



执行用时：8 ms, 在所有 C++ 提交中击败了98.29%的用户  
内存消耗：11.5 MB, 在所有 C++ 提交中击败了17.52%的用户

Attention:
- vector<int>&
- vector<string>
- vector.push_back()
- to_string(int)
- sort(A.begin(), A.end(), compare);
- Lambda in C++ 11

- 引用传递不需要调用构造函数去构造函数的局部变量 &s1