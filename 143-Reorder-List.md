## NO.143. Reorder List

Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.

## 思路
首先找到链表的中点， 使用slow-fast的方法，注意在while循环中使用fast && fast->next进行判断可以保证最后slow指针是偏向有半边的。最后slow的指针指向右半边的链表，然后对右半边的链表进行反向，将左半边的链表与反向后的右半边的链表重组即可得到最后的链表。

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
    void reorderList(ListNode* head) {
        if(!head || !head->next || !head->next->next) return;
        int size = 0;
        ListNode *ptr1 = head, *ptr2 = head, *prev = NULL;
        while(ptr2 && ptr2->next)
        {
            prev = ptr1;
            ptr1 = ptr1->next;
            ptr2 = ptr2->next->next;
        }
        prev->next = NULL;
        ListNode *reversePrev = NULL;
        while(ptr1)
        {
            ListNode *nextNode = ptr1->next;
            ptr1->next = reversePrev;
            reversePrev = ptr1;
            ptr1 = nextNode;
        }
        ptr1 = reversePrev;
        ListNode *ptr0 = head, *temp = NULL;
        while(ptr0 && ptr1)
        {
            temp = ptr0->next;
            ptr0->next = ptr1;
            ptr1 = ptr1->next;
            ptr0->next->next = temp;
            prev = ptr0->next;
            ptr0 = temp;
        }
        if(ptr1) prev->next = ptr1;
        return;
    }
};
```