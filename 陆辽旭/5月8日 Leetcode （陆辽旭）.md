## 5月8日 Leetcode （陆辽旭）

#### 685.冗余链接||（[685. 冗余连接 II - 力扣（LeetCode）](https://leetcode.cn/problems/redundant-connection-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240508193645.png)

##### code：

```c
/*
n个节点有n-1个分支，多出1个分支存在两种情况：
    1.所有节点出度为1，即肯定形成了环，取形成环的节点(u和v同父)
    2.存在节点出度为2，表现在v节点入树时已经存在父节点，说明v有两个父节点，当前的边u,v冲突
*/
int* ancestor;

int find(int index) {
    return index == ancestor[index] ? index : (ancestor[index] = find(ancestor[index]));
}

void merge(int u, int v) {
    ancestor[find(u)] = find(v);
}

int* findRedundantDirectedConnection(int** edges, int edgesSize, int* edgesColSize, int* returnSize) {
    int n = edgesSize;
    ancestor = malloc(sizeof(int) * (n + 1));//并查集
    for (int i = 1; i <= n; ++i) {
        ancestor[i] = i;
    }
    int parent[n + 1];//记录树的父节点
    for (int i = 1; i <= n; ++i) {
        parent[i] = i;
    }
    int conflict = -1;
    int cycle = -1;
    for (int i = 0; i < n; ++i) {
        int node1 = edges[i][0], node2 = edges[i][1];
        if (parent[node2] != node2) {//计入v时发现v有两个父节点，标记冲突节点
            conflict = i;
        } else {
            parent[node2] = node1;
            if (find(node1) == find(node2)) {//u和v同父，标记环节点
                cycle = i;
            } else {
                merge(node1, node2);
            }
        }
    }
    int* redundant = malloc(sizeof(int) * 2);
    *returnSize = 2;
    if (conflict < 0) {//不存在冲突
        redundant[0] = edges[cycle][0], redundant[1] = edges[cycle][1];
        return redundant;
    } else {//存在冲突
        int* conflictEdge = edges[conflict];
        if (cycle >= 0) {//同时存在环， redundant[0]为冲突[u,v]中v的父节点
            redundant[0] = parent[conflictEdge[1]], redundant[1] = conflictEdge[1];
            return redundant;
        } else {//不存在环
            redundant[0] = conflictEdge[0], redundant[1] = conflictEdge[1];
            return redundant;
        }
    }
    return redundant;
}
```

