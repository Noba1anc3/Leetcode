Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.



**Example:**

```
Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

Example 2:

Input: n = 1
Output: ["()"]
```

## Idea - I Back Trace

我们可以生成所有 2^{2n}个 `'('` 和 `')'` 字符构成的序列，然后我们检查每一个是否有效即可。

为了生成所有序列，我们可以使用递归。长度为 `n` 的序列就是在长度为 `n-1` 的序列后加一个 `'('` 或 `')'`。

为了检查序列是否有效，我们遍历这个序列，并使用一个变量 `balance` 表示左括号的数量减去右括号的数量。如果在遍历过程中 `balance` 的值小于零，或者结束时 `balance` 的值不为零，那么该序列就是无效的，否则它是有效的。

## Solution - I

c++


```c++
#include <iostream>
#include <vector>
#include <unordered_map>

using namespace std;

class Solution {
private:
    std::vector<char> brackets = {'(', ')'};
    std::string tmp_ans;
    std::vector<string> ans;

public:
    bool check(std::string& ans){
        int balance = 0;
        for (char c : ans){
            if (c == '(') balance++;
            else balance--;
            if (balance < 0) return false;
        }
        return balance == 0;
    }

    void parenthesis(int length){
        if (tmp_ans.size() == 2*length) {
            if (check(tmp_ans))
                ans.push_back(tmp_ans);
        }
        else{
            for (char bracket : brackets){
                tmp_ans.push_back(bracket);
                parenthesis(length);
                tmp_ans.pop_back();
            }
        }
    }

    std::vector<string> generateParenthesis(int n) {
        parenthesis(n);
        return ans;
    }
};


int main()
{
    Solution slt = Solution();
    std::vector<string> ans_vector = slt.generateParenthesis(2);
    for (std::string ans : ans_vector){
        std::cout << ans <<'\n';
    }
    return 0;
}
```

执行用时：12 ms, 在所有 C++ 提交中击败了14.22%的用户

内存消耗：7.1 MB, 在所有 C++ 提交中击败了96.92%的用户

**复杂度分析**

- 时间复杂度：O(2^{2n}n)，对于 2^{2n}个序列中的每一个，我们用于建立和验证该序列的复杂度为 O(n)。

- 空间复杂度：O(n)，除了答案数组之外，我们所需要的空间取决于递归栈的深度，每一层递归函数需要 O(1) 的空间，最多递归 2n 层，因此空间复杂度为 O(n)。

Attention

- 判断括号是否匹配时用左括号数量减去右括号数量即可，不需要使用栈，内存消耗过高

- 能用指针就用指针传值，速度快

## Idea - II 回溯剪枝

第一个方法是每次生成完整的串之后再判断是不是正确的串，修改为只在序列有效时才添加括号。这就需要再递归时将左右括号的个数传进去。如果左括号数量不大于 n，我们可以放一个左括号。如果右括号数量小于左括号的数量，我们可以放一个右括号。(下面有图)

![](https://pic.leetcode-cn.com/7ec04f84e936e95782aba26c4663c5fe7aaf94a2a80986a97d81574467b0c513-LeetCode%20%E7%AC%AC%2022%20%E9%A2%98%EF%BC%9A%E2%80%9C%E6%8B%AC%E5%8F%B7%E7%94%9F%E5%87%BA%E2%80%9D%E9%A2%98%E8%A7%A3%E9%85%8D%E5%9B%BE.png)



## Solution - II

```c++
class Solution {
private:
    string slt;
    vector<string> tmp_ans;
    vector<string> ans;

public:
    bool check(string &ans){
        int balance = 0;
        for (char c : ans){
            if (c == '(')
                balance++;
            else
                balance--;
            if (balance < 0)
                return false;
        }
        return balance == 0;
    }
    
    void parenthesis(int length, int left, int right){
        if (slt.size() == 2*length){
            if (check(slt))
                ans.push_back(slt);
        }
        else{
            if (left < length){
                char bracket = '(';
                slt.push_back(bracket);
                parenthesis(length, left+1, right);
                slt.pop_back();
            }
            if (right < left){
                char bracket = ')';
                slt.push_back(bracket);
                parenthesis(length, left, right+1);
                slt.pop_back();            
            }
        }
    }

    vector<string> generateParenthesis(int n) {
        parenthesis(n, 0, 0);
        return ans;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：11.8 MB, 在所有 C++ 提交中击败了61.43%的用户
