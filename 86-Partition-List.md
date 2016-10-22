## NO.86. Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given 1->4->3->2->5->2 and x = 3,
return 1->2->2->4->3->5.

## 思路
1. 使用递归，分析头节点是否是小于x
	- 小于x，则可以直接向后移一位，进行递归调用
	- 大于等于x，从后面取出第一个比x小的节点插入到最前面，再进行递归调用。

2. 维护两个链表，依次遍历链表，第一个链表存放小于x的节点，第二个链表存放大于等于x的节点，最后将两个链表链接返回。

## 实现
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
    ListNode* partitionImpl(ListNode* head, ListNode* search, int x)
    {
        if(!head || !search ) return head;
        if(head->val < x)
        {
            head->next = partitionImpl(head->next, head->next, x);
            return head;
        }
        else
        {
            while(search && search->next && search->next->val >= x) search = search->next;
            if(search->next == NULL) return head;
            ListNode* headTmp = search->next;
            search->next = search->next->next;
            headTmp->next = partitionImpl(head, search, x);
            return headTmp;
        }
    }
    ListNode* partition(ListNode* head, int x)
    {
        if(!head || !head->next) return head;
        return partitionImpl(head, head, x);
    }
    
};
```
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
    ListNode* partition(ListNode* head, int x)
    {
        if(!head || !head->next) return head;
        ListNode *left_start = NULL, *left_end = NULL, 
                    *right_start = NULL, *right_end = NULL;
        while(head)
        {
            if(head->val < x)
            {
                if(!left_start) left_start = left_end = head;
                else left_end->next = head, left_end = left_end->next;
            }
            else
            {
                if(!right_start) right_start = right_end = head;
                else right_end->next = head, right_end = right_end->next;
            }
            head = head->next;
        }
        if(right_end) right_end->next = NULL;
        if(!left_start) return right_start;
        left_end->next = right_start;
        return left_start;
    }
    
};
```
