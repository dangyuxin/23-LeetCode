## 5月7日 Leetcode （陆辽旭）

#### 684.冗余链接（[684. 冗余连接 - 力扣（LeetCode）](https://leetcode.cn/problems/redundant-connection/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240507185741.png)

##### code：

```c
int n = 1005;

// 并查集初始化
void init(int* father) {
    for (int i = 0; i < n; ++i) {
        father[i] = i;
    }
}

// 并查集里寻根的过程
int find(int u, int* father) { 
    return u == father[u] ? u : (father[u] = find(father[u], father)); 
}

// 判断 u 和 v 是否找到同一个根
bool isSame(int u, int v, int* father) {
    u = find(u, father);
    v = find(v, father);
    return u == v;
}

// 将 v->u 这条边加入并查集
void join(int u, int v, int* father) {
    int rootU = find(u, father); // 寻找u的根
    int rootV = find(v, father); // 寻找v的根
    if (rootU == rootV) {
        return; // 如果发现根相同，则说明在一个集合，不用两个节点相连直接返回
    }
    father[rootV] = rootU;
}

int* findRedundantConnection(int** edges, int edgesSize, int* edgesColSize, int* returnSize) {
    int father[n];
    init(father);
    for (int i = 0; i < edgesSize; i++) {
        if (isSame(edges[i][0], edges[i][1], father)) {
            *returnSize = 2;
            return edges[i];
        } else {
            join(edges[i][0], edges[i][1], father);
        }
    }
    *returnSize = 0;
    return NULL;
}
```

