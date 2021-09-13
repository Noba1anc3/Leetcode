You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

 
Example
```
Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]

Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]

merging them into one sorted list:
1->1->2->3->4->4->5->6

Example 2:

Input: lists = []
Output: []

Example 3:

Input: lists = [[]]
Output: []
```

## Solution

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<int, vector<int>, greater<int>> Q;
        for (ListNode* LN : lists){
            while (LN != nullptr){
                Q.push(LN->val);
                LN = LN->next;
            }
        }
        ListNode* dummy = new ListNode(-1);
        ListNode* ln = dummy;
        while (!Q.empty()){
            int curVal = Q.top(); Q.pop();
            ln->next = new ListNode(curVal);
            ln = ln->next;
        }
        
        return dummy->next;
    }
};
```

执行用时：20 ms, 在所有 C++ 提交中击败了87.26%的用户

内存消耗：13.6 MB, 在所有 C++ 提交中击败了22.74%的用户
