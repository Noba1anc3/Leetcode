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

```c++
class Solution {
public:
    static bool cmp(string a, string b){
        return a + b < b + a;
    }

    string minNumber(vector<int>& nums) {
        string rsp;
        vector<string> str_nums;

        for (const int& num : nums)
            str_nums.push_back(to_string(num));

        sort(str_nums.begin(), str_nums.end(), cmp);
     // sort(str_nums.begin(), str_nums.end(), [](string& a, string& b){return a + b < b + a;});
     
        for (const string& str_num : str_nums)
            rsp += str_num;
            
        return rsp;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了78.55%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了10.65%的用户

```c++
class Solution {
public:
    static bool cmp(string a, string b){
        return a + b < b + a;
    }

    string minNumber(vector<int>& nums) {
        string rsp;
        vector<string> str_nums;
        vector<int>::iterator int_it = nums.begin();

        while (int_it != nums.end())
            str_nums.push_back(to_string(*int_it++));

        sort(str_nums.begin(), str_nums.end(), cmp);

        vector<string>::iterator str_it = str_nums.begin();
        while (str_it != str_nums.end())
            rsp += *str_it++;

        return rsp;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了97.26%的用户

内存消耗：11.1 MB, 在所有 C++ 提交中击败了52.94%的用户

Attention:
- to_string(int)
- sort(A.begin(), A.end(), compare);
- Lambda in C++ 11

- 引用传递不需要调用构造函数去构造函数的局部变量 &s1
