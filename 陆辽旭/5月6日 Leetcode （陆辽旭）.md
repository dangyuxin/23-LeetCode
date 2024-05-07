## 5月6日 Leetcode （陆辽旭）

#### 133.克隆图（[133. 克隆图 - 力扣（LeetCode）](https://leetcode.cn/problems/clone-graph/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240506204756.png)

##### code：

```c
struct Node** map;

struct Node* dfs(struct Node* s) {
    if (s == NULL) {
        return NULL;
    }

    // 如果该节点已经被访问过了，则直接从哈希表中取出对应的克隆节点返回
    if (map[s->val]) {
        return map[s->val];
    }

    // 克隆节点，注意到为了深拷贝我们不会克隆它的邻居的列表
    struct Node* cloneNode = (struct Node*)malloc(sizeof(struct Node));
    cloneNode->val = s->val;
    cloneNode->numNeighbors = s->numNeighbors;

    // 哈希表存储
    map[cloneNode->val] = cloneNode;
    cloneNode->neighbors = (struct Node**)malloc(sizeof(struct Node*) * cloneNode->numNeighbors);

    // 遍历该节点的邻居并更新克隆节点的邻居列表
    for (int i = 0; i < cloneNode->numNeighbors; i++) {
        cloneNode->neighbors[i] = dfs(s->neighbors[i]);
    }
    return cloneNode;
}

struct Node* cloneGraph(struct Node* s) {
    map = (struct Node**)malloc(sizeof(struct Node*) * 101);
    memset(map, 0, sizeof(struct Node*) * 101);
    return dfs(s);
}
```





#### 1971.寻找图中是否存在路径（[1971. 寻找图中是否存在路径 - 力扣（LeetCode）](https://leetcode.cn/problems/find-if-path-exists-in-graph/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240506212409.png)

##### code：

```c
// 创建一个新的节点
struct ListNode *creatListNode(int val) {
    struct ListNode *node = (struct ListNode *)malloc(sizeof(struct ListNode)); // 为节点分配内存空间
    node->val = val; // 赋值
    node->next = NULL; // 下一个指针为NULL
    return node; 
}


bool validPath(int n, int** edges, int edgesSize, int* edgesColSize, int source, int destination){
    struct ListNode * adj[n]; // 创建邻接表
    bool visited[n]; // 创建数组，用于记录节点是否被访问
    // 初始化
    for (int i = 0; i < n; i++) {
        adj[i] = NULL;
        visited[i] = false; 
    }
    // 构建邻接表
    for (int i = 0; i < edgesSize; i++) {
        int x = edges[i][0], y = edges[i][1]; 
        struct ListNode *nodex = creatListNode(x); // 起始节点
        nodex->next = adj[y]; // 将起始节点连接到结束节点的邻接表中
        adj[y] = nodex; // 更新结束节点的邻接表
        // 创建结束节点到起始节点的连接，与上面同理
        struct ListNode *nodey = creatListNode(y); 
        nodey->next = adj[x]; 
        adj[x] = nodey; 
    }
    // 使用广度优先搜索遍历图
    int queue[n]; // 创建一个队列，用于存储待访问节点
    int head = 0, tail = 0; 
    queue[tail++] = source; 
    visited[source] = true; 
    while (head != tail) { 
        int vertex = queue[head++];
        if (vertex == destination) { // 如果当前节点是目标节点，则存在路径
            break;
        }
        // 遍历当前节点的邻接表
        for (struct ListNode *p = adj[vertex]; p != NULL; p = p->next) {
            int next = p->val; // 获取下一个节点
            if (!visited[next]) { // 如果下一个节点未被访问，则加入队列
                queue[tail++] = next; // 入队列
                visited[next] = true; // 标记为已访问
            }
        }
    }
    return visited[destination]; // 返回目标节点是否被访问
}
```

