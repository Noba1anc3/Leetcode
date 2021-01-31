Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.



**Example:**

 ![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg) 

```
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]

Example 2:

Input: l1 = [], l2 = []
Output: []

Example 3:

Input: l1 = [], l2 = [0]
Output: [0]
```

## Solution

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* ans = new ListNode(0);
        ListNode* iter_ans = ans;
        while (l1 != NULL && l2 != NULL){
            if (l1->val < l2->val){
                iter_ans->next = l1;
                l1 = l1->next;
            }
            else{
                iter_ans->next = l2;
                l2 = l2->next;
            }
            iter_ans = iter_ans->next;
        }

        iter_ans->next = l1 == NULL ? l2 : l1;
        
        return ans->next;
    }
};
```

执行用时：24 ms, 在所有 C++ 提交中击败了97.81%的用户

内存消耗：18.7 MB, 在所有 C++ 提交中击败了91.59%的用户

Attention:
- ```c++
  ListNode* ans = new ListNode(0);
  ```
