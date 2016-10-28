## NO.117. Populating Next Right Pointers in Each Node II
Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

    You may only use constant extra space.

For example,
Given the following binary tree,

         1
       /  \
      2    3
     / \    \
    4   5    7

After calling your function, the tree should look like:

         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL

## 思路
使用每个节点中的next指针在每一层形成链表，然后使用此链表可以推出下一层的链表。

## 实现
```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
 class Solution{
  public:
    void connect(TreeLinkNode *root) {
        if(!root) return;
        TreeLinkNode *ptr = root;
        TreeLinkNode *start = NULL, *lastEnd = NULL;
        while(ptr)
        {
            if(ptr->left && ptr->right)
            {
                ptr->left->next = ptr->right;
                if(!lastEnd && !start) start = ptr->left;
                else lastEnd->next = ptr->left;
                lastEnd = ptr->right;
            }
            else if(ptr->left == NULL ^ ptr->right == NULL)
            {
                if(!lastEnd && !start) start = (ptr->left == NULL ? ptr->right : ptr->left);
                else lastEnd->next = (ptr->left == NULL ? ptr->right : ptr->left);
                lastEnd = (ptr->left == NULL ? ptr->right : ptr->left);
            }
            ptr = ptr->next;
            if(!ptr)
            {
                ptr = start;
                start = lastEnd = NULL;
            }
        }
    }
};
```