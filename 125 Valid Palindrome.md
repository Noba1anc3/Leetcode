Given a string, determine if it is a palindrome, considering only **alphanumeric** characters and **ignoring cases**.




Examples:

```
Example 1:

Input: "A man, a plan, a canal: Panama"
Output: true
Example 2:

Input: "race a car"
Output: false

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-palindrome
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## Solution - 双指针

### c++

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0, j = s.size() - 1;

        while (i < j){
            while (i < j && !isalnum(s[i]))
                i++;
            while (i < j && !isalnum(s[j]))
                j--;

            if (tolower(s[i]) == tolower(s[j])){
                i++;
                j--;
            }
            else
                return false;
        }

        return true;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了95.13%的用户

内存消耗：7.1 MB, 在所有 C++ 提交中击败了98.66%的用户

Attention:

- ```
  isalnum()
  ```

## Solution - 栈

### c++

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        stack<char> S;

        for (const char c : s)
            if (isalnum(c))
                S.push(c);

        for (const char c : s){
            if (isalnum(c)){
                if (tolower(S.top()) == tolower(c))
                    S.pop();
                else
                    return false;
            }
        }

        return true;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了95.13%的用户

内存消耗：7.7 MB, 在所有 C++ 提交中击败了42.66%的用户