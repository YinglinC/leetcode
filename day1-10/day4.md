# 第二章 链表part02

● 24. 两两交换链表中的节点
● 19.删除链表的倒数第N个节点
● 面试题 02.07. 链表相交
● 142.环形链表II
● 总结

## 24. 两两交换链表中的节点

第一想法：奇数节点怎么办？怎么两两交换

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(-1, head);
        ListNode* cur = dummyHead;
        
        while(cur->next!= nullptr && cur->next->next != nullptr){
            ListNode* ptr1 = cur->next;
            ListNode* ptr2 = cur->next->next;

            ListNode* tmp = ptr2->next;
            cur->next = ptr2;
            ptr2->next = ptr1;
            ptr1->next = tmp;

            cur = cur->next->next;
        }
        return dummyHead->next;
    }
};

```

- 处理2个next节点，都要判读是否为nullptr
- 指针更新注意

## 19.删除链表的倒数第N个节点

第一想法：模拟，但是是倒数n，双指针

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0, head);
        ListNode* slowptr = dummyHead;
        ListNode* fastptr = dummyHead;
        while(n--){
            fastptr = fastptr->next;
        } 
        while(fastptr->next != nullptr){
            slowptr = slowptr->next;
            fastptr = fastptr->next;
        }
        ListNode* tmp = slowptr->next;
        slowptr->next = tmp->next;
        return dummyHead->next;
        
    }
};
```

## 面试题 02.07. 链表相交

第一想法：如果暴力，复杂度O（m*n），遍历2个链表

看提示后，先求出diff，长的链表先移动，然后一起移动

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int get_size(ListNode* dummyHead){
        ListNode* cur = dummyHead;
        int size = 0;
        while(cur->next != nullptr){
            size++;
            cur = cur->next;
        }
        return size;
    }
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int sizeA = 0;
        int sizeB = 0;
        ListNode* dummyHeadA = new ListNode(0);
        dummyHeadA->next = headA;
        
        ListNode* dummyHeadB = new ListNode(0);
        dummyHeadB->next = headB;
        sizeA = get_size(dummyHeadA);
        sizeB = get_size(dummyHeadB);

        if(sizeA < sizeB){
            ListNode* tmp = dummyHeadA;
            dummyHeadA = dummyHeadB;
            dummyHeadB = tmp;

            int temp = sizeA;
            sizeA = sizeB;
            sizeB = temp;
        }

        int diff = sizeA - sizeB;
        ListNode* curA = dummyHeadA;
        while(diff--){
            curA = curA->next;
        }

        ListNode* curB = dummyHeadB;
        while(curA != curB){
            curA = curA->next;
            curB = curB->next;
        }
        return curA;
        
    }
};
```

问题：求链表长度可以不折磨复杂，交换有`swap()`函数

## ● 142.环形链表II

第一想法，双指针，一个快一个满，但是相差多少个节点不知道

看提示：指针不是常熟间隔，而是速度倍速间隔，如何确定交点？

**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点。**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;

        ListNode* slow = dummyHead;
        ListNode* fast = dummyHead;

        while(slow->next != nullptr && fast->next != nullptr &&fast->next->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast){
                ListNode* start = dummyHead;
                ListNode* cur = slow;
                while(start != cur){
                    start = start->next;
                    cur = cur->next;
                }
                return cur;
            }
        }
        return NULL;

        
    }
};
```

更简洁的写法`while (fast != nullptr && fast->next != nullptr)`,原因

`slow->next != nullptr：`
这个条件检查slow的下一个节点是否为空。
实际上，slow的下一个节点是否为空并不影响快慢指针的核心逻辑，因为slow每次只移动一步，而fast每次移动两步。
这个条件是多余的，因为fast的移动已经隐含了对slow的检查。

`fast->next != nullptr：`
确保fast可以移动一步到fast->next。
同时，由于fast每次移动两步，fast->next->next的检查已经隐含在fast->next的检查中。

## 总结

对于链表的题目，大家最大的困惑可能就是 什么使用用虚拟头结点，什么时候不用虚拟头结点？

一般涉及到 增删改操作，用虚拟头结点都会方便很多， 如果只能查的话，用不用虚拟头结点都差不多
