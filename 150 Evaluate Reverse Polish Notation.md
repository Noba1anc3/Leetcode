Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Note:

- Division between two integers should truncate toward zero.
- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example:**
```
Example 1:

Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Example 2:

Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Example 3:

Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```

## Solution

**栈与递归之间在某种程度上是可以转换的！**

逆波兰表达式相当于二叉树中的后序遍历。 把运算符作为中间节点，按照后序遍历的规则画出一个二叉树。

c++
```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        set<string> operators = {"+", "-", "*", "/"};
        stack<int> S;

        for (const string& token : tokens) {
            if (operators.find(token) == operators.end()) {
                S.push(stoi(token));
            } 
            else {
                int num1 = S.top(); S.pop();
                int num2 = S.top(); S.pop();
                if (token == "+") S.push(num2 + num1);
                if (token == "-") S.push(num2 - num1);
                if (token == "*") S.push(num2 * num1);
                if (token == "/") S.push(num2 / num1);
            }
        }
        return S.top();
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了72.99%的用户

内存消耗：11.7 MB, 在所有 C++ 提交中击败了14.85%的用户

Attention:
- stoi(string)

During the 1970s and 1980s, Hewlett-Packard used RPN in all of their desktop and hand-held calculators, and continued to use it in some models into the 2020s.


