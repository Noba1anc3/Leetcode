Given a list `accounts`, each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are emails representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some email that is common to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in any order.



Examples:

```
Input: 

accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], 
			["John", "johnnybravo@mail.com"], 
			["John", "johnsmith@mail.com", "john_newyork@mail.com"], 
			["Mary", "mary@mail.com"]]

Output: [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  
		 ["John", "johnnybravo@mail.com"], 
		 ["Mary", "mary@mail.com"]]

Explanation: 
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer 
[['Mary', 'mary@mail.com'], 
 ['John', 'johnnybravo@mail.com'], 
 ['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']]
would still be accepted.
```

## Solution

c++

```c++
class Solution {
private:
    vector<int> parent;
    unordered_set<string> mailSet;
    unordered_map<string, int> mailUserMap;

public:
    int Find_Set(int x){
        while (parent[x] != x)
            x = parent[x];

        return x;
    }

    void Union(int x, int y){
        int ROOT1 = Find_Set(x), ROOT2 = Find_Set(y);

        if (ROOT1 != ROOT2){
            parent[ROOT2] = ROOT1;
            parent[x] = ROOT1;
            parent[y] = ROOT1;
        }
    }

    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        parent.resize(accounts.size());
        for(int i = 0; i < parent.size(); i++) 
            parent[i] = i;

        for (int i = 0; i < accounts.size(); i++) {
            for(int j = 1; j < accounts[i].size(); j++) {
                string mail = accounts[i][j];
                if (!mailSet.count(mail)) {
                    mailSet.insert(mail);
                    mailUserMap[mail] = i;
                }
                else{
                    Union(i, mailUserMap[mail]);
                }
            }
        }

        // 遍历accounts中每个人名，寻找每个人名的父辈，并将该人名下所有邮箱保存到一起
        // 由于会存在重复邮箱，且要考虑邮箱顺序，所以使用set<string>来去重并内部自动排序
        unordered_map<int, set<string>> accMail;
        for (int i = 0; i < accounts.size(); i++) {
            int key = Find_Set(i);
            int len = accounts[i].size();

            for(int j = 1; j < len; j++) 
                accMail[key].insert(accounts[i][j]);
        }

        // 遍历accMail，其中每一项为<人名位置， 该人名下所有的邮箱>
        // 将其转化为题目需要的返回格式
        vector<vector<string>> res;
        
        for(auto& acc: accMail)
        {
            vector<string> ans;
            ans.push_back(accounts[acc.first][0]);
            
            for(auto& mail: acc.second) 
                ans.push_back(mail);

            res.push_back(ans);
        }

        return res;
    }
};
```

执行用时：168 ms, 在所有 C++ 提交中击败了85.61%的用户

内存消耗：31.2 MB, 在所有 C++ 提交中击败了62.59%的用户

Attention

- ```c++
  unordered_set.count(ele)
  unordered_set.insert(ele)
  ```

- ```c++
  unordered_map[index]
  ```

- ```c++
  set.insert()
  ```