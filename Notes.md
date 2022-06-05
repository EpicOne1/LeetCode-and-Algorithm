

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

#### 数据结构

```c++
// Definition for singly-linked list
struct ListNode {
     int val;
     ListNode *next;
     ListNode() : val(0), next(nullptr) {}
     ListNode(int x) : val(x), next(nullptr) {}
     ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

递归或者迭代

经常使用哑结点 (**记得最后要删除**)

```c++
ListNode* dummyHead = new ListNode();
dummyHead->next = head;
```

记得删除不需要的结点（释放内存）

#### [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

```c++
ListNode* removeElements(ListNode* head, int val) {
    if (head == nullptr) {
        return head;
    }

    ListNode* dummyHead = new ListNode();
    dummyHead->next = head;

    ListNode* prev = dummyHead;

    while (prev->next != nullptr) {
        if (prev->next->val == val) {
            ListNode* del = prev->next; // node need to delete
            prev->next = del->next;
            delete del;
        }
        else {
            prev = prev->next;  // move to next node
        }
    }

    // remember to delete dummy head as well
    head = dummyHead->next;
    delete dummyHead;

    return head;
}
```





```c++
class MyLinkedList {
public:
    // singly linked list
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(): val(0), next(nullptr) {}
        LinkedNode(int v): val(v), next(nullptr) {}
    };

    MyLinkedList() {
        dummyHead = new LinkedNode();
        size = 0;
    }
    
    int get(int index) {
        if (index < 0 || index > (size - 1) ) return -1;
    
        LinkedNode* curr = dummyHead;
        for (int i = 0; i <= index; i++) {
            curr = curr->next;
        }

        return curr->val;
    }
    
    // 在链表最前面插入一个节点，插入完成后，新插入的节点为链表的新的头结点
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode();
        newNode->val = val;

        newNode->next = dummyHead->next;
        dummyHead->next = newNode;

        size++;
    }
    
    // 在链表最后面添加一个节点
    void addAtTail(int val) {
        LinkedNode* curr = dummyHead;
        while (curr->next != nullptr) {
            curr = curr->next;
        }

        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = curr->next;
        curr->next = newNode;
        size++;
    }
    
    //在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
    void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        LinkedNode* newNode = new LinkedNode(val);
        // find where to insert
        LinkedNode* cur = dummyHead;
        while(index > 0) {
            cur = cur->next;
            index--;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        size++;
    }
    
    //如果索引 index 有效，则删除链表中的第 index 个节点。
    void deleteAtIndex(int index) {
        if (index < 0 || index > (size - 1)) return;
        // find's delete node's prev node
        LinkedNode* prev = dummyHead;
        while (index > 0) {
            prev = prev->next;
            index--;
        }
        LinkedNode* temp = prev->next;
        prev->next = prev->next->next;
        delete temp;
        size--;
    }

    // need private members
private:
    LinkedNode* dummyHead;
    int size;
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



#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

$1 \to 2 \to 3 \to \phi$

$3 \to 2 \to 1 \to \phi$

递归

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

遍历

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

递归

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

迭代

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

**Method1**

1. get size of linked list
2. locate last Nth node's previous node

![image-20220527205938260](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527205938260.png)

**Method2**

Use fast and slow pointer to locate last Nth node's previous node

由于我们需要找到倒数第 n 个节点，因此我们可以使用两个指针 first, second 同时对链表进行遍历，并且 first 比 second 超前 n 个节点。当 first 遍历到链表的末尾时，second 就恰好处于倒数第 n 个节点。为了使second在倒数第 n 个节点之前，我们可以让second从哑结点开始。

![image-20220527210226191](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220527210226191.png)

#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

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



#### [148. 排序链表](https://leetcode.cn/problems/sort-list/)

**Method1** 

![image-20220529091630947](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529091630947.png)

```c++
ListNode* sortList(ListNode* head) {
    ListNode* fast = head;
    ListNode* slow = head;
    
    while (fast != nullptr && fast->next != nullptr) {
        
    }
}
```







## 树

### 递归

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

depth(root) = 1 + max(depth(root.left) + depth(root.right))

```c++
int maxDepth(TreeNode* root) {
    if (root == nullptr) {
        return 0;
    }
    
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```

#### [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

**自顶向下**

```c++
int height(TreeNode* root) {
    if (!root) {
        return 0;
    }
    else {
        return 1 + max(height(root->left), height(root->right) );
    }
}

bool isBalanced(TreeNode* root) {
    if (!root) {
        return true;
    }        
    else {
        return abs(height(root->left) - height(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right);
    }
}
```

**自底向上**

![image-20220529111411955](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529111411955.png)

后续遍历：左右根

```c++
int height(TreeNode* root) {
    if (root == nullptr) {
        return 0;
    }
    
    int leftHeight = height(root->left);
    int rightHeight = height(root->right);
    if (leftHeight == -1 || rightHeight == -1 || abs(leftHeight - rightHeight) > 1) {
        return -1;
    }
    else {
        return 1 + max(leftHeight, rightHeight);
    }
}

bool isBalanced(TreeNode* root) {
	return height(root) >= 0;
}
```

#### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

节点最大直径 = 左子树深度+右子树深度

![image-20220529115133913](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529115133913.png)

```c++
int diameter = -1;	// initial diameter as min

int diameterOfBinaryTree(TreeNode* root) {
    depth(root);
    
    return diameter;
}

int depth(TreeNode* root) {
    if (root == nullptr) {
        return 0;
    }
    
    int leftDepth = depth(root->left);
    int rightDepth = depth(root->right);
    
    diameter = max(diameter, leftDepth + rightDepth);	// update max diameter
    
    return 1 + max(leftDepth, rightDepth);	// return depth
}
```

#### [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

![image-20220529155937396](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529155937396.png)

对于每个节点p：rootSum(p, val)

而对于所有的节点：rootSum(root, targetSum) + pathSum(root->left, targetSum) + pathSum(root->right, targetSum)

rootSum(root, targetSum): 对于root有多少种路径？递归左右子树得到

pathSum(root->left, targetSum)：递归左子树，对于左子树都多少种路径？遍历所有节点

总共有两次遍历

```c++
int rootSum(TreeNode* root, int targetSum) {
    if (root == nullptr) {
        return 0;
    }
    
    int res = 0;
    
    if (root->val == targetSum) res++;
    
    res += rootSum(root->left, targetSum - root->val);
    res += rootSum(root->right, targetSum - root->val);
    
    return res; 
}

int pathSum(TreeNode* root, int targetSum) {
    if (root == nullptr) {
        return 0;
    }
    
    int res = rootSum(root, targetSum);
    res += pathSum(root->left, targetSum);
    res += pathSum(root->right, targetSum);
    
    return res;
}
```

#### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

两个子树是否**对称**或者**相等**：

![image-20220529161832941](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529161832941.png)

DFS：

```c++
// check if two tree are mirror
bool check(TreeNode* t1, TreeNode* t2) {
    // both null tree
    if (!t1 && !t2) {
        return true;
    }

    // one tree is empty
    if (!t1 || !t2) {
        return false;
    }

    return (t1->val == t2->val) && check(t1->left, t2->right) && 
            check(t1->right, t2->left);
}


bool isSymmetric(TreeNode* root) {
    return check(root, root);
}
```

BFS：

```c++
bool isSymmetric(TreeNode* root) {
    std::queue<TreeNode*> q;
    q.push(root);
    q.push(root);

    while (!q.empty()) {
        TreeNode* t1 = q.front();
        q.pop();
        TreeNode* t2 = q.front();
        q.pop();

        if (!t1 && !t2) continue;    // both empty tree
        if ((!t1 || !t2) || (t1->val != t2->val)) return false;   

        // mirror pair
        q.push(t1->left);
        q.push(t2->right);

        q.push(t1->right);
        q.push(t2->left);

    }

    return true;
}
```

### 层次遍历

![image-20220529162725024](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529162725024.png)

#### [637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

```c++
vector<double> averageOfLevels(TreeNode* root) {
    vector<double> res;
    
    if (root == nullptr) {
        return res;
    }

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int level_size = q.size();  // current level size
        double sum = 0.0;

        // pop up all current level's nodes and add next level's nodes
        for (int i = 0; i < level_size; i++) {
            TreeNode* node = q.front();
            q.pop();
            sum += node->val;

            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }

        res.push_back(sum/level_size);

    }

    return res;
}
```

### 前中后序遍历

前：根左右

中：左根右

后：左右根

```c++
void preorder(TreeNode* root) {
    visit(root);
    preorder(root->left);
    preorder(root->right);
}

void inorder(TreeNode* root) {
    inorder(root->left);
    visit(root);
    inorder(root->right);
}

void postorder(TreeNode* root) {
    postorder(root->left);
    postorder(root->right);
    visit(root);
}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

![image-20220529165638529](/Users/jingyanglin/Library/Application Support/typora-user-images/image-20220529165638529.png)

```c++
unordered_map<int, int> index;  // {TreeNode->val, TreeNode->index}

// build tree recursively based on preorder (not inorder!!!)
TreeNode* buildTreeHelper(const vector<int>& preorder,const vector<int>& inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right ){
    if (preorder_left > preorder_right) {
        return nullptr;
    }    

    // find root index in preorder
    int preorder_root = preorder_left;

    // find root index in inorder
    int inorder_root = index[preorder[preorder_root]];

    // build root
    TreeNode* root = new TreeNode(preorder[preorder_root]);

    // size of left subtree
    int size_left_subtree = inorder_root - inorder_left;

    // build left subtree
    root->left = buildTreeHelper(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root-1);

    // build right subtree
    root->right = buildTreeHelper(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);

    return root;
}


TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int n = preorder.size();
    // build hash map
    for (int i = 0; i < n; i++){
        index[inorder[i]] = i;
    }

    return buildTreeHelper(preorder, inorder, 0, n-1, 0, n-1);
}
```



## 图

![img](https://em62weavv9.feishu.cn/space/api/box/stream/download/asynccode/?code=MDUyNTQzODlmNjcwNWJkYzExZjc0YTM4MDhkODA0ZmZfT2RaVWdMVWtUd2hsYm9aSTByazZoeDZKbFp6VUtkaHRfVG9rZW46Ym94Y25idnd2ajZYblJUVENISkpaRVZuekZiXzE2NTM4OTYzNzQ6MTY1Mzg5OTk3NF9WNA)

![img](https://em62weavv9.feishu.cn/space/api/box/stream/download/asynccode/?code=YzljYTg4ZWU3MGMwYWE3Y2IyMmJiNjY2MjI3ZjI5YjNfVkpJTFE0MUJHZmxkN3lYUUxVTU1DU2FLYnVUN1N0TnVfVG9rZW46Ym94Y252STRhN3VNM0VybDRpZFpNS3RlZVdiXzE2NTM4OTY0MTE6MTY1MzkwMDAxMV9WNA)

### Dijkstra

Node has three states: 

1. Open set: implemented in priority queue = <g(n), node n>, (visited but unexpanded, meaning that sucessors have not been explored yet). This is the list of pending tasks.
2. Close set: visited and expanded, the node in close set has optimal cost. 
3. Not in open set and close set: unvisited node (newly explored node).

g(n) = accumulated cost from start state to node "n"

Strategy: **expand node with least g(n)**

```c++
struct Node {
    state;	// open list, close list, unvisited
    parent;
    g;	// distance to source
}
```

```c++
void Dijkstra(...) {
    typedef pair<int, int> pi;	// <g(n), node index>
    priority_queue<pi, vector<pi>, greater<pi>> OpenSet;
    
    // initialize
    dist[source] = 0;	// source.g = 0;
    dist[other_nodes] = inf;	// initial other nodes g as inf, g[n] = inf
    vector<bool> CloseSet (Graph.size()); // store expanded node
    
    while (true) {
        // no nodes to expand, return 
        if (OpenSet.empty()) {
            return;
        }
        // expand node with lowest g(n) from priority queue
        auto [g, n] = OpenSet.top();
        OpenSet.pop();
        // mark node n as expanded
        CloseSet[n] = true;
        // if node n is goal state, return 
        if (n == goal) {
            return;
        }
        // for all neighbors "m" of node "n", m is n's neighbor
        for (auto m:n) {
            if (m.state == unvisited) {// g[m] == inf, new explore
                g[m] = g[n] + C(n,m);	// C(n,m): cost from n to m
                OpenSet.push(make_pair(g[m], m)); 	// add to OpenSet
                // update node m
                m.state = OpenSet;
                m.parent = n;
            }
            if (m.state == OpenSet)	{ // update if path from n to m is better
                if (g[m] > g[n] + C(n,m)) {
                    g[m] = g[n] + C(n,m);
                    m.parent = n;
                }
            }
            if (m.state == CloseSet) {	// m is already optimal
                continue;
            }
        }
    }
}
```



### A*

Open set: implemented in priority queue = <f(n), node n>

f(n) = g(n) + h(n)

g(n) = accumulated cost from start state to node "n"

h(n) = **estimated least cost** from node n to goal state

Strategy: **expand node with least f(n) = g(n) + h(n)** (only different to Dijkstra)

A key property is admissibility: an **admissible heuristic** is one that never overestimates the cost to reach a goal. (An admissible heuristic is therefore optimistic)

h(n) <= h*(n)

h*(n) = **true least cost** from node n to goal state





**weight A***

f(n) = a * g(n) + b * h(n)

a = 0, b = 1: f(n) = h(n), greedy

a = 1, b = 1: f(n) = g(n) + h(n), A*

a = 1, b = 0: f(n) = g(n), Dijkstra

```c++
// node.h

typedef GridNode* GridNodePtr;
#define inf 1>>20

struct GridNode {
    int id;	// open set = 1, close set = -1, unvisited = 0
    double gScore, fScore;
    GridNodePtr cameFrome;	// parent
    
    std::multimap<double, GridNodePtr>::iterator nodeMapIt;	// iterator that points to its position in multimap
   	
    // initialization
    GridNode() {
        id = 0;
        gScore = inf;
        fScore = inf;
        cameFrom = nullptr;
    }
};
```

```c++
// Astar.cpp

std::multimap<double, GridNodePtr> openSet;	// <fScore, GridNodePtr>

double getHeu(GridNode* node1, GridNode* node2);	// get heuristic 

openSet.clear();

// put start node to open set
startPtr->gScore = 0;
startPtr->fScore = getHeu(startPtr, endPtr);
startPtr->id = 1;
startPtr->cameFrom = nullptr;
startPtr->nodeMapIt = openSet.insert(make_pair(startPtr->fScore, startPtr));	// (fScore, Node)

while(true) {
    if (openSet.empty()) 
        return;
    
    // remove node with lowest f(n)
    currentPtr = openSet.begin()->second;
    openSet.erase(openSet.begin());
    // add to close set (expanded)
    currentPtr->id = -1;	
    
    // if reach goal
    if (currentPtr->index == goalIdx) 
        return;
    
    // neighbors
    for (auto neighborPtr : currentPtr->neighbors) {
        // new visited
        if (neighborPtr->id == 0) {
            neighborPtr->gScore = currentPtr->gScore + edgeCost();
            neighborPtr->fScore = neighborPtr->gScore + neighborPtr->hScore;
            // add to open set
            neighborPtr->id = 1;
            neighborPtr->nodeMapIt = openSet.insert(make_pair(neighborPtr->fScore, neighborPtr));
            neighborPtr->cameFrom = currentPtr;
        }
        
        // in open set
        if (neighborPtr->id == 1) {
            if (neighborPtr->gScore > currentPtr->gScore + edgeCost()) {
                neighborPtr->gScore = currentPtr->gScore + edgeCost();
                neighborPtr->fScore = neighborPtr->gScore + neighborPtr->hScore;
                neighborPtr->cameFrom = currentPtr;
                // update open set
                openSet.erase(neighborPtr->nodeMapIt);
                neighborPtr->nodeMapIt = openSet.insert(make_pair(neighborPtr->fScore, neighborPtr));
            }
        }
        
        // in close set
        if (neighborPtr->id == -1) {
            continue;
        }        
    }        
}
```

```c++
// get_path.cpp
vector<Vector3d> path;
vector<GridNodePtr> gridPath;

GridNodePtr ptr = terminatePtr;
while(ptr){
    gridPath.push_back(ptr);
    ptr=ptr->cameFrom;
}

for (auto ptr : gridPath)
    path.push_back(ptr->coord);

reverse(path.begin(), path.end());
```

### Priority Queue Implementation

**Method1** 

`std::priority_queue`

```c++
template <class T, class Container = vector<T>,
  class Compare = less<typename Container::value_type> > class priority_queue     
```

Default: first element is the greatest element 

```c++
// pair = <cost, vertex index>
typedef pair<int,int> pi;

priority_queue<pi, vector<pi>, greater<pi>> pq;	// use greater rather than less to achieve first is minimum

pq.empty();
pq.top();
pq.pop();
pq.push(make_pair(dist[m], m));

```

**Method2**

`std::multimap`

```c++
template < class Key,                                     // multimap::key_type
           class T,                                       // multimap::mapped_type
           class Compare = less<Key>,                     // multimap::key_compare
           class Alloc = allocator<pair<const Key,T> >    // multimap::allocator_type
           > class multimap;
```

Default: first element is the element with minimum key 

```c++
std::multimap<double, GridNodePtr> openSet;	// <fScore, GridNodePtr>

openSet.begin();	// return iterator points to first element
GridNodePtr currentPtr = openSet.begin()->second;

openSet.erase(openSet.begin());	// remove elements using according to iterator

// return an iterator pointing to the newly inserted element in the multiset.
neighborPtr->nodeMapIt = openSet.insert(make_pair(neighborPtr->fScore, neighborPtr));
```





