## 4月25日 Leetcode （陆辽旭）

#### 450.删除二叉搜索树中的节点（[450. 删除二叉搜索树中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-node-in-a-bst/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240425184633.png)

##### code：

```c
struct TreeNode* deleteNode(struct TreeNode* root, int key){
    // 如果根节点为空，直接返回 NULL
    if (root == NULL) {
        return NULL;
    }
    // 如果 key 小于当前节点值，递归删除左子树中的节点
    if (root->val > key) {
        root->left = deleteNode(root->left, key);
        return root;
    }
    // 如果 key 大于当前节点值，递归删除右子树中的节点
    if (root->val < key) {
        root->right = deleteNode(root->right, key);
        return root;
    }
    // 如果当前节点值等于 key
    if (root->val == key) {
        // 如果当前节点没有左右子节点，直接返回 NULL
        if (!root->left && !root->right) {
            return NULL;
        }
        // 如果只有右子节点，返回右子节点
        if (!root->right) {
            return root->left;
        }
        // 如果只有左子节点，返回左子节点
        if (!root->left) {
            return root->right;
        }
        // 如果既有左子节点又有右子节点
        // 找到右子树中最小的节点作为当前节点的替代节点
        struct TreeNode *successor = root->right;
        while (successor->left) {
            successor = successor->left;
        }
        // 递归删除右子树中的最小节点
        root->right = deleteNode(root->right, successor->val);
        // 将替代节点的右子树连接到当前节点的右子树
        successor->right = root->right;
        // 将替代节点的左子树连接到当前节点的左子树
        successor->left = root->left;
        // 返回替代节点作为当前节点的父节点的子节点
        return successor;
    }
    // 返回根节点
    return root;
}
```





#### 701.二叉搜索树中的插入操作（[701. 二叉搜索树中的插入操作 - 力扣（LeetCode）](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240425190421.png)

##### code：

```c
struct TreeNode* createTreeNode(int val) {
    struct TreeNode* ret = malloc(sizeof(struct TreeNode));
    // 设置节点值
    ret->val = val;
    // 左右子节点为空
    ret->left = ret->right = NULL;
    return ret;
}
 
struct TreeNode* insertIntoBST(struct TreeNode* root, int val) {
    // 如果根节点为空，直接将新节点作为根节点返回
    if (root == NULL) {
        root = createTreeNode(val);
        return root;
    }
    struct TreeNode* pos = root;
    while (pos != NULL) {
        // 如果 val 小于当前节点值
        if (val < pos->val) {
            // 如果当前节点的左子节点为空，将新节点插入左子节点位置
            if (pos->left == NULL) {
                pos->left = createTreeNode(val);
                break;
            } else {
                pos = pos->left;
            }
        } else {
            // 如果 val 大于等于当前节点值
            // 如果当前节点的右子节点为空，将新节点插入右子节点位置
            if (pos->right == NULL) {
                pos->right = createTreeNode(val);
                break;
            } else {
                pos = pos->right;
            }
        }
    }
    return root;
}
```





#### 1020.飞地的数量（[1020. 飞地的数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-enclaves/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240425191255.png)

##### code：

```c
bool dfs(int** grid, int i, int j, int n, int m, int* res) {
    // 当前位置超出边界或者为海洋，返回 true
    if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] == 0) return true;
    // 当前边界且为陆地，返回false
    if ((i == 0 || j == 0 || i == n - 1 || j == m - 1) && grid[i][j] == 1) return false;
    // 当前位置为陆地，陆地数量加一
    (*res)++;
    // 标记为已访问过
    grid[i][j] = 0;
    // 递归上、下、左、右
    return dfs(grid, i - 1, j, n, m, res) & dfs(grid, i + 1, j, n, m, res) &
           dfs(grid, i, j - 1, n, m, res) & dfs(grid, i, j + 1, n, m, res);
}

// 计算不被包围的陆地数量
int numEnclaves(int** grid, int gridSize, int* gridColSize) {
    int n = gridSize, m = gridColSize[0];
    int sum = 0; // 存储不被包围的陆地数量
    // 遍历整个网格
    for (int i = 1; i < n - 1; i++) {
        for (int j = 1; j < m - 1; j++) {
            if (grid[i][j] == 1) { // 如果当前位置为陆地且未被访问过
                int res = 0; // 用于存储当前陆地连通的陆地数量
                // 调用深度优先搜索函数判断当前陆地是否被包围
                if (dfs(grid, i, j, n, m, &res)) {
                    sum += res; // 如果未被包围，则累加当前陆地的数量到总和中
                }
            }
        }
    }
    return sum; // 返回不被包围的陆地数量
}
```

