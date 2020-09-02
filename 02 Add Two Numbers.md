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

```
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int len1=1;//记录l1的长度
        int len2=1;//记录l2的长度
        ListNode* p=l1;
        ListNode* q=l2;

        while(p->next!=NULL)//获取l1的长度
        {
            len1++;
            p=p->next;
        }
        while(q->next!=NULL)//获取l2的长度
        {
            len2++;
            q=q->next;
        }

        if(len1>len2)//l1较长，在l2末尾补零
        {
            for(int i=1;i<=len1-len2;i++)
            {
                q->next=new ListNode(0);
                q=q->next;
            }
        }
        else//l2较长，在l1末尾补零
        {
            for(int i=1;i<=len2-len1;i++)
            {
                p->next=new ListNode(0);
                p=p->next;
            }
        }


	p=l1;
        q=l2;
        bool count=false;//记录进位
        ListNode* l3=new ListNode(-1);//存放结果的链表
        ListNode* w=l3;//l3的移动指针
        int i=0;//记录相加结果
	
	while(p!=NULL&&q!=NULL)
        {
            i=count+p->val+q->val;
            w->next=new ListNode(i%10);
            count=i>=10?true:false;
            w=w->next;
            p=p->next;
            q=q->next;
        }
        if(count)//若最后还有进位
        {
            w->next=new ListNode(1);
            w=w->next;
        }
        return l3->next; 
    }
};

```

Time : 28ms  
Memory : 9.4MB

## Solution 2
不对齐补零

```
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head=new ListNode(-1);//存放结果的链表
        ListNode* h=head;//移动指针
        int sum=0;//每个位的加和结果
        bool carry=false;//进位标志
        while(l1!=NULL||l2!=NULL)
        {
            sum=0;
            if(l1!=NULL)
            {
                sum+=l1->val;
                l1=l1->next;
            }
            if(l2!=NULL)
            {
                sum+=l2->val;
                l2=l2->next;
            }
            if(carry)
                sum++;
            h->next=new ListNode(sum%10);
            h=h->next;
            carry=sum>=10?true:false;
        }
        if(carry)
        {
            h->next=new ListNode(1);
        }
        return head->next;
    }
};

```
Time : 32ms  
Memory: 9.3MB
