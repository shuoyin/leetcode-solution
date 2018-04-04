## [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)
#### Description
>Given a binary tree, find the maximum path sum.
>
>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.
>
>For example:
>Given the below binary tree,
>
>       1
>      / \
>     2   3
>Return 6.

#### Solution
后序遍历,返回值是终点为当前节点的最长路径长度,因为最长的路径不一定经过当前节点,所以还要引用传递进来一个max_path值来保存当前子树下的最长路径.
当前子树的最长路径是左子树的最长路径,右子树的最长路径,当前孤立节点的值,终点在左右孩子的较长路径经过当前节点的路径,这些值中的最大者.
```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int f(TreeNode *p, int &max_path){
        if(p->left==NULL && p->right==NULL){
            if(p->val>max_path) max_path=p->val;
            return p->val;
        }
        int l=INT_MIN,r=INT_MIN;
        if(p->left) l=f(p->left, max_path);
        if(p->right) r=f(p->right, max_path);
        if(p->val>max_path) max_path=p->val;
        if(p->left && p->val+l>max_path) max_path=p->val+l;
        if(p->right && p->val+r>max_path) max_path=p->val+r;
        if(p->left && p->right && p->val+l+r>max_path) max_path=p->val+r+l;
        return max(p->val, p->val+max(l,r));
    }
    int maxPathSum(TreeNode *root) {
        int max_path=INT_MIN;
        f(root, max_path);
        return max_path;
    }
};
```
