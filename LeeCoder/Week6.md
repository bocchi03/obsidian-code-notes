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
        if(root == nullptr)     // 左右子树可能为nullptr
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

### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
- KMP+字符串匹配
```cpp
class Solution {  
public:  
    int strStr(string haystack, string needle) {  
        vector<int> PM = getNext(needle);  
        int i = 0, j = 0;  
        while (i < haystack.size() && j < needle.size()) {  
            if (haystack[i] == needle[j]) {  
                j++;  
                i++;  
            } else {  
                if (j != 0) {  
                    j = PM[j - 1];  
                } else {  
                    i++;  
                }  
            }  
        }  
        return j == needle.size() ? i - j : -1;  
    }  
  
    vector<int> getNext(string m) {    // 得到next数组
        vector<int> PM(m.size());  
        PM[0] = 0;  
        int i = 1, j = 0;  
        while (i < m.size()) {  
            if (m[i] == m[j]) {  
                j++;  
                PM[i] = j;  
                i++;  
            } else {  
                if (j != 0) {  
                    j = PM[j - 1];  
                } else {  
                    PM[i] = 0;  
                    i++;  
                }  
            }  
        }  
        return PM;  
    }  
};
```

### [112. 路径总和](https://leetcode.cn/problems/path-sum/)
- 树+递归
```cpp
// inferior
class Solution {  
public:  
    bool hasPathSum(TreeNode* root, int targetSum) {  
        if (root == nullptr)  
            return false;  
        bool res = false;  
        recur(root, targetSum, res);  
        return res;  
    }  
  
    void recur(TreeNode *root, int targetSum, bool &res) {  
        if (root->left == nullptr && root->right == nullptr) {   //终止条件
            if (targetSum == root->val)                  // 为叶子节点且和为节点值
                res = true;  
            return;  
        }  
        if (root->left != nullptr)  
            recur(root->left, targetSum - root->val, res);  // 递归左子树
  
        if (root->right != nullptr) {  
            recur(root->right, targetSum - root->val, res);  // 递归右子树
        }  
    }  
};

// True
class Solution {  
public:  
    bool hasPathSum(TreeNode* root, int targetSum) {  
        if (root == nullptr)  
            return false;  
        if (root->left == nullptr && root->right == nullptr)  
            return root->val == targetSum;  // 当前为叶子节点，查看是否匹配
            // 左节点或者右节点有匹配成功则为True
        return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);  
    }  
};
```
>[!warning]
>注意路径为到叶子节点个数-> 所以需要遍历到叶子节点


### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)
- 树+递归
```cpp
// inferiror
class Solution {  
public:  
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {  
        if (root1 == nullptr && root2 == nullptr)  // 两个节点都为空
            return nullptr;  
        if (root1 != nullptr && root2 != nullptr) {  // 非空
            root1->val += root2->val;  
        }else if (root1 == nullptr)   // 左树节点为空直接返回右节点
            return root2;  
        else if (root2 == nullptr)  // 返回左节点
            return root1;  
        root1->left = mergeTrees(root1->left, root2->left); // 遍历构造 
        root1->right = mergeTrees(root1->right, root2->right);  
        return root1;  
    }  
};

// True
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        // 如果一个节点为空，直接返回另一个节点
        if (root1 == nullptr) return root2;
        if (root2 == nullptr) return root1;
        
        // 合并当前节点值
        root1->val += root2->val;
        
        // 递归合并左右子树
        root1->left = mergeTrees(root1->left, root2->left);
        root1->right = mergeTrees(root1->right, root2->right);
        
        return root1;
    }
};
```

### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
- 树+递归
```cpp
// inferiror
class Solution {  
public:  
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {  
        vector<TreeNode*> P, Q, temp;  
        unordered_map<int, int> dire;   //在路径数组中找找最近的共同点 
        path(root, p->val, P, temp);  
        temp.clear();  
        path(root, q->val, Q, temp);  
        int l1 = 0, l2 = P.size() - 1;  
        while (l1 < Q.size()) {  
            dire[Q[l1]->val]++;  
            l1++;  
        }  
        while (l2 > 0) {  
            if (dire.count(P[l2]->val))  
                return P[l2];  
            l2--;  
        }  
        return P[l2];  
    }  
  
    void path(TreeNode *root, int target, vector<TreeNode*> &res, vector<TreeNode*> &temp) {  
        if (root == nullptr)  // 将树到节点的路径转化成数组
            return;           
        temp.push_back(root);  
        if (root->val == target) {  
            res = temp;  
            return;  
        }  
        path(root->left, target, res, temp);  
        path(root->right, target, res, temp);  
        temp.pop_back();  
    }  
};

// True
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // 基准情况：如果 root 为空，或者 root 就是 p 或 q，直接返回 root
    if (!root || root == p || root == q) 
	    return root;
    
    // 递归在左子树中查找 p 或 q
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    
    // 递归在右子树中查找 p 或 q
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    // 如果 left 和 right 都不为空，说明 p 和 q 分别在当前节点的左右子树中，当前节点就是 LCA
    if (left && right) 
	    return root;
    
    // 否则，返回非空的一边（即 p 或 q 所在的分支）
    return left ? left : right;
}

```

### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
- 二叉树的遍历
```cpp
class Solution {  
public:  
    vector<vector<int>> levelOrder(TreeNode* root) {  
        vector<vector<int>> res;  
        if (root == nullptr)  
            return res;  
        queue<TreeNode*> que;  
        que.push(root);  
        int tag = 0;  
        while (!que.empty()) {  
            vector<int> temp;  
            for (int i = que.size(); i > 0; i--) {  
                TreeNode *te = que.front();  // 通过分析队列的大小进行数组分层
                que.pop();  
                temp.push_back(te->val);  
                if (te->left != nullptr)  
                    que.push(te->left);  
                if (te->right != nullptr)  
                    que.push(te->right);  
            }  
            res.push_back(temp);  
        }  
        return res;  
    }  
};
```
>[!note]
>注意到当前队列的大小为下个数组元素的大小
