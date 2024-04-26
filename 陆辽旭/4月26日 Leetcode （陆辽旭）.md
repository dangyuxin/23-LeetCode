## 4月26日 Leetcode （陆辽旭）

#### 669.修剪二叉搜索树（[669. 修剪二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/trim-a-binary-search-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240426191438.png)

##### code：

```c
struct TreeNode* trimBST(struct TreeNode* root, int low, int high) {
    if (root == NULL) return NULL;//搜索到头
    if (root->val < low) return trimBST(root->right, low, high);//小于就递归右子树
    if (root->val > high) return trimBST(root->left, low, high);//大于递归左子树
    root->left = trimBST(root->left, low, high);//拼接
    root->right = trimBST(root->right, low, high);
    return root;
}
```





#### 108.将有序数组转换为二叉搜索树（[108. 将有序数组转换为二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240426200400.png)

##### code：

```c
struct TreeNode* helper(int* nums, int left, int right) {
    if (left > right) {
        return NULL;
    }

    // 总是选择中间位置左边的数字作为根节点
    int mid = (left + right) / 2;

    struct TreeNode* root = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val = nums[mid];
    root->left = helper(nums, left, mid - 1);//递归左，区间是开头到根节点左边一位，这样取到的总是中间位置
    root->right = helper(nums, mid + 1, right);
    return root;
}

struct TreeNode* sortedArrayToBST(int* nums, int numsSize) {
    return helper(nums, 0, numsSize - 1);
}
```





#### 538.把二叉搜索树转换为累加树（[538. 把二叉搜索树转换为累加树 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240426201327.png)

##### code：

```c
// 二叉搜索树中序遍历升序，只要反中序遍历，即降序并且求和就行
struct TreeNode* dfs(struct TreeNode* cur, int* pre) {
    // 右中左遍历
    if (cur != NULL) {
        dfs(cur->right, pre);
        cur->val += *pre;
        *pre = cur->val;
        dfs(cur->left, pre);
    }
    return cur;
}

struct TreeNode* convertBST(struct TreeNode* root) {
    int pre = 0;
    dfs(root, &pre);
    return root;
}
```





#### 130.被围绕的区域（[130. 被围绕的区域 - 力扣（LeetCode）](https://leetcode.cn/problems/surrounded-regions/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240426204238.png)

##### code：

```c
int n, m;
void dfs(char** board, int x, int y) {
    // 边界判断和停止条件
    if (x < 0 || x >= n || y < 0 || y >= m || board[x][y] != 'O') {
        return;
    }
    // 将'O'标为 'A'
    board[x][y] = 'A';
    // 继续搜索相邻的 'O'
    dfs(board, x + 1, y);
    dfs(board, x - 1, y);
    dfs(board, x, y + 1);
    dfs(board, x, y - 1);
}

void solve(char** board, int boardSize, int* boardColSize) {
    n = boardSize;
    if (n == 0) {
        return;
    }
    m = *boardColSize;
    // 从边界开始搜索
    for (int i = 0; i < n; i++) {
        //左右边
        dfs(board, i, 0);
        dfs(board, i, m - 1);
    }
    for (int i = 1; i < m - 1; i++) {
        //上下边
        dfs(board, 0, i);
        dfs(board, n - 1, i);
    }
    // 将标记过的 'A' 还原为 'O'，未标记的 'O' 修改为 'X'
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == 'A') {
                board[i][j] = 'O';
            } else if (board[i][j] == 'O') {
                board[i][j] = 'X';
            }
        }
    }
}
```

