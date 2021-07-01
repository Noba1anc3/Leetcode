You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Solution 1

将长度较短的链表在末尾补0使得两个链表的长度相等，再一个一个元素对齐相加

Learned:
- ListNode* FuncName(ListNode* l1, ListNode* l2)
- new ListNode(0)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int len1 = 0, len2 = 0l;
        ListNode *p = l1, *q = l2;

        while(p->next != NULL)
        {
            len1++;
            p = p->next;
        }
        while(q->next != NULL)
        {
            len2++;
            q = q->next;
        }

        if(len1 > len2)
        {
            for(int i = 0; i < len1 - len2; i++)
            {
                q->next = new ListNode(0);
                q = q->next;
            }
        }
        else
        {
            for(int i = 0; i < len2 - len1; i++)
            {
                p->next = new ListNode(0);
                p = p->next;
            }
        }

        ListNode* l3 = new ListNode(-1); //存放结果的链表
        ListNode* w = l3; //l3的移动指针
        int i = 0; //记录相加结果
        int count = 0; //记录进位

        while(l1 != NULL && l2!= NULL)
        {
            i = count + l1->val + l2->val;
            w->next = new ListNode(i % 10);
            count = i >= 10 ? 1 : 0;
            w = w->next;
            l1 = l1->next;
            l2 = l2->next;
        }

        if(count) w->next = new ListNode(1);

        return l3->next; 
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了97.93%的用户

内存消耗：69.6 MB, 在所有 C++ 提交中击败了5.24%的用户

## Solution 2
不对齐补零

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(-1);
        ListNode* h = head;
        int sum = 0;
        int carry = 0;

        while(l1 != NULL || l2 != NULL)
        {
            sum=0;
            if(l1 != NULL)
            {
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2 != NULL)
            {
                sum += l2->val;
                l2 = l2->next;
            }
            if(carry) sum++;
            h->next = new ListNode(sum % 10);
            h = h->next;
            carry = sum >= 10 ? 1 : 0;
        }
        if(carry) h->next = new ListNode(1);

        return head->next;
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了97.93%的用户

内存消耗：69.6 MB, 在所有 C++ 提交中击败了5.24%的用户
