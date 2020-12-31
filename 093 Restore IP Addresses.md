Given a string `s` containing only digits, return all possible valid IP addresses that can be obtained from `s`. You can return them in **any** order.

A **valid IP address** consists of exactly four integers, each integer is between `0` and `255`, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are **valid** IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are **invalid** IP addresses. 



**Example:**

```
Example 1:

Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]

Example 2:

Input: s = "0000"
Output: ["0.0.0.0"]

Example 3:

Input: s = "1111"
Output: ["1.1.1.1"]

Example 4:

Input: s = "010010"
Output: ["0.10.0.10","0.100.1.0"]

Example 5:

Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

## Solution


```c++
class Solution {
private:
    string ip;
    vector<string> ips;

public:
    void backtrack(string& s, int piece, int index){
        // ip串有四个组成部分或开头指示指针越界则返回，如果两部分同时满足则添加一个有效ip，注意去掉最后的'.'
        if (piece == 4 || index == s.size()){
            if (piece == 4 && index == s.size())
                ips.push_back(ip.substr(0, ip.size() - 1));
            return;
        }
        // 可能一位，两位，三位
        for (int i = 1; i <= 3; i++){
            // 如果当前指针加当前长度超过长度限制则返回
            if (index + i > s.size()) return;
            // 如果开始位是0并且当前长度不是1则返回
            if (s[index] == '0' && i != 1) return;
            // 如果有三位并且大于255则返回
            if (i == 3 && s.substr(index, i) > "255") return;
            ip += s.substr(index, i);
            ip.push_back('.');
            // 段数加一，指针后移
            backtrack(s, piece + 1, index + i);
            // 回溯时去掉新加的数字和小数点
            ip = ip.substr(0, ip.size() - i - 1);
        }
    }

    vector<string> restoreIpAddresses(string s) {
        backtrack(s, 0, 0);
        return ips;
    }
};
```

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.7 MB, 在所有 C++ 提交中击败了90.16%的用户