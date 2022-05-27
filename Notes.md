## 双指针



## 快慢指针

#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

给定一个链表，有环把环的入口找出来，无环返回nullptr。

**Method1**

Traverse the linked list and store the address of node, if we meet node that is already stored, then this is the enter of cycle. Since the address is unique, we can use set to store.

```c++
ListNode* detectCycle(ListNode* head) {
	unordered_set<ListNode*> address;
    
    ListNode* curr = head;
    
    while (curr != nullptr) {
        // check if curr exists
        if (address.find(curr) != address.end()) {
            return curr;
        }
        // store curr
        address.insert(curr);
        curr = curr->next;
    }
    
    return nullptr;
}
```

**Method2**

Fast and slow pointer start at the head of linked list.

Fast pointer moves 2 steps and slow pointer moves 1 step each time. If there is cycle in linked list, fast and slow pointer will meet in cycle.

当发现 slow 与 fast 相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow 每次向后移动一个位置。最终，它们会在入环点相遇。

```c++
ListNode* detectCycle(ListNode* head) {
    ListNode* fast = head;
    ListNode* slow = head;
    
    while (fast != nullptr) {
        if (fast->next == nullptr) {
            return nullptr;
        }
        slow = slow->next;
        fast = fast->next->next;
        
        // there is a cycle
        if (fast == slow) {
            ListNode* ptr = head;
            while (ptr != slow) {
                ptr = ptr->next;
                slow = slow->next;             
            }
            return ptr; // or slow
        }
    }
    
    return nullptr;
}
```



## 搜索算法（BFS，DFS）

单源搜索，从单个节点向外搜索

#### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)











