Given a string `s`, return *the longest palindromic substring* in `s`.



**Example:**
```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Input: s = "cbbd"
Output: "bb"

Input: s = "a"
Output: "a"

Input: s = "ac"
Output: "a"
```

## Idea - I DP

状态转移方程:

*P*(*i*, *j*) = *P*(*i*+1, *j*−1) ∧ (*S*i == *S*j)

上文讨论是建立在子串长度大于 2 的前提之上的，我们还需要考虑动态规划中的边界条件，即子串的长度为 1 或 2。对于长度为 1 的子串，它显然是个回文串；对于长度为 2 的子串，只要它的两个字母相同，它就是一个回文串。因此我们就可以写出动态规划的边界条件：

 *P*(*i*, *i*) = true

*P*(*i*, *i*+1) = (*S*i == *S*i + 1)

## Solution

### c++

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.size();
        bool dp[len][len];
        string ans;

        for (int l = 1; l <= len; l++){
            for (int i = 0; i <= len - l; i++){
                int j = i + l - 1;
                if (l == 1)
                    dp[i][j] = true;
                else if (l == 2)
                    dp[i][j] = (s[i]==s[j]);
                else
                    dp[i][j] = (dp[i+1][j-1] && s[i]==s[j]); 

                if (dp[i][j] && l > ans.size())
                    ans = s.substr(i, l);
            }
        }

        return ans;
    }
};

```

执行用时：276 ms, 在所有 C++ 提交中击败了52.32%的用户

内存消耗：25.3 MB, 在所有 C++ 提交中击败了58.56%的用户

Attention
- 由短到长的遍历

### java

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        String ans = "";
        for (int l = 0; l < n; l++) {
            for (int i = 0; i + l < n; i++) {
                int j = i + l;
                if (l == 0) {
                    dp[i][j] = true;
                } else if (l == 1) {
                    dp[i][j] = (s.charAt(i) == s.charAt(j));
                } else {
                    dp[i][j] = (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]);
                }
                if (dp[i][j] && l + 1 > ans.length()) {
                    ans = s.substring(i, i + l + 1);
                }
            }
        }
        return ans;
    }
}
```

Attention
- String.length()
- boolean[] array = new boolean[n]
- String.charAt(i)
- String.substring(i, j)

**复杂度分析**

- 时间复杂度：O(n^2)

- 空间复杂度：O(n^2)

## Idea - II 中心扩展算法

状态转移链：
P(i, j) ← P(i+1, j−1) ← P(i+2, j−2) ← ⋯ ← 某一边界情况

可以发现，所有的状态在转移的时候的可能性都是唯一的。也就是说，我们可以从每一种边界情况开始「扩展」，也可以得出所有的状态对应的答案。

边界情况即为子串长度为 1 或 2 的情况。我们枚举每一种边界情况，并从对应的子串开始不断地向两边扩展。如果两边的字母相同，我们就可以继续扩展，例如从 P(i+1, j-1) 扩展到 P(i, j)；如果两边的字母不同，我们就可以停止扩展，因为在这之后的子串都不能是回文串了。

## Solution - II

c++

```c++
class Solution {
public:
    pair<int, int> expandAroundCenter(string s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            left--;
            right++;
        }
        return {left + 1, right - 1};
    }

    string longestPalindrome(string s) {
        int start = 0, end = 0;
        for (int i = 0; i < s.size(); i++) {
            auto [left1, right1] = expandAroundCenter(s, i, i);
            auto [left2, right2] = expandAroundCenter(s, i, i + 1);
            if (right1 - left1 > end - start) {
                start = left1; end = right1;
            }
            if (right2 - left2 > end - start) {
                start = left2; end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }
};
```

执行用时：80 ms, 在所有 C++ 提交中击败了68.02%的用户

内存消耗：226.2 MB, 在所有 C++ 提交中击败了34.90%的用户

**复杂度分析**

- 时间复杂度：O(n^2)

- 空间复杂度：O(1)

Attention
- auto
