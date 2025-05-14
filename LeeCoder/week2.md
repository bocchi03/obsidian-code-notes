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