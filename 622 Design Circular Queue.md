Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implementation the MyCircularQueue class:

- `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`.
- `int Front()` Gets the front item from the queue. If the queue is empty, return `-1`.
- `int Rear()` Gets the last item from the queue. If the queue is empty, return `-1`.
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return `true` if the operation is successful.
- `boolean deQueue()` Deletes an element from the circular queue. Return `true` if the operation is successful.
- `boolean isEmpty()` Checks whether the circular queue is empty or not.
- `boolean isFull()` Checks whether the circular queue is full or not.
You must solve the problem without using the built-in queue data structure in your programming language. 

**Example:**

```
Input
["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]

Output
[null, true, true, true, false, 3, true, true, true, 4]

Explanation
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
```

## Solution

```c++
class MyCircularQueue {
private:
    vector<int> data;
    int head;
    int tail;
    int size;

public:
    MyCircularQueue(int k): 
        head(-1),
        tail(-1),
        size(k),
        data(k)
    {}
    
    bool isEmpty() {
        return head == -1;
    }

    bool isFull() {
        return ((tail + 1) % size) == head;
    }
    
    int Front() {
        if (isEmpty())
            return -1;
        return data[head];
    }

    int Rear() {
        if (isEmpty())
            return -1;
        return data[tail];
    }
    
    bool enQueue(int value) {
        if (isFull())
            return false;

        if (isEmpty())
            head = 0;

        tail = (tail + 1) % size;
        data[tail] = value;
        return true;
    }

    bool deQueue() {
        if (isEmpty()) 
            return false;

        if (head == tail) {
            head = -1;
            tail = -1;
            return true;
        }

        head = (head + 1) % size;
        return true;
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了99.27%的用户

内存消耗：16.4 MB, 在所有 C++ 提交中击败了18.86%的用户
