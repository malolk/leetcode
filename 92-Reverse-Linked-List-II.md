## NO.92. Reverse Linked List II
Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,

return 1->4->3->2->5->NULL.

Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

## 思路
第一步找到m的位置，检查m所在的位置是否是第一个位置，不是则利用递归操作余下的链表，使得下次计算的m位置即是第一个位置。**相当于是使用递归消除特殊的可能性**
第二步从第一个节点进行反转。

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if(head == NULL || head->next == NULL) return head;
        ListNode *ret = NULL, *prev = NULL;
        ListNode *s = head;
        int cnt = 1;
        while(cnt < m)
        {
            if(cnt++ == 1) ret = s;
            prev = s;
            s = s->next;
        }
        if(ret != NULL) 
        {
            prev->next = reverseBetween(s, 1, n - m + 1);
            return ret;
        }
        ListNode *first = s, *cur = s, *next = s;
        prev = s;
        while(cnt <= n)
        {
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
            cnt++;
        }
        ret = prev;
        first->next = cur;
        return ret;
    }
};
```