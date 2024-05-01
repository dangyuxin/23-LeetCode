## 4月27日 Leetcode （陆辽旭）

#### 51.N皇后（[51. N 皇后 - 力扣（LeetCode）](https://leetcode.cn/problems/n-queens/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240427222711.png)

##### code：

```c
char*** solveNQueens(int n, int* returnSize, int** returnColumnSizes) {
    char*** res = (char***)malloc(sizeof(char**) * 114514);
    *returnSize = 0;
    *returnColumnSizes = (int*)malloc(sizeof(int) * 114514);
    char **board = (char **)malloc(sizeof(char *) * n);
    for (int i = 0; i < n; i++) {
        board[i] = (char *)malloc(sizeof(char) * (n + 1));
        memset(board[i], '.', n);
        board[i][n] = '\0';
    }
    // 调用回溯函数
    backtrack(board, 0, n, returnSize, res, returnColumnSizes);
    return res;
}

void backtrack(char **board, int row, int n, int* returnSize, char*** res,
               int** returnColumnSizes) {
    if (row == n) {
        // 存储当前解法
        res[*returnSize] = (char**)malloc(sizeof(char*) * n);
        for (int i = 0; i < n; i++) {
            res[*returnSize][i] = (char*)malloc(sizeof(char) * (n + 1));
            strcpy(res[*returnSize][i], board[i]);
        }
        (*returnColumnSizes)[*returnSize] = n;
        (*returnSize)++;
        return;
    }

    for (int i = 0; i < n; i++) {
        if (exist(board, row, i, n)) {
            board[row][i] = 'Q';
            backtrack(board, row + 1, n, returnSize, res, returnColumnSizes);
            board[row][i] = '.';//回溯
        }
    }
}

int exist(char **board, int row, int col, int n) {
    // 检查列是否有皇后
    for (int i = 0; i < n; i++) {
        if (board[i][col] == 'Q') {
            return 0;
        }
    }
    // 检查左上方是否有皇后
    for (int x = row - 1, y = col - 1; x >= 0 && y >= 0; x--, y--) {
        if (board[x][y] == 'Q') {
            return 0;
        }
    }
    // 检查右上方是否有皇后
    for (int m = row - 1, k = col + 1; m >= 0 && k < n; m--, k++) {
        if (board[m][k] == 'Q') {
            return 0;
        }
    }
    return 1;
}
```

