Reverse a singly linked list. 



**Example:**
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

## Idea - I 双指针迭代

好理解的双指针

- 定义两个指针： pre = NULL 和 cur = head；pre 在前 cur 在后。
- 每次让 cur 的 next 指向 pre，实现一次局部反转
  局部反转完成之后， pre 和 cur 同时往前移动一个位置
  循环上述过程，直至 cur 到达链表尾部，返回pre

## Solution - I

### c++

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *pre = NULL, *cur = head;
        
        while (cur != NULL){
            ListNode* tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }

        return pre;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了67.91%的用户

内存消耗：8.2 MB, 在所有 C++ 提交中击败了29.57%的用户
