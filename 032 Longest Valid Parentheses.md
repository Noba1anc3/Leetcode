Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.



**Example:**
```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".

Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".

Input: s = ""
Output: 0
```

## Idea - I DP

我们定义 `dp[i]` 表示以下标 `i` 字符结尾的最长有效括号的长度。我们将 `dp` 数组全部初始化为 0 。显然有效的子串一定以 `‘)’` 结尾，因此我们可以知道以 `‘(’` 结尾的子串对应的 `dp` 值必定为 `0` ，我们只需要求解 `‘)’` 在 `dp` 数组中对应位置的值。

我们从前往后遍历字符串求解 `dp` 值，我们每两个字符检查一次：

1. `s[i] = ‘)’` 且 `s[i - 1] = ‘(’`，也就是字符串形如 “……()”，我们可以推出 : `dp[i] = dp[i−2] + 2`

   我们可以进行这样的转移，是因为结束部分的 "()" 是一个有效子字符串，并且将之前有效子字符串的长度增加了 2 。

2. `s[i] = ‘)’` 且 `s[i - 1] = ‘)’`，也就是字符串形如 “……))”，我们可以推出：

   如果 `s[i − 1 - dp[i − 1]] = ‘(’`，那么 `dp[i] = dp[i - 1] + dp[i - 2 - dp[i - 1]] + 2`

   我们考虑如果倒数第二个 `‘)’` 是一个有效子字符串的一部分（记作 `sub_s`），对于最后一个 `‘)’`，如果它是一个更长子字符串的一部分，那么它一定有一个对应的 `'(’` ，且它的位置在倒数第二个 `‘)’` 所在的有效子字符串的前面（也就是 `sub_s `的前面）。因此，如果子字符串 `sub_s` 的前面恰好是 `'(’` ，那么我们就用 2 加上 `sub_s` 的长度（`dp[i−1]`）去更新 `dp[i]`。同时，我们也会把有效子串 “(sub_s)” 之前的有效子串的长度也加上，也就是再加上 `dp[i − 2 - dp[i − 1]]`。

## Solution - I

### c++

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int len = s.size(), dpMax = 0;
        vector<int> dp(len, 0);

        for (int i = 1; i < len; i++)
            if (s[i] == ')'){
                if (s[i-1] == '(')
                    dp[i] = (i >= 2) ? dp[i-2] + 2 : 2;
                else if (s[i-1] == ')'){
                    if (i-dp[i-1] >= 1 && s[i-1-dp[i-1]] == '(')
                        dp[i] = (i-dp[i-1] >= 2) ? dp[i-1] + dp[i-2-dp[i-1]] + 2 : dp[i-1] + 2;
                }
                dpMax = max(dpMax, dp[i]);
            } 

        return dpMax;

    }
};
```
执行用时：4 ms, 在所有 C++ 提交中击败了93.84%的用户

内存消耗：7.6 MB, 在所有 C++ 提交中击败了20.94%的用户

### java

```java
class Solution {
public:
    int longestValidParentheses(string s) {
        int dpMax = 0;
        int[] dp = new int[s.length()];

        for (int i = 1; i < s.length(); i++)
            if (s.charAt(i) == ')'){
                if (s.charAt(i-1) == '(')
                    dp[i] = (i >= 2) ? dp[i-2] + 2 : 2;
                else if (s.charAt(i-1) == ')'){
                    if (i-dp[i-1] >= 1 && s.charAt(i-1-dp[i-1]) == '(')
                        dp[i] = (i-dp[i-1] >= 2) ? dp[i-1] + dp[i-2-dp[i-1]] + 2 : dp[i-1] + 2;
                }
                dpMax = Math.max(dpMax, dp[i]);
            } 

        return dpMax;

    }
};
```

执行用时：1 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：38.3 MB, 在所有 Java 提交中击败了87.59%的用户

Attention

- String.length()
- int[] array = new int[s.length()]
- String.charAt(i)
- Math.max(a, b)

**复杂度分析**

- 时间复杂度：O(n)

- 空间复杂度：O(n)
