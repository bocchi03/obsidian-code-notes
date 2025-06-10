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