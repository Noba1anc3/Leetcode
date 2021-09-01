Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

```
Example 1:

Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]

Example 2:

Input: head = [5], left = 1, right = 1
Output: [5]
```

## Solution

```c++
class Solution {
private:
    void reverseLinkedList(ListNode *head) {
        ListNode *pre = nullptr;
        ListNode *cur = head;

        while (cur != nullptr) {
            ListNode *next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
    }

public:
    ListNode *reverseBetween(ListNode *head, int left, int right) {
        ListNode *dummyNode = new ListNode(-1);
        dummyNode->next = head;

        ListNode *pre = dummyNode;
        for (int i = 0; i < left - 1; i++)
            pre = pre->next;
        
        ListNode *rightNode = pre;
        for (int i = 0; i < right - left + 1; i++)
            rightNode = rightNode->next;

        ListNode *leftNode = pre->next;
        ListNode *post = rightNode->next;

        pre->next = nullptr;
        rightNode->next = nullptr;

        reverseLinkedList(leftNode);

        pre->next = rightNode;
        leftNode->next = post;
        
        return dummyNode->next;
    }
};
```
