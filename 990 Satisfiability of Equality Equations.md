Given an array `equations` of strings that represent relationships between variables, each string `equations[i]` has length 4 and takes one of two different forms: `"a==b"` or `"a!=b"`.  Here, `a` and `b` are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.



Examples:

```
Example 1:

Input: ["a==b","b!=a"]
Output: false

Explanation: If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.  There is no way to assign the variables to satisfy both equations.

Example 2:

Input: ["b==a","a==b"]
Output: true

Explanation: We could assign a = 1 and b = 1 to satisfy both equations.

Example 3:

Input: ["a==b","b==c","a==c"]
Output: true

Example 4:

Input: ["a==b","b!=c","c==a"]
Output: false

Example 5:

Input: ["c==c","b==d","x!=z"]
Output: true
```

## Solution

c++

```c++
class Solution {
private:
    vector<int> parent;
    vector<string> equal;
    vector<string> unequal;
    unordered_map<char, int> M;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];

        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;
        
        parent[ROOT2] = ROOT1;
        parent[x] = ROOT1;
        parent[y] = ROOT1;
    }

    bool equationsPossible(vector<string>& equations) {
        int n = 1;
        for (string& equation : equations){
            char a = equation[0];
            char b = equation[3];
            if (M[a] == 0)
                M[a] = n++;
            if (M[b] == 0)
                M[b] = n++;
            if (equation[1] == '!')
                unequal.push_back(equation);
            else
                equal.push_back(equation);
        }

        parent.resize(n);
        for (int i = 1; i < n; i++)
            parent[i] = i;

        for (string& equation : equal)
            Union(M[equation[0]], M[equation[3]]);

        for (string& equation : unequal){
            int a = M[equation[0]], b = M[equation[3]];

            if (Find_Set(a) == Find_Set(b))
                return false;
        }
        return true;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了40.07%的用户

内存消耗：12.6 MB, 在所有 C++ 提交中击败了5.55%的用户

Attention

- 对于非整数而是字符或字符串的下标想使用并查集需要用`unordered_map<char, int>` 或 `unordered_map<string, int>`，直接用字符或字符串作为下标查询即可，初始状态下结果是0，需要赋值一个`i++`作为其索引

然鹅，通过观察可以发现，本问题只有26个英文字母的可能范围，没有必要为了一点点空间去消耗更多的时间，直接创建字母表并查集

```c++
class Solution {
private:
    vector<int> parent, height;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 == ROOT2)
            return;
        
        if (height[ROOT1] <= height[ROOT2]) {
            parent[ROOT2] = ROOT1;
            height[ROOT2] = height[ROOT1] + 1;
            parent[y] = ROOT1;
            height[y] = height[ROOT1] + 1;
        }
        else{
            parent[ROOT1] = ROOT2;
            height[ROOT1] = height[ROOT2] + 1;
            parent[x] = ROOT2;
            height[x] = height[ROOT2] + 1;
        }
    }

    bool equationsPossible(vector<string>& equations) {
        parent.resize(26);
        height.resize(26);
        
        for (int i = 0; i < 26; i++) 
            parent[i] = i;

        for (const string& equation : equations){
            if (equation[1] == '='){
                int a = equation[0] - 'a';
                int b = equation[3] - 'a';
                Union(a, b);
            }
        }

        for (const string& equation : equations){
            if (equation[1] == '!'){
                int a = equation[0] - 'a';
                int b = equation[3] - 'a';
                if (Find_Set(a) == Find_Set(b))
                    return false;
            }
        }
        return true;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了88.71%的用户

内存消耗：11.2 MB, 在所有 C++ 提交中击败了11.70%的用户
