Given a string ```s``` containing just the characters ```'('```, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.  
Open brackets must be closed in the correct order.

**Example:**
```
Example 1:

Input: s = "()"
Output: true

Example 2:

Input: s = "()[]{}"
Output: true

Example 3:

Input: s = "(]"
Output: false

Example 4:

Input: s = "([)]"
Output: false

Example 5:

Input: s = "{[]}"
Output: true
```

## Solution
判断括号的有效性可以使用「栈」这一数据结构来解决。

我们对给定的字符串 `s` 进行遍历，当我们遇到一个左括号时，我们会期望在后续的遍历中，有一个相同类型的右括号将其闭合。由于**后遇到的左括号要先闭合**，因此我们可以将这个左括号放入栈顶。

当我们遇到一个右括号时，我们需要将一个相同类型的左括号闭合。此时，我们可以取出栈顶的左括号并判断它们是否是相同类型的括号。如果不是相同的类型，或者栈中并没有左括号，那么字符串 `s` 无效，返回 `False`。为了快速判断括号的类型，我们可以使用哈希映射（HashMap）存储每一种括号。哈希映射的键为右括号，值为相同类型的左括号。

在遍历结束后，如果栈中没有左括号，说明我们将字符串 `s` 中的所有左括号闭合，返回 `True`，否则返回 `False`。

注意到有效字符串的长度一定为偶数，因此如果字符串的长度为奇数，我们可以直接返回 ```False```，省去后续的遍历判断过程。

Python
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 == 1:
            return False
        
        pairs = {
            ")": "(",
            "]": "[",
            "}": "{",
        }
        
        stack = list()
        for ch in s:
            if ch in pairs:
                if not stack or stack[-1] != pairs[ch]:
                    return False
                stack.pop()
            else:
                stack.append(ch)
        
        return not stack
```
执行用时：44 ms, 在所有 Python3 提交中击败了56.48%的用户  
内存消耗：13.2 MB, 在所有 Python3 提交中击败了92.82%的用户

Attention:
- 可能存在的特殊条件判断，例如长度是奇数
- 字典的使用（哈希）
- list() = []

### c++

```c++
class Solution {
private:
    unordered_map<char, char> kuohao = {
        {')', '('},
        {']', '['},
        {'}', '{'}
    };
    stack<char> S;

public:
    bool isValid(string s) {
        for (const auto& ch : s){
            if (kuohao.find(ch) != kuohao.end()){
                if (S.empty() || S.top() != kuohao.at(ch))
                    return false;
		S.pop();
            }
            else{
                S.push(ch);
            }
        }

        return S.empty();
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.3 MB, 在所有 C++ 提交中击败了87.10%的用户
