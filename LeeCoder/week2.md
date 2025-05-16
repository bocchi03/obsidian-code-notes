### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)
- 迭代+双指针
```cpp
ListNode* cur = head;
ListNode* pre = nullptr;  // 头节点指向空节点
while(cur){
    ListNode* next = cur->next;
    cur->next = pre;
    pre = cur;
    cur = next;
    }
return pre;
```
>[!note]
>注意第一步 ``pre = nullptr``

- 递归
```cpp
if(cur == nullptr)
    return pre;
ListNode* res = recur(cur->next, cur);
cur->next = pre;
return res;
```
>[!note]
>带着pre一起递归


### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)
- 快慢指针
```cpp
while(fast && fast->next){  
    fast = fast->next->next;  
    slow = slow->next;  
    if(fast == slow)  
        return true;  
}  
return false;
```
>[!warning]
>空指针引用``fast->next``不能为空，否则``fast->next->next``为空指针引用

### [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)
- 快慢指针+链表
```cpp
if(!head || !head->next)
    return head;
ListNode *fast = head->next;  // 快慢指针
ListNode *slow = head;
while(fast){
    if(fast->val == slow->val){
        slow->next = fast->next;
        fast = slow->next;     // 保证fast在slow前面
    }else{
        slow = slow->next;
        fast = fast->next;
    }
}
    return head;
}
```
### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)
- 双指针
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    // 递归终止条件
    // l2还有剩余
    if (l1 == NULL) {
        return l2;
    }
    // l1还有剩余
    if (l2 == NULL) {
        return l1;
    }
    // 找出小的值，然后剩下的继续递归比较
    if (l1->val <= l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    }
    l2->next = mergeTwoLists(l1, l2->next);
    return l2;
    }
```


### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)
```cpp
// False
ListNode *L1 = headA;  
ListNode *L2 = headB;  
int cnt = 0;  
while(L1 && L2){  
    if(!L1->next && cnt == 0){  
        L1->next = headB;  // 修改了链表物理结构，无交点会形成环
        cnt++;             
    }  
    if(!L2->next)  
        L2->next = headA;  //同理
    if(L1 == L2)  
        return L1;  
    L1 = L1->next;  
    L2 = L2->next;  
}  
return NULL;

// True
ListNode *L1 = headA;  
ListNode *L2 = headB;  
int cnt = 0;  
while(L1 && L2){  
    if(L1 == L2)  
        return L1;  
    L1 = L1->next;   // 只改变逻辑结构，当为空时才改变
    L2 = L2->next;  
    if(!L1 && !cnt){  
        L1 = headB;  
        cnt++;  
    }  
    if(!L2)  
        L2 = headA;  
}  
return NULL;
```
>[!warning]
>注意链表结构（可能成环），遍历链表时尽量不改变链表物理结构

### [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)
- 双指针
```cpp
//False
ListNode *cur = head;  
ListNode *pre = head;  
while(cur){  
    if(cur->val == val){  
        pre->next = cur->next;  
        cur = cur->next;  
    }else{  
        pre = cur;  
        cur = cur->next;  
    }  
}  
return head;    // 头节点一直没动，pre只能改变head结构无法移动或删除

//True
 while(head && head->val == val){  // 处理符合条件的头节点
        head = head->next;  
    }  
    if(!head)  
        return nullptr;     //处理空节点
    ListNode *cur = head;  
    ListNode *pre = head;  
    while(cur){  
        if(cur->val == val){  
            pre->next = cur->next;  
            cur = cur->next;  
        }else{  
            pre = cur;  
            cur = cur->next;  
        }  
    }  
    return head;  
}
```
>[!warning]
>指针相等只是指向同一节点，无法更改原指针

### [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)
- 双指针
```cpp
// False
vector<char> temp;  
    for(auto c : s){    // 判断是否为字母数字
        if(c >= 65 && c < 91)  
            temp.push_back(c + 32);  
        if(c >= 97 && c < 123 )  
            temp.push_back(c);  
        if(c >= 48 && c <= 57)  
            temp.push_back(c);  
    }  
    if(temp.empty())  
        return true;  
    int left = 0, right = temp.size() - 1;  
    while(left != right){  
        if(temp[left] != temp[right])  
            return false;  
        left++;  
        right--;  
    }  
    return true;  
}

// True
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0, right = s.size() - 1;
        while(left < right){
            while(left < right && !isalnum(s[left])) left++;
            while(left < right && !isalnum(s[right])) right--;
            if(tolower(s[left]) != tolower(s[right])) 
                return false;
            left++;
            right--;
        }
        return true;
    }
};
```
>[!tip]
>- 双指针判断边界 (left < right)
>- isalnum()判断是否为字母数字（isdigit()判断数字， isalpha()判断字母）
>- tolowe(), toupper()



>[!note]
>- 快慢指针：解决链表相交，倒数第k个数
>- 双指针边界判断用  (left < right)

 