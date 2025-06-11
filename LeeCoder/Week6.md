### [100. 相同的树](https://leetcode.cn/problems/same-tree/)
- DFS+树的遍历
```cpp
// DFS
class Solution {  
public:  
    bool isSameTree(TreeNode* p, TreeNode* q) {  
        if(p == nullptr && q == nullptr)   // 同时进行递归
            return true;  
        else if(p == nullptr || q == nullptr)  
            return false;  
        else if(p->val != q->val)  
            return false;  
        else  
            return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);  
    }  
};

// 序列化，转化成数组进行比较
class Solution {  
public:  
    bool isSameTree(TreeNode* p, TreeNode* q) {  
        vector<int> Q, P;  
        recur(p, P);  
        recur(q, Q);  
        if(Q.size() != P.size())  
            return false;  
        for(int i = 0; i < Q.size(); i++){  
            if(Q[i] != P[i])  
                return false;  
        }  
        return true;  
    }  
  
    void recur(TreeNode *root, vector<int> &res){  
        if(root == nullptr){     // 前序遍历
            res.push_back(INT_MAX);  
            return;  
        }  
        res.push_back(root->val);  
        recur(root->left, res);  
        recur(root->right, res);  
    }  
};
```
>[!note]
>前序遍历可以唯一确认树的结构

### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)
- 树+递归
```cpp
// True
class Solution {  
public:  
    bool isSymmetric(TreeNode* root) {  
        bool res = func(root->left, root->right);  
        return res;  
    }  
  
    bool func(TreeNode *left, TreeNode *right){  
        if(left == nullptr && right == nullptr)  // 左子树先遍历右边
            return true;                         // 右子树先遍历左边
        if(left == nullptr || right == nullptr)  
            return false;  
        if(left->val != right->val)  
            return false;  
        return func(left->right, right->left) && func(left->left, right->right);  
    }  
};
// False
class Solution {  
public:  
    bool isSymmetric(TreeNode* root) {  
        bool res = func(root->left, root->right);  
        return res;  
    }  
  
    bool func(TreeNode *left, TreeNode *right){  
        if(left == nullptr && right == nullptr)  
            return true;  
        if(left == nullptr || right == nullptr)  
            return false;  
        if(left->val != right->val)  
            return false;  
        func(left->right, right->left);  
        func(left->left, right->right);  
        return true;    // 任何情况都是true,无结合左右遍历情况
    }  
};
```

### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
- 树+DFS
```cpp
// False
class Solution {
public:
    int maxDepth(TreeNode* root) {
        int res = 0;
        int M = INT_MIN;
        recur(root, res, M);
        return res;   // 应该返回最大值
    }

    void recur(TreeNode *root, int &res, int M){  // M为值传递
        if(root == nullptr){                      // 每次调用都是0
            M = max(M, res);
            return;
        }
        res++;
        recur(root->left, res, M);
        recur(root->right, res, M);
        res--;
    }
};

// True
class Solution {  
public:  
    int maxDepth(TreeNode* root) {  
        int res = 0;  
        int M = INT_MIN;  
        recur(root, res, M);  
        return M;  
    }  
  
    void recur(TreeNode *root, int &res, int &M){  
        if(root == nullptr){  
            M = max(M, res);  
            return;  
        }  
        res++;  
        recur(root->left, res, M);  
        recur(root->right, res, M);  
        res--;  
    }  
};

// Better
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int leftDepth = maxDepth(root->left);    // 左子树深度
        int rightDepth = maxDepth(root->right); // 右子树深度
        return max(leftDepth, rightDepth) + 1;  // 当前深度 = max(左, 右) + 1
    }
};
```

### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)
- 树+DFS
```cpp
class Solution {  
public:  
    int minDepth(TreeNode* root) {  
        if(root == nullptr)     // 左右子树可能为0
            return 0;  
        if(root->left == nullptr)  
            return 1 + minDepth(root->right);  
        if(root->right == nullptr)  
            return 1 + minDepth(root->left);  
        int leftDep = minDepth(root->left);  
        int rightDep = minDepth(root->right);  
        return min(leftDep, rightDep) + 1;  
    }  
};
```