Given the `head` of a linked list, return the list after sorting it in **ascending** order.

Follow up: Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

 

Examples:

```
Example 1:

Input: head = [4,2,1,3]
Output: [1,2,3,4]

Example 2:

Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

## Idea

时间复杂度是 O(n log n) 的排序算法包括归并排序、堆排序和快速排序（快速排序的最差时间复杂度是 O(n^2)），其中最适合链表的排序算法是归并排序。

对**数组**做归并排序需要的空间复杂度为 O(n) --> 新开辟数组 O(n) + 递归调用函数 O(logn);
对**链表**做归并排序可以通过修改引用来更改节点位置，因此不需要向数组一样开辟额外的 O(n) 空间，但是只要是递归就需要消耗 O(log n) 的空间复杂度，要达到 O(1) 空间复杂度的目标，得使用迭代法。

因此对于链表进行排序有两种方案：
（1）递归实现归并排序（空间复杂度不符合要求）
（2）迭代实现归并排序

### 技巧一：通过快慢指针找到链表中点
需要确定链表的中点以进行两路归并。可以通过快慢指针的方法。快指针每次走两步，慢指针每次走一步。遍历完链表时，慢指针停留的位置就在链表的中点。

```c++
ListNode* slow = head;
ListNode* fast = head->next; 

while(fast != NULL && fast->next != NULL){ 
    slow = slow->next; //慢指针走一步
    fast = fast->next->next; //快指针走两步
}

ListNode* rightHead = slow->next; //链表第二部分的头节点
slow->next = NULL; //cut 链表
```

### 技巧二：断链操作
`split(l,n)` 即切掉链表l的前n个节点，并返回后半部分的链表头。
比如原来链表是`dummy->1->2->4->3->NULL`
`split(l,2)`的操作造成：

```
dummy->1->2->NULL 
4->3->NULL
```

```c++
ListNode* split(ListNode* head, int step){
    if(head == NULL)  return NULL;
    ListNode* cur = head;

    //注意这里 cur->next != NULL 有可能出现后半段还没到规定步长但是走完的情况
    for(int i = 1; i < step && cur->next != NULL; i++)
        cur = cur->next;

    ListNode* right = cur->next; //right为后半段链表头
    cur->next = NULL; //切断前半段
    return right; //返回后半段链表头
}
```

### **技巧三：合并两个有序链表**

```c++
ListNode* merge(ListNode* h1, ListNode* h2){
    ListNode* head = new ListNode(-1); //新创建一个伪头节点
    ListNode* p = head;

    while(h1 != NULL && h2 != NULL){
        if (h1->val < h2->val){
            p->next = h1;
            h1 = h1->next;
        }
        else{
            p->next = h2;
            h2 = h2->next;
        }
        p = p->next;           
    }

    p->next = h1 == NULL ? h2 : h1;

    return head->next;  //返回排序好的链表头    
}
```

## Solution

### 递归

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
    ListNode* sortList(ListNode* head) {
        if(head == NULL || head->next == NULL) return head;
        
        ListNode* rightHead = getRightHalf(head);
        
        ListNode* left = sortList(head); //递归排序前一段链表
        ListNode* right = sortList(rightHead); //递归排序后一段链表
        
        return merge(left, right);
    }  
    
    ListNode* getRightHalf(ListNode* head) {
        ListNode* slow = head; //慢指针
        ListNode* fast = head->next; //快指针
        
        while(fast != NULL && fast->next != NULL){ 
            slow = slow->next; //慢指针走一步
            fast = fast->next->next; //快指针走两步
        }

        ListNode* rightHead = slow->next; //链表第二部分的头节点
        slow->next = NULL; //cut 链表
        
        return rightHead;
    }
    
    ListNode* merge(ListNode* h1, ListNode* h2){
        ListNode* head = new ListNode(-1); //新创建一个伪头节点
        ListNode* p = head;
        
        while(h1 != NULL && h2 != NULL){
            if (h1->val < h2->val){
                p->next = h1;
                h1 = h1->next;
            }
            else{
                p->next = h2;
                h2 = h2->next;
            }
            p = p->next;
        }

        p->next = h1 == NULL ? h2 : h1;

        return head->next;  //返回排序好的链表头    
    }
};
```

执行用时：120 ms, 在所有 C++ 提交中击败了69.31%的用户

内存消耗：47.5 MB, 在所有 C++ 提交中击败了22.26%的用户

### 迭代

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
    ListNode* sortList(ListNode* head) {
        int length = getLength(head);
        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        for(int step = 1; step < length; step *= 2) { //依次将链表分成1块，2块，4块...
            //每次变换步长，pre指针和cur指针都初始化在链表头
            ListNode* pre = dummy;
            ListNode* cur = dummy->next;

            while(cur != nullptr) {
                ListNode* h1 = cur; //第一部分头（第二次循环之后，cur为剩余部分头，不断往后把链表按照步长step分成一块一块...）
                ListNode* h2 = split(h1, step);  //第二部分头
                cur = split(h2, step); //剩余部分的头
                pre->next = merge(h1, h2); //将前面的部分与排序好的部分连接
                while(pre->next != nullptr) pre = pre->next; //把pre指针移动到排序好的部分的末尾
            }
        }

        return dummy->next;
    }
    
    int getLength(ListNode* head) {
        //获取链表长度
        int count = 0;
        while(head != nullptr){
            count++;
            head = head->next;
        }
        return count;
    }
    
    ListNode* split(ListNode* head, int step){
        //断链操作 返回第二部分链表头
        if(head == nullptr)  return head;
        for(int i = 1; i < step && head->next != nullptr; i++)
            head = head->next;
        ListNode* right = head->next;
        head->next = nullptr; //切断连接
        return right;
    }
    
    ListNode* merge(ListNode* h1, ListNode* h2){
        ListNode* head = new ListNode(-1);
        ListNode* p = head;
        
        while(h1 != nullptr && h2 != nullptr){
            if (h1->val < h2->val){
                p->next = h1;
                h1 = h1->next;
            }
            else{
                p->next = h2;
                h2 = h2->next;
            }
            p = p->next;
        }

        p->next = h1 == nullptr ? h2 : h1;

        return head->next;  
    }
};
```

执行用时：124 ms, 在所有 C++ 提交中击败了63.19%的用户

内存消耗：31.9 MB, 在所有 C++ 提交中击败了50.89%的用户
