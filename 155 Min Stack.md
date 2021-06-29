Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**Example:**
```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

## Solution

### 辅助栈

按照上面的思路，我们只需要设计一个数据结构，使得每个元素 a 与其相应的最小值 m 时刻保持一一对应。因此我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。

- 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；

- 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；

- 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。

python
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = [math.inf]

    def push(self, x: int) -> None:
        self.stack.append(x)
        self.min_stack.append(min(x, self.min_stack[-1]))

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

执行用时：64 ms, 在所有 Python3 提交中击败了83.80%的用户

内存消耗：18.2 MB, 在所有 Python3 提交中击败了13.82%的用户

c++

```c++
class MinStack {
private:
    stack<int> S, minimum;

public:
    /** initialize your data structure here. */
    MinStack() {
        minimum.push(INT_MAX);
    }
    
    void push(int val) {
        S.push(val);
        minimum.push(min(minimum.top(), val));
    }
    
    void pop() {
        S.pop();
        minimum.pop();
    }
    
    int top() {
        return S.top();
    }
    
    int getMin() {
        return minimum.top();
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了95.86%的用户

内存消耗：16 MB, 在所有 C++ 提交中击败了31.39%的用户

### 数值齐存
```java
class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<Integer>();
    public void push(int x) {
        //当前值更小
        if(x <= min){   
            //将之前的最小值保存
            stack.push(min);
            //更新最小值
            min=x;
        }
        stack.push(x);
    }

    public void pop() {
        //如果弹出的值是最小值，那么将下一个元素更新为最小值
        if(stack.pop() == min) {
            min=stack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min;
    }
}
```

执行用时：6 ms, 在所有 Java 提交中击败了97.64%的用户  
内存消耗：40.8 MB, 在所有 Java 提交中击败了25.56%的用户

