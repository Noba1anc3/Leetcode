Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)



**Example:**
```
Example 1:

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

Example 2:

Input: digits = ""
Output: []

Example 3:

Input: digits = "2"
Output: ["a","b","c"]
```

## Idea

假设输入是2，只有一个字符，那么应该怎么解呢？
按照题目要求2=“abc"，所以结果应该是["a","b","c"]
先不用想着怎么去写递归，只思考下怎么打印出这个结果。

```
result = List()
for(i=0;i<len("abc");i++) {
    tmp = i
    result.add(tmp)
}
return result
```

如果输入的是23，应该怎么做呢？23的结果是`["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]`，我们仍然不考虑怎么去写递归，只是考虑怎么把这个结果给弄出来。代码如下：

```
result = List()
for(i=0;i<len("abc");i++) {
    for(j=0;j<len("def");j++)
        tmp = i+j
        result.add(tmp)
}
return result
```


也就是说23这样的长度为2的字符串可以用两层循环搞定。

循环的嵌套层数，就是输入的字符串长度。输入的字符串长度是1，循环只有1层。
输入的字符串长度是3，循环就是3层。如果输入的字符串长度是10，那么循环就是10层。
可是输入的字符串长度是不固定的，对应的循环的嵌套层数也是不固定的，那这种情况怎么解决呢？这时候递归就派上用场了。

## Solution

```c++
class Solution {
private:
    vector<string> ans;
    string combination;
    unordered_map<char, string> mapping = {
        {'2', "abc"},
        {'3', "def"},
        {'4', "ghi"},
        {'5', "jkl"},
        {'6', "mno"},
        {'7', "pqrs"},
        {'8', "tuv"},
        {'9', "wxyz"}
    };

public:
    vector<string> letterCombinations(string digits) {
        if (!digits.empty())
        	backtrack(digits, 0);
        return ans;
    }

    void backtrack(string digits, int start){
        if (start == digits.length())
            ans.push_back(combination);
        else{
            string letters = mapping.at(digits[start]);
            for (char letter : letters){
                combination.push_back(letter);
                backtrack(digits, start + 1);
                combination.pop_back();
            }
        }
    }

};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.9 MB, 在所有 C++ 提交中击败了27.94%的用户

Attention

- ```c++
  unordered_map<type1, type2> hashmap = {{type1 a, type2 b},{},{}};
  ```

- ```
  unordered_map hashmap.at(index)
  ```

- ```c++
  string.pop_back()
  ```

