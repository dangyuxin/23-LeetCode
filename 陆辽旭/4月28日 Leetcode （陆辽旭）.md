## 4月28日 Leetcode （陆辽旭）

#### 207.课程表（[207. 课程表 - 力扣（LeetCode）](https://leetcode.cn/problems/course-schedule/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240428205213.png)

##### code：

```c
bool canFinish(int numCourses, int** prerequisites, int prerequisitesSize,
               int* prerequisitesColSize) {
    if (prerequisitesSize == 0) {
        return true;
    }
    int* ans = malloc(sizeof(int) * numCourses);
    int i = 0;
    for (i = 0; i < numCourses; i++) // 初始化为0
    {
        ans[i] = 0;
    }
    int m = prerequisitesSize;
    int j = 0;
    int res = 0;
    for (i = 0; i < prerequisitesSize; i++) // 更新每个点的入度
    {
        ans[prerequisites[i][0]]++;
    }
    for (i = 0; i < numCourses; i++) {
        if (ans[i] == 0) // 当前科目i学完了
        {
            res++; // 学完的科目总数+1
            if (res == numCourses) // 总数等于我要学的科目，说明我全部可以学完
            {
                return true;
            }
            for (j = 0; j < prerequisitesSize; j++) // 更新以我为入度的点的入度
            {
                if (prerequisites[j][1] == i) {
                    ans[prerequisites[j][0]]--;
                }
            }
            ans[i]--; // 避免重复比较
            i = -1;   // 从头重新开始比较
        }
    }
    return false;
}
```





#### 210.课程表||（[210. 课程表 II - 力扣（LeetCode）](https://leetcode.cn/problems/course-schedule-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240428211323.png)

##### code：

```c
int* findOrder(int numCourses, int** prerequisites, int prerequisitesSize, int* prerequisitesColSize, int* returnSize) {
    int* fin = malloc(sizeof(int) * numCourses);
    int* ans = malloc(sizeof(int) * numCourses);
    
    for (int i = 0; i < numCourses; i++) {
        ans[i] = 0; // 初始化为0
    }
    
    for (int i = 0; i < prerequisitesSize; i++) {
        ans[prerequisites[i][0]]++; // 更新每个点的入度
    }
    
    int res = 0;
    for (int i = 0; i < numCourses; i++) {
        if (ans[i] == 0) { // 当前科目i学完了
            fin[res++] = i; // 学完的科目总数+1
            for (int j = 0; j < prerequisitesSize; j++) { // 更新以我为入度的点的入度
                if (prerequisites[j][1] == i) {
                    ans[prerequisites[j][0]]--;
                }
            }
            if (res == numCourses) { // 总数等于我要学的科目，说明我全部可以学完
                *returnSize = numCourses;
                return fin;
            }
            ans[i]--; // 避免重复比较
            i = -1;   // 从头重新开始比较
        }
    }
    
    *returnSize = 0; // 没有找到满足条件的学习顺序
    free(fin);
    free(ans);
    return NULL;
}
```

