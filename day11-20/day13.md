# 第六章 二叉树part01

## 前序遍历

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    void travel(TreeNode* cur, vector<int>& vec) {
        if (cur == nullptr)
            return;
        vec.push_back(cur->val);
        travel(cur->left, vec);
        travel(cur->right, vec);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        travel(root, result);
        return result;
    }
};
```
