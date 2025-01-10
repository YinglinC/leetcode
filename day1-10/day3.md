# 第二章 链表part01

203.移除链表元素
707.设计链表
206.反转链表

[关于链表，你该了解这些！](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E9%93%BE%E8%A1%A8%E7%9A%84%E7%B1%BB%E5%9E%8B)

数组是在内存中是连续分布的，但是链表在内存中可不是连续分布的。
链表定义

```cpp
struct ListNode{
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr){}
};
```

python实现

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

## 203.移除链表元素

第一想法：虚拟头节点，需要一个临时tmp，来存放要删除的节点

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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(-1);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while(cur->next != nullptr){
            if (cur->next->val == val){
                // delete
                ListNode* tmp;
                tmp = cur->next;
                cur->next = cur->next->next;
            }else{
                cur = cur->next;
            }
            
        }
        return dummyHead->next;
    }
};
```

问题：cpp语法不太熟悉，初始化变量，初始化指针类型的数据，初始化class，struct需要注意

## 707.设计链表

第一想法，需要构建ListNode类，

```cpp
class MyLinkedList {
public:
struct ListNode{
        int val;
        ListNode* next;
        ListNode(int x=0, ListNode* next=nullptr) : val(x), next(next) {}
    };
private:
    int size;
    ListNode* dumyHead = new ListNode();

public:    
    MyLinkedList() {
        dumyHead->val = 0;
        dumyHead->next = nullptr;
        size = 0;
    }
    
    int get(int index) {
        if(index < 0 || index >= size){
            return -1;
        }

        ListNode* cur = dumyHead;
        // i++ 与 ++i
        for(int i = 0; i < index; ++i){
            cur = cur->next;
        }
        return cur->next->val;
    }
    
    void addAtHead(int val) {
        ListNode* head = new ListNode();
        head->val = val;
        head->next = dumyHead->next;

        this->dumyHead->next = head;
        size++;
    }
    
    void addAtTail(int val) {
        ListNode* cur = dumyHead;
        while(cur->next != nullptr){
            cur = cur->next;
        }
        ListNode* tail = new ListNode();
        tail->val = val;
        cur->next = tail;
        size++;
    }
    
    void addAtIndex(int index, int val) {
        if(index > size || index < 0){
            return;
        }
        if(index == size){
            addAtTail(val);
            return;
        }
        if(index == 0){
            addAtHead(val);
            return;
        }

        ListNode* newNode = new ListNode(val);
        ListNode* cur = dumyHead;
        for(int i = 0; i < index; i++){
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        size++;
        return;
    }
    
    void deleteAtIndex(int index) {
        if(index < 0 || index >= size){
            return;
        }
        ListNode* cur = dumyHead;
        for(int i = 0; i < index; i++){
            cur = cur->next;
        }
        ListNode* tmp;
        tmp = cur->next;
        cur->next = tmp->next;
        size--;
        return;
    }
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */

```

问题：

- cpp语法加强加强

- 你在创建新节点时，使用了`ListNode* head = new ListNode();`，但构造函数已经提供了默认值，可以直接使用`ListNode* head = new ListNode(val, dummyHead->next);`，使代码更简洁。
dummyHead 的初始化：

- 你在构造函数中初始化 `dummyHead->val = 0`，但 `dummyHead` 的值不会被使用，可以省略。

- size 的初始化可以放在成员变量声明时直接初始化，使代码更简洁。

## 206.反转链表

定义连个指针，一步一步反转链表

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;  // 前一个节点
        ListNode* curr = head;     // 当前节点

        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;  // 保存下一个节点
            curr->next = prev;                // 反转当前节点的指针
            prev = curr;                      // 移动prev到当前节点
            curr = nextTemp;                  // 移动curr到下一个节点
        }

        return prev;  // prev现在是新的头节点
    }
};
```

问题
错误代码⬇⬇⬇⬇⬇⬇⬇⬇

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* left = head;
        ListNode* right = head->next;
        ListNode* tmp = head->next;
        while(tmp->next != nullptr){
            right->next = left;

            left = right;
            right = tmp->next;
            tmp = right;

        }
        return right;
    }
};

```

- 空链表处理：如果链表为空（head为nullptr），你的代码会直接访问head->next，这将导致空指针异常。你需要先检查head是否为空。

- 循环条件：你的循环条件是tmp->next != nullptr，这意味着循环会在tmp指向最后一个节点时停止，导致最后一个节点没有被正确处理。

- 指针更新：在循环中，你更新left和right指针的方式有问题。你需要在每次迭代中正确地更新这些指针。

- 返回值：你的代码返回的是right，但在循环结束时，right指向的是最后一个节点，而不是反转后的链表的头节点。
