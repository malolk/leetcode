## NO.138 Copy List with Random Pointer
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

## 思路
1. 额外空间复杂度O(n), 时间复杂度O(n)
使用两个vector分别存储原来的和新的链表节点指针，并将原来的链表节点指针和其所在vector中的索引存放在map中。接着遍历一次vector，通过原来链表节点中random指针找到其对应的索引，从而为新的链表节点中的random指针附上对应的节点指针。

2. 额外空间复杂度O(1), 时间复杂度O(n)
第一次遍历对元链表中的每个节点进行复制新节点并将新节点连在原节点后面。
第二次遍历根据原节点中的random指针找到新节点的random指针所指节点位置。
第三次将原节点和新节点分开，返回新的链表。

## 实现
```
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if(!head) return NULL;
        vector<RandomListNode*> nodes;
        vector<RandomListNode*> newNodes;
        unordered_map<RandomListNode*, int> ptr2Index;
        while(head)
        {
            nodes.push_back(head);
            ptr2Index[head] = nodes.size() - 1;
            RandomListNode* node = new struct RandomListNode(head->label);
            newNodes.push_back(node);
            head = head->next;
        }
        int size = nodes.size();
        for(int i = 0; i < size; ++i)
        {
            if(i != size - 1) newNodes[i]->next = newNodes[i + 1];
            RandomListNode* temp = nodes[i]->random;
            if(!temp) continue;
            newNodes[i]->random = newNodes[ptr2Index[temp]];
        }
        return newNodes[0];
    }
};
```

```

/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if(!head) return NULL;
        RandomListNode *iter = head;
        while(iter)
        {
            RandomListNode *newNode = new RandomListNode(iter->label);
            newNode->next = iter->next;
            iter->next = newNode;
            iter = newNode->next;
        }
        RandomListNode *randPtr;
        iter = head;
        while(iter)
        {
            randPtr = iter->random;
            if(randPtr) iter->next->random = randPtr->next;
            iter = iter->next->next;
        }
        iter = head;
        RandomListNode *ret = iter->next, *prev = iter->next;
        iter->next = prev->next;
        prev->next = NULL;
        iter = iter->next;
        while(iter)
        {
            prev->next = iter->next;
            prev = prev->next;
            iter->next = prev->next;
            prev->next = NULL;
            iter = iter->next;
        }
        return ret;
    }
};
```