## NO.61 Rotate List

- Problem 
Given a list, rotate the list to the right by k places, where k is non-negative.

For example:
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.

- 思路

第一种，计算整个链表的长度，再次从头开始获得断点，然后修改头指针指向新的节点。
第二种，使用slow和fast指针，fast比slow提前k个位置，当fast指向最后一个节点时，从后往前算slow指向k+1节点，因此可以修改头指针。当k大于链表的长度时，需要将fast调整到开始，或者直接获得链表的长度，将k取长度的模再次进行遍历。

- 实现
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
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == NULL || k == 0) return head;
        int len = k;
        ListNode *slow = head, *fast = head;
        while(fast->next && len-- > 0)
            fast = fast->next;
        if(len != 0)
        {
            len = k - len + 1;
            k %= len;
            fast = head;
            while(fast->next && k-- > 0) fast = fast->next;
        }
        while(fast->next)
        {
            slow = slow->next;
            fast = fast->next;
        }
        fast->next = head;
        head = slow->next;
        slow->next = NULL;
        return head;
    }
};
```
