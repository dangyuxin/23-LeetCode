## 5月5日 Leetcode （陆辽旭）

#### 841.钥匙和房间（[841. 钥匙和房间 - 力扣（LeetCode）](https://leetcode.cn/problems/keys-and-rooms/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240505195525.png)

##### code：

```c
void dfs(int** rooms, int roomsSize, int* roomsColSize, int cur, int* book, int* key) {
    // 如果已访问返回
    if (book[cur] == 1) {
        return;
    }
    if (book[cur] != 1 && key[cur] == 1) {
        book[cur] = 1; // 标记
        for (int i = 0; i < roomsColSize[cur]; ++i) {
            key[rooms[cur][i]] = 1; // 获取钥匙
            dfs(rooms, roomsSize, roomsColSize, rooms[cur][i], book, key); // 递归搜索下一个房间
        }
    }
    return;
}

bool canVisitAllRooms(int** rooms, int roomsSize, int* roomsColSize) {
    int book[roomsSize];
    memset(book, 0, sizeof(book));
    int key[roomsSize];
    memset(key, 0, sizeof(book));
    key[0] = 1; // 第一个房间有钥匙
    // 对每个房间进行bfs
    for (int i = 0; i < roomsSize; ++i) {
        dfs(rooms, roomsSize, roomsColSize, i, &book, &key);
    }
    // 检查是否所有房间都被访问过
    for (int j = 0; j < roomsSize; ++j) {
        if (book[j] != 1) {
            return false; 
        }
    }
    return true; 
}
```





#### 827.最大人工岛（[827. 最大人工岛 - 力扣（LeetCode）](https://leetcode.cn/problems/making-a-large-island/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240505205426.png)

##### code：

```c
#define NUM_MAX 250000 
#define INDEX_INITIAL 2 

int dfs(int **grid, int row, int col, int x, int y, int index)
{
    // 边超出网格范围或当前单不是陆地
    if (x < 0 || y < 0 || x >= row || y >= col) {
        return 0;
    }
    if (grid[x][y] != 1) {
        return 0;
    }
    // 单元格标记为当前索引
    grid[x][y] = index;
    return 1 + dfs(grid, row, col, x + 1, y, index) + dfs(grid, row, col, x - 1, y, index) +
        dfs(grid, row, col, x, y + 1, index) + dfs(grid, row, col, x, y - 1, index);
}


int getArea(int **grid, int *map, int row, int col, int x, int y, int index)
{
    // 获取上下左右的值
    int upval = x == 0 ? 0 : grid[x - 1][y];
    int downval = x == row - 1 ? 0 : grid[x + 1][y];
    int leftval = y == 0 ? 0 : grid[x][y - 1];
    int rightval = y == col - 1 ? 0 : grid[x][y + 1];
    // 计算周围陆地的总面积
    int ret = 1 + map[upval];
    if (upval != leftval) {
        ret += map[leftval];
    }
    if (downval != leftval && downval != upval) {
        ret += map[downval];
    }
    if (rightval != upval && rightval != leftval && rightval != downval) {
        ret += map[rightval];
    }
    return ret;
}

int largestIsland(int **grid, int gridSize, int *gridColSize)
{
    int row = gridSize;
    int col = *gridColSize;
    int map[NUM_MAX] = {0}; 
    int index = INDEX_INITIAL; 
    int max = 0;

    // 计算所有岛屿的大小，并将它们存储在map数组中
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (grid[i][j] == 1) {
                int size = dfs(grid, row, col, i, j, index);
                max = fmax(max, size); 
                map[index++] = size; 
            }
        }
    }
    
    // 遍历整个网格找到最大值
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (grid[i][j] == 0) {
                int sum = getArea(grid, map, row, col, i, j, index);
                max = fmax(max, sum); 
            }
        }
    }
    return max;
}
```

