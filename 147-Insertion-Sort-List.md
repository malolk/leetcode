## NO.147. Insertion Sort List

Sort a linked list using insertion sort.

## 思路
维持另一个链表指针，这个链表指针所指链表都是已经排好序，所以只需要将head的指针中的节点插入到这个链表指针中即可。

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
    ListNode* insertionSortList(ListNode* head) {
        if(!head || !head->next)   return head;
        ListNode* ret = head;
        ListNode* nextNode = head->next;
        ret->next = NULL;
        head = nextNode;
        ListNode* prev = NULL;
        ListNode* ptr = NULL;
        while(head)
        {
            nextNode = head->next;
            if(head->val <= ret->val)
            {
                head->next = ret;
                ret = head;
            }
            else
            {
                prev = ret, ptr = ret->next;
                while(ptr)
                {
                    if(ptr->val >= head->val) break;
                    prev = ptr;
                    ptr = ptr->next;
                }
                prev->next = head;
                head->next = ptr;
            }
            head = nextNode;
        }
        return ret;
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
    ListNode* insertionSortList(ListNode* head) {
        if(!head || !head->next)   return head;
        ListNode* ret = new struct ListNode(INT_MIN);
        ListNode* nextNode = NULL;
        ListNode* prev = NULL;
        ListNode* ptr = NULL;
        while(head)
        {
            nextNode = head->next;
            prev = ret, ptr = ret->next;
            while(ptr)
            {
                if(ptr->val >= head->val) break;
                prev = ptr;
                ptr = ptr->next;
            }
            prev->next = head;
            head->next = ptr;
            head = nextNode;
        }
        ListNode* temp = ret;
        ret = ret->next;
        delete temp;
        return ret;
    }
};
```