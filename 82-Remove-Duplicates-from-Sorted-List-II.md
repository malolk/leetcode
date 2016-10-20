## NO.82. Remove Duplicates from Sorted List II

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given 1->2->3->3->4->4->5, return 1->2->5.
Given 1->1->1->2->3, return 2->3. 

## 思路
1. （方法1）计算有效的head节点，然后再计算其他节点
2. （方法2）对步骤1使用递归

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
    ListNode* unique(ListNode* l)
    {
        while(l)
        {
            int cnt = 1;
            ListNode* ptr = l;
            while(ptr->next && ptr->next->val == l->val)
            {
                cnt++; ptr = ptr->next;
            }
            if(cnt == 1) break;
            else if(!ptr->next) return NULL;
            else l = ptr->next;
        }
        return l;
    }
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL || head->next == NULL) return head;
        if(!(head = unique(head))) return NULL;
        ListNode *prev = head, *ptr = head->next;
        while(true)
        {
            prev->next = unique(ptr);
            if(prev->next) prev = prev->next;
            else break;
            ptr = prev->next;
        }
        return head;
    }
};

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
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL || head->next == NULL) return head;
        if(head->val != head->next->val)
        {
            head->next = deleteDuplicates(head->next);
            return head;
        }
        else
        {
            ListNode *ptr = head;
            while(ptr->next && ptr->next->val == head->val) ptr = ptr->next;
            return deleteDuplicates(ptr->next);
        }
    }
};
```