### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)
- 迭代+双指针
```cpp
ListNode* cur = head;
ListNode* pre = nullptr;
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

### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

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