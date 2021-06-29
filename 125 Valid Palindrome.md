Given a string, determine if it is a palindrome, considering only **alphanumeric** characters and **ignoring cases**.




Examples:

```
Example 1:

Input: "A man, a plan, a canal: Panama"
Output: true

Example 2:

Input: "race a car"
Output: false
```

## Solution - 双指针

由两侧向中间靠拢

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

执行用时：4 ms, 在所有 C++ 提交中击败了89.13%的用户

内存消耗：7.1 MB, 在所有 C++ 提交中击败了98.66%的用户

Attention:

- ```
  isalnum()
  ```
  
- ```
  tolower()
  ```

## Solution - 栈

遍历两次字符串，第一次将所有字母数字压入栈中。
第二次遍历时，比较当前元素与栈顶元素，如果相同则弹出。

### c++

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        stack<char> S;

        for (const char& c : s)
            if (isalnum(c))
                S.push(c);

        for (const char& c : s){
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

执行用时：4 ms, 在所有 C++ 提交中击败了89.13%的用户

内存消耗：7.7 MB, 在所有 C++ 提交中击败了5.66%的用户
