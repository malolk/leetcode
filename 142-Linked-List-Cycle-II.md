## NO.142 Linked List Cycle II
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:
Can you solve it without using extra space?

## 思路
假设从头指针到环路起始节点的距离为x，从环路起始节点到相遇节点的距离为m，环的长度为l。
首先判断链路中是否存在环，为了去除节点过少的无环链表的干扰导致无法通过while循环正常判断，在起始时判断head的下一个节点和下下个节点是否存在，如果不存在说明链表中肯定没有环。
使用步长为1的slow和步长为2的fast指针来遍历链表，当两个指针相遇时，存在等式`2*(x + m) = x + Nl + m`，得到`x+m = Nl`,说明x和m的长度之和是环的长度的n倍。当两指针相遇时，接着使用另一个指针temp与slow指针以步长为1的速度前进，当temp指针走了x时，slow指针应该也走了x，此时temp指针在环路起始节点处，由上推导可知slow指针也在环路起始节点处。因此，temp指针和slow指针一定会相遇，且相遇点是环路起始节点。

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
    ListNode *detectCycle(ListNode *head) {
        if(!head || !head->next || !head->next->next) return NULL;
        ListNode *slow = head, *fast = head;
        while(fast && fast->next && fast->next->next)
        {
            slow = slow->next;
            fast = fast->next->next;
            if(fast == slow) break;
        }
        if(fast != slow) return NULL;
        fast = head;
        while(fast != slow)
        {
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```
