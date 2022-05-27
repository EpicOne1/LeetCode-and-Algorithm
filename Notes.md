## 双指针



## 快慢指针

![image-20220527175032294](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527175032294.png)



### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

##### Hash Table

```c++
unordered_set<ListNode*> container;
// find 
container.find(curr) == container.end();

// count
container.count(curr) 
```

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

### 邻居

搜索算法中经常会出现相邻节点，上下左右

```c++
vector<int> direction = {-1, 0, 1, 0, -1};

vector<vector<int>>& grid = {{...}}
int m = grid.size();
int n = grid[0].size();

// (i, j)'s neighbors NESW
for (int k = 0; k < 4; k++) {
    neigh_i = i + direction[k];
    neigh_j = j + direction[k+1];
    // valid neighbor
    if (neigh_x >= 0 && neigh_x < m && neigh_y >= 0 && neigh_y < n) {
        ...
    }
}
```

### 数据结构 （栈，队列）

节点坐标

`pair<int,int> p (i,j)`

`p = make_pair(i, j)`

`p.first, p.second`



### DFS

Stack 用来储存节点的坐标

`s.top(), s.pop(), s.push()`

```c++
stack<pair<int,int>> s;

s.push({i, j});
while (!s.empty()) {
    auto coordinate = s.top();
	s.pop();
    
    // neighbors of (i,j)
    for (neighs : (i,j)) {
        if (visited[neighs] == false) {
            visited[neighs] = true;
            dfs(neighs);
        }
        // or use stack
        if (visited[neighs] == false) {
            visited[neighs] = true;
            s.push(neighs);
        }
    }
}

```



### BFS

Queue 用来储存节点的坐标

`q.front(), q.pop(), q.push()`

```c++
queue<pair<int,int>> q;

q.push({i,j});
while (!q.empty()) {
    auto coordinate = q.front();
    q.pop();
    
    // neighbors of (i,j)
    for (neighs : (i,j)) {
        if (visited[neighs] == false) {
            visited[neighs] = true;
            s.push(neighs);
        }
    }
}
```



### 访问矩阵

记录访问过的节点，以防止循环访问

### 单源搜索

从单个节点向外搜索

#### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)

把每个岛屿的面积都求出来，再选取最大的岛屿

```c++
int maxAreaOfIsland(vector<vector<int>>& grid) {
	int m = grid
}
```



### 多源搜索

从多个节点同时向外扩张

#### [934. 最短的桥](https://leetcode.cn/problems/shortest-bridge/)

#### [542. 01 矩阵](https://leetcode.cn/problems/01-matrix/)



## 链表

递归或者迭代

经常使用哑结点

```c++
ListNode* prevHead = new ListNode();
prevHead->next = head;
```

#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

$1 \to 2 \to 3 \to \phi$

$3 \to 2 \to 1 \to \phi$

##### **递归**

![image-20220527154443613](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527154443613.png)

注意如何获取$n_{k+1} = n_k.next$ 的，不需要通过遍历 $reverseList(n_{k+1})$ 

```c++
ListNode* reverseList(ListNode* head) {
    // list with 0 or 1 element
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    
    ListNode* newHead = reverseList(head->next);
    head->next->next = head;
    head->next = nullptr;
    
    return newHead;
}
```

##### **遍历**

![image-20220527155259534](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527155259534.png)

注意，这里的prev并不是哑结点，而只是用来储存curr的前一个节点而已

```c++
ListNode* reverseList(ListNode* head) {
    // at the beginning, curr's previous node is nullptr
    ListNode* prev = nullptr;
    ListNode* curr = head;
    
    while (curr != nullptr) {
        ListNode* next = curr->next;
        curr->next = prev;
        // move to next position 
        prev = curr;
        curr = next;
    }
    
    return prev;
}
```

#### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)



##### 递归

```c++
ListNode* swapPairs(ListNode* head) {
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    
    ListNode* newHead = swapPairs(head->next->next);
    ListNode* next = head->next;
    // change head->next->newHead to next->head->newHead
    next->next = head;
    head->next = newHead;
    
    return next;
}
```



##### 迭代

![image-20220527161958762](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527161958762.png)

```c++
ListNode* swapPairs(ListNode* head) {
    ListNode* prevHead = new ListNode();
    prevHead->next = head;
    
    ListNode* temp = prevHead;
    while(temp->next != nullptr && temp->next->next != nullptr) {
        ListNode* node1 = temp->next;
        ListNode* node2 = temp->next->next;
        // change temp->node1->node2->next to temp->node2->node1->next
        temp->next = node2;
        node1->next = node2->next;
        node2->next = node1;
        
        temp = node1;
    }
    
    return prevHead->next;
}
```

#### [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

Traversal 

```c++
ListNode* deleteDuplicates(ListNode* head) {
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    
    ListNode* curr = head;
    while(curr->next != nullptr) {
        // curr->next->nextnext
        if (curr->val == curr->next->val) {
            ListNode* temp = curr->next;	// store for deletion
            curr->next = curr->next->next;
            delete temp;
        }
        else {
            curr = curr->next;
        }
    }
    
    return head;
}
```



#### [328. 奇偶链表](https://leetcode.cn/problems/odd-even-linked-list/)

原始链表的头节点 head 也是奇数链表的头节点以及结果链表的头节点，head 的后一个节点是偶数链表的头节点。令 evenHead = head.next，则 evenHead 是偶数链表的头节点。

![image-20220527184804837](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527184804837.png)

```c++
ListNode* oddEvenList(ListNode* head) {
    if (head == nullptr) {
        return head;
    }
    
    ListNode* oddHead = head;
    ListNode* evenHead = head->next;
    
    ListNode* odd = oddHead;
    ListNode* even = evenHead;
    
    while(even != nullptr && even->next != nullptr) {
        odd->next = even->next;
        odd = odd->next;
        even->next = odd->next;	// odd is updated
        even = even->next;
    }
    
    // now odd is the last element of odd list
    odd->next = evenHead;
    
    return oddHead;
}
```

#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

为了方便操作，应该取得被删节点之前的节点

For example, $1 \to 2 \to 3 \to 4 \to \phi$ 

say we need to delete 3, we need to know `prev_delete = 2`

```c++
ListNode* temp = prev_delete->next;	// store node need to delete
prev_delete->next = prev_delete->next->next;
delete temp;
```























