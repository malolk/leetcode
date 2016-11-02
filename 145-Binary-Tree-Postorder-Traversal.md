## NO.145 Binary Tree Postorder Traversal

ven a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
```
return [3,2,1].

Note: Recursive solution is trivial, could you do it iteratively?

## 思路
1. one-stack

- 首先获得最左的节点，对于stack中的每个节点首先查看是否有右节点，如果有右节点则将其右子树的所有靠左边节点压入stack中，如果没有右节点，则先将当前节点的值保存到返回结果中，并检查当前节点是否是栈顶中节点的右子节点，如果是说明栈顶节点的左右子树都已经遍历完毕因此需要将其弹出stack，然后不断进行此项检查，直到stack为空或者栈顶节点的右节点不为当前节点。

- 另外一个思路是，先将靠左边的节点压入栈中。然后从最左边的节点即栈顶节点开始处理，使用pre记录上次访问的节点，只有当当前节点的右子节点为空或者当前节点的右子节点为pre(表示上一个访问的节点为当前节点的右子节点，说明当前节点的左右子树都已经访问完毕)时，访问当前节点并将其从栈中弹出来。否则将当前节点的右子节点压入栈中。循环结束的条件是栈中的元素为空或者root指针不为空。

2. two-stack
使用两个stack分别存储实际操作的节点和path节点。
首先将root节点存入stack s中。
然后每次先检查path的栈顶节点是否与s的栈顶节点是否相同，如果相同则将其存入返回结果中，并将该节点从两个栈中弹出。
如果不相同则将其压入path栈中，并将该节点的**右边节点和左边节点**依次压入s中, 注意stack本身是到过来的，所以得先压入右边的节点再压入左边的节点。


## 实现
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(!root) return {};
        stack<TreeNode*> stk;
        vector<int> ret;
        while(true)
        {
            while(root)
            {
                stk.push(root);
                root = root->left;  
            }
            TreeNode* temp = stk.top();
            if(temp->right) root = temp->right;
            else 
            {
                stk.pop();
                ret.push_back(temp->val);
                while(!stk.empty() && temp == stk.top()->right)
                {
                    temp = stk.top();
                    stk.pop();
                    ret.push_back(temp->val);
                }   
                if(stk.empty()) break;
            }   
        }   
        return ret;
    }
};

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * }
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(!root) return {};
        stack<TreeNode*> stk;
        vector<int> ret;
        TreeNode* pre = NULL;
        while(root || !stk.empty())
        {
            if(root)
            {
                stk.push(root);
                root = root->left;
            }
            else
            {
                TreeNode* temp = stk.top();
                if(!temp->right || temp->right == pre)
                {
                    stk.pop();
                    ret.push_back(temp->val);
                    pre = temp;
                }
                else root = temp->right;
            }
        }   
        return ret;
    }
};
```

```
two-stack
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * }
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(!root) return {};
        stack<TreeNode*> s, path;
        vector<int> ret;
        s.push(root);
        while(!s.empty())
        {
            root = s.top();
            if(!path.empty() && path.top() == s.top())
            {
                ret.push_back(root->val);
                s.pop();
                path.pop();
            }
            else
            {
                path.push(root);
                if(root->right) s.push(root->right);
                if(root->left) s.push(root->left);
            }
        }
        return ret;
    }
};
```