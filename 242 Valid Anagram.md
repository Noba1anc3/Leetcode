Given two strings s and t , write a function to determine if t is an anagram of s.

You may assume the string contains only lowercase alphabets.



Examples:

```
Example 1:

Input: s = "anagram", t = "nagaram"
Output: true

Example 2:

Input: s = "rat", t = "car"
Output: false
```

## Idea

### 排序

通过将 `s` 的字母重新排列成 `t` 来生成变位词。因此，如果 `T` 是 `S` 的变位词，对两个字符串进行排序将产生两个相同的字符串。此外，如果 `s` 和 `t` 的长度不同，`t` 不能是 `s` 的变位词，我们可以提前返回。

#### 复杂度

时间复杂度：`O(n log n)`，假设 `n` 是 `s` 的长度，排序成本 `O(n log n)` 和比较两个字符串的成本 `O(n)`，排序时间占主导地位．

### Hashmap

为了检查 `t` 是否是 `s` 的重新排列，我们可以计算两个字符串中每个字母的出现次数并进行比较。因为 `S` 和 `T` 都只包含 `A-Z` 的字母，所以一个简单的 26 位计数器表就足够了。
我们需要两个计数器数表进行比较吗？实际上不是，因为我们可以用一个计数器表计算 `s` 字母的频率，用 `t` 减少计数器表中的每个字母的计数器，然后检查计数器是否回到零。

#### 复杂度

时间复杂度：`O(n)`  因为访问计数器表是一个固定的时间操作。
空间复杂度：`O(1)`  尽管我们使用了额外的空间，但无论 `N` 有多大，表的大小都保持不变。

## Solution

### 排序

c++

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size())
            return false;
  
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());

        if (s == t)
            return true;
        return false;
    }
};
```

python

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        if sorted(s) != sorted(t):
            return False
        return True
```

### Hashmap

c++

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size())
            return false;
        
        int alphabet[26] = {0};

        for(int i = 0; i < s.size(); i++){
            alphabet[int(s[i]) - 97] += 1;
            alphabet[int(t[i]) - 97] -= 1;
        }

        for (int i = 0; i < 26; i++)
            if (alphabet[i] != 0)
                return false;
        return true;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了98.18%的用户

内存消耗：7.1 MB, 在所有 C++ 提交中击败了99.28%的用户

python

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        
        dicts = collections.defaultdict(int)
        
        for i in range(len(s)):
            dicts[s[i]] += 1
            dicts[t[i]] -= 1
            
        for val in dicts.values():
            if val != 0:
                return False
        return True
```

Attention

- int alphabet[26] = {0};
- a = 97
