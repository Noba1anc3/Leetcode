You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: ```'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'```. The wheels can rotate freely and wrap around: for example we can turn ```'9'``` to be ```'0'```, or ```'0'``` to be ```'9'```. Each move consists of turning one wheel one slot.

The lock initially starts at ```'0000'```, a string representing the state of the 4 wheels.

You are given a list of ```deadends``` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a ```target``` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

**Example:**
```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".


Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".


Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation:
We can't reach the target without getting stuck.


Input: deadends = ["0000"], target = "8888"
Output: -1
```

## Solution
我们可以将 ```0000``` 到 ```9999``` 这 10000 状态看成图上的 10000 个节点，两个节点之间存在一条边，当且仅当这两个节点对应的状态只有 1 位不同，且不同的那位相差 1（包括 0 和 9 也相差 1 的情况），并且这两个节点均不在数组 ```deadends``` 中。最终的答案即为 0000 到 target 的最短路径。

我们用广度优先搜索来找到最短路径，从 ```0000``` 开始搜索。对于每一个状态，它可以扩展到最多 8 个状态，即将它的第 i = 0, 1, 2, 3 位增加 1 或减少 1，将这些状态中没有搜索过并且不在 ```deadends``` 中的状态全部加入到队列中，并继续进行搜索。注意 ```0000``` 本身有可能也在 ```deadends``` 中。

```python
class Solution(object):
    def openLock(self, deadends, target):
        def neighbors(node):
            for i in range(4):
                x = int(node[i])
                for d in (-1, 1):
                    y = (x + d) % 10
                    yield node[:i] + str(y) + node[i+1:]

        dead = set(deadends)
        queue = collections.deque([('0000', 0)])
        seen = {'0000'}
        while queue:
            node, depth = queue.popleft()
            if node == target: return depth
            if node in dead: continue
            for nei in neighbors(node):
                if nei not in seen:
                    seen.add(nei)
                    queue.append((nei, depth+1))
        return -1
```

执行用时：664 ms, 在所有 Python3 提交中击败了51.34%的用户

内存消耗：16.1 MB, 在所有 Python3 提交中击败了42.43%的用户

Attention:  
- 表盘问题使用广度优先搜索
- yield in python
- node, depth = queue.popleft()

c++
```c++
class Solution {
public:
    std::vector<string> neighbors(string& cur){
        std::vector<string> neighbors;
        for (int i = 0; i < 4; i++){
            for (const int& d : {-1, 1}){
                string nei_left = cur.substr(0, i);
                string nei_right = cur.substr(i+1, 3-i);
                int nei_mid = (cur[i] - '0' + d) % 10;
                if (nei_mid < 0) nei_mid += 10;
                string neighbor = nei_left + to_string(nei_mid) + nei_right;
                neighbors.push_back(neighbor);
            }
        }
        return neighbors;
    }

    int openLock(std::vector<string>& deadends, string target) {
        std::set<string> seen = {"0000"};
        std::queue<std::pair<string, int>> Q;
        Q.push(std::pair<string, int>("0000", 0));

        while (!Q.empty()){
            string cur = Q.front().first;
            int step = Q.front().second;
            Q.pop();
            if (cur == target) 
                return step;
            if (find(deadends.begin(), deadends.end(), cur) != deadends.end()) 
                continue;
            std::vector<string> neis = neighbors(cur);
            for (const string& nei : neis){
                if (seen.find(nei) == seen.end()){
                    seen.insert(nei);
                    Q.push(std::pair<string, int>(nei, step + 1));
                }
            }
        }

        return -1;
    }
};
```

执行用时：1064 ms, 在所有 C++ 提交中击败了5.11%的用户

内存消耗：138.7 MB, 在所有 C++ 提交中击败了5.05%的用户

Attention:
- int(char) 会转为ascii码，正确做法是 ch - '0'
- c++求余会求出符数，如果小于0，需要加余数
