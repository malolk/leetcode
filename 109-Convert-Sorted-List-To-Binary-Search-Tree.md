## NO.109. Convert Sorted List to Binary Search Tree 

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

## 思路
1. 先将list转换为vector存储，再利用上一题二分查找的方法即可获得平衡二叉树。
2. 使用递归可以不使用额外空间，首先计算节点的个数是多少，然后每次递归时将节点数分为size/2、当前节点和余下的size - size/2 - 1节点，使用中序遍历的方法。

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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution
{
    public:
        TreeNode* impl(vector<int>& nums, int start, int end)
        {   
            if(start == end)
                return new TreeNode(nums[start]);
            else if(start > end)
                return NULL;    
            int root = (start + end)/2;
            TreeNode* rootPtr = new TreeNode(nums[root]);
            rootPtr->left = impl(nums, start, root - 1); 
            rootPtr->right = impl(nums, root + 1, end);
            return rootPtr;
        }   

        TreeNode* sortedListToBST(ListNode* head)
        {   
            vector<int> nums;
            while(head)
            {   
                nums.push_back(head->val);
                head = head->next;  
            }    
            return impl(nums, 0, nums.size() - 1);
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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution
{
    public:
        TreeNode* impl(ListNode** head, int size)
        {
            if(size == 0) return NULL;
            TreeNode* newNode = new TreeNode(0);
            newNode->left = impl(head, size / 2);
            newNode->val = (*head)->val;
            *head = (*head)->next;
            newNode->right = impl(head, size - size/2 - 1);
            return newNode;
        }
        TreeNode* sortedListToBST(ListNode* head)
        {   
            ListNode* tmp = head;
            int size = 0;
            while(tmp)
            {   
                ++size;
                tmp = tmp->next;  
            }
            return impl(&head, size);
        }   
};

```
