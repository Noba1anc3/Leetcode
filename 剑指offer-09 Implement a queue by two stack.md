用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )



 **Example:**

```
示例 1：

输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]

示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

## Solution
### c++

```c++
class CQueue {
private:
    stack<int> s1, s2;
    
public:
    CQueue() {
    }
    
    void appendTail(int value) {
        s1.push(value);
    }
    
    int deleteHead() {
        if (!s2.empty()){
            int head = s2.top();
            s2.pop();
            return head;
        }
        while (!s1.empty()){
            int ele = s1.top();
            s1.pop();
            s2.push(ele);
        }
        if (!s2.empty()) {
            int head = s2.top();
            s2.pop();
            return head;
        }
        return -1;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

执行用时：308 ms, 在所有 C++ 提交中击败了98.83%的用户

内存消耗：101 MB, 在所有 C++ 提交中击败了97.89%的用户
