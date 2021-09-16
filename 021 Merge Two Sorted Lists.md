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
        ListNode* rsp = new ListNode(0);
        ListNode* iter_rsp = rsp;

        while (l1 != NULL && l2 != NULL){
            if (l1->val < l2->val){
                iter_rsp->next = l1;
                l1 = l1->next;
            }
            else{
                iter_rsp->next = l2;
                l2 = l2->next;
            }
            iter_rsp = iter_rsp->next;
        }

        iter_rsp->next = l1? l1 : l2;
        
        return rsp->next;
    }
};
```

执行用时：8 ms, 在所有 C++ 提交中击败了82.48%的用户

内存消耗：14.4 MB, 在所有 C++ 提交中击败了53.60%的用户

Attention:
- ```c++
  ListNode* ans = new ListNode(0);
  ```
