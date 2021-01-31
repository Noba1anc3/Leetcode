输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。 



**Example:**
```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

## Idea - I 

直接遍历

## Solution - I

### c++

```c++
class Solution {
public:
    vector<int> printNumbers(int n) {
        vector<int> ans;
        for(int i = 1; i < pow(10, n); i++)
            ans.push_back(i);

        return ans;
    }       
};
```
执行用时：4 ms, 在所有 C++ 提交中击败了98.72%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了71.73%的用户

**复杂度分析**：
时间复杂度 O(10^n) ：生成长度为 10^n 的列表需使用 O(10^n) 时间
空间复杂度 O(1)：建立列表需使用 O(1) 大小的额外空间（ 列表作为返回结果，不计入额外空间 ）

## Idea - II

**大数打印解法**：
实际上，本题的主要考点是大数越界情况下的打印。需要解决以下三个问题：

1. 表示大数的变量类型：
无论是 short / int / long ... 任意变量类型，数字的取值范围都是有限的。因此，大数的表示应用字符串 String 类型。
2. 生成数字的字符串集：
  使用 int 类型时，每轮可通过 +1 生成下个数字，而此方法无法应用至 String 类型。并且， String 类型的数字的进位操作效率较低，例如 "9999" 至 "10000" 需要从个位到千位循环判断，进位 4 次。

  观察可知，生成的列表实际上是 n 位 0 - 9 的全排列，因此可避开进位操作，通过递归生成数字 String 列表。

使用回溯算法

## Solution - II

```c++
class Solution {
private:
    vector<string> ans;
    string res;

public:
    void backtrack(int width){
        if (width == 0){
            ans.push_back(res);
            return;
        }

        for (int i = 0; i < 10; i++){
            res += to_string(i);
            backtrack(width - 1);
            res = res.substr(0, res.size() - 1);
        }
    }

    vector<int> printNumbers(int n) {
        backtrack(n);
        
		vector<int> ret_ans;
        for (string num : ans)
            ret_ans.push_back(atoi(num.c_str()));

        vector<int> new_ans(ret_ans.begin() + 1, ret_ans.end());
        return new_ans;
    }       
};
```

执行用时：48 ms, 在所有 C++ 提交中击败了5.22%的用户

内存消耗：20.8 MB, 在所有 C++ 提交中击败了5.05%的用户

Attention:

- ```c++
  string.substr(start_index, len)
  ```

- ```c++
  string -> int : atoi(string.c_str())
  ```

- ```
  int -> string : to_string(int)
  ```