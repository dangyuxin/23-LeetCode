## 4月29日 Leetcode（陆辽旭）

#### 417.太平洋大西洋水流问题（[417. 太平洋大西洋水流问题 - 力扣（LeetCode）](https://leetcode.cn/problems/pacific-atlantic-water-flow/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240429185712.png)

##### code：

```c
void dfs(int** heights, int i, int j, int maxLen, int maxCol, bool** res) {
    int step[4][2] = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    if (res[i][j] == true) {
        return;
    }
    res[i][j] = true;
    // 判断相邻的四个方向的单元格的高度是不是比当前单元格高，如果比当前高，则继续递归
    for (int k = 0; k < 4; k++) {
        int new_i = i + step[k][0];
        int new_j = j + step[k][1];
        if (new_i >= 0 && new_i < maxLen && new_j >= 0 && new_j < maxCol &&
            heights[new_i][new_j] >= heights[i][j]) {
            dfs(heights, new_i, new_j, maxLen, maxCol, res);
        }
    }
}

int** pacificAtlantic(int** heights, int heightsSize, int* heightsColSize,
                      int* returnSize, int** returnColumnSizes) {
    int col = heightsColSize[0];
    // 1. 用bool数组来标记能流向太平洋的单元格
    bool** Pacific = (bool**)calloc(sizeof(bool*) * heightsSize, sizeof(bool*));
    bool** Atlantic =
        (bool**)calloc(sizeof(bool*) * heightsSize, sizeof(bool*));
    for (int i = 0; i < heightsSize; i++) {
        Pacific[i] = calloc(sizeof(bool) * col, sizeof(bool));
        Atlantic[i] = calloc(sizeof(bool) * col, sizeof(bool));
    }

    // 太平洋的dfs从左边缘和上边缘开始
    for (int i = 0; i < heightsSize; i++) {
        dfs(heights, i, 0, heightsSize, col, Pacific);
    }
    for (int i = 0; i < col; i++) {
        dfs(heights, 0, i, heightsSize, col, Pacific);
    }

    // 大西洋，从下边缘和右边缘dfs
    for (int i = heightsSize - 1; i >= 0; i--) {
        dfs(heights, i, col - 1, heightsSize, col, Atlantic);
    }
    for (int i = col - 1; i >= 0; i--) {
        dfs(heights, heightsSize - 1, i, heightsSize, col, Atlantic);
    }
    // 结果
    int** ans =
        (int**)calloc(sizeof(int*) * (heightsSize * col),
                      sizeof(int*)); // 最多所有的单元格都是结果heightsSize*col
    for (int i = 0; i < heightsSize * col; i++) {
        ans[i] = calloc(sizeof(int) * 2, sizeof(int));
    }
    int top = 0;

    // 同时遍历Pacific 和Atlantic 两个都为true，则放入ans
    for (int i = 0; i < heightsSize; i++) {
        for (int j = 0; j < col; j++) {
            if (Pacific[i][j] && Atlantic[i][j]) {
                ans[top][0] = i;
                ans[top][1] = j;
                top++;
            }
        }
    }

    *returnColumnSizes = (int*)malloc(sizeof(int) * top); // 返回值要申请空间
    for (int i = 0; i < top; i++) {
        (*returnColumnSizes)[i] = 2;
    }
    *returnSize = top;
    return ans;
}
```





#### 787.k站中转内最便宜的航班（[787. K 站中转内最便宜的航班 - 力扣（LeetCode）](https://leetcode.cn/problems/cheapest-flights-within-k-stops/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240429210939.png)

##### code：

```c
typedef struct {
    int node, dist, exp;//下标，转机次数，花费
} Queue;

Queue que[7000]; 
int map[100][100]; 

int BFS(int n, int src, int dst, int k) {
    int head = 0, tail = 0, visited[n]; // 头指针、尾指针、访问标记数组
    memset(visited, -1, sizeof(visited)); // 初始化访问标记数组为-1
    que[tail].node = src, que[tail].dist = 0; 
    que[tail++].exp = 0, visited[src] = 0; 
    while (head != tail) { // 当队列不为空时
        Queue temp = que[head++]; // 出队
        if (temp.dist > k) { // 如果当前航班已经超过了限定的转机次数
            continue; 
        }
        for (int i = 0; i < n; i++) { 
            if (map[temp.node][i] <= 0) { // 如果不存在航班或价格为负数
                continue; 
            }
            if (visited[i] != -1 && visited[i] < temp.exp + map[temp.node][i]) { // 如果已经访问过且花费更少
                continue; // 跳过
            }
            else { // 否则更新
                visited[i] = temp.exp + map[temp.node][i];
            }
            que[tail].node = i, que[tail].dist = temp.dist + 1; // 更新队列节点和距离
            que[tail++].exp = temp.exp + map[temp.node][i]; // 更新经历时间
        }
    }
    return visited[dst]; // 返回目的地最少花费
}


int findCheapestPrice(int n, int** flights, int flightsSize, int* flightsColSize, int src, int dst, int k) {
    memset(map, -1, sizeof(map)); 
    for (int i = 0; i < flightsSize; i++) { 
        map[flights[i][0]][flights[i][1]] = flights[i][2];
    }
    return BFS(n, src, dst, k);
}
```





#### 547.省份数量（[547. 省份数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-provinces/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240429213023.png)

##### code：

```c
void dfs(int** isConnected, int* book, int isConnectedSize, int i) {
    for (int j = 0; j < isConnectedSize; ++j) {
        if (book[j] == 0 && isConnected[i][j] == 1) { // 如果邻接城市未被访问过且与当前城市相连
            book[j] = 1;
            dfs(isConnected, book, isConnectedSize, j); // 递归地访问邻接城市的邻接城市
        }
    }
}

int findCircleNum(int** isConnected, int isConnectedSize, int* isConnectedColSize) {
    int book[isConnectedSize];
    memset(book, 0, sizeof(book));
    int provinces = 0;
    for (int i = 0; i < isConnectedSize; i++) {
        if (!book[i]) {
            dfs(isConnected, book, isConnectedSize, i);
            provinces++;
        }
    }
    return provinces;
}
```

