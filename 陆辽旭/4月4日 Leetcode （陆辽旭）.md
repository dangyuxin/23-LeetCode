## 4月4日 Leetcode （陆辽旭）

#### 86.分隔链表（[86. 分隔链表 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-list/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240404204813.png)

##### code：

```c
struct ListNode* partition(struct ListNode* head, int x) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    struct ListNode *dummy = (struct ListNode*)malloc(sizeof(struct ListNode));
    dummy->next = head;
    struct ListNode *pre = dummy;  // 前一个节点
    struct ListNode *cur = head;   // 当前节点
    // 找到第一个大于等于 x 的节点
    while (pre->next != NULL && pre->next->val < x) 
    {
        pre = pre->next;
    }
    cur = pre;//cur = pre->next如果pre->next = NULL就会出错
    // 从第一个大于等于 x 的节点开始，将小于 x 的节点移动到其前面
    while (cur->next != NULL) 
    {
        if (cur->next->val < x) 
        {
            struct ListNode *temp = cur->next;
            cur->next = temp->next;  // 删除当前节点
            temp->next = pre->next;  // 将当前节点插入到pre之后
            pre->next = temp;  // 更新pre的下一个节点
            pre = pre->next;  // 更新pre指针
        } 
        else 
        {
            cur = cur->next;
        }
    }
    return dummy->next;
}
```



#### 42.接雨水（[42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240404212728.png)

##### code：

```c
int trap(int* height, int heightSize) {
    int i, j, start;
    int sum = 0, max = 0, pre = 0;
    int n = heightSize;
        
    // 找到起始位置,即第一个可以接水的坑
    for(start = 0; start < n - 1; start++) {
        if(height[start] > 0 && height[start] > height[start+1]) {
            max = height[start];
            break;
        } 
    }
        
    i = start;
    // 计算盛水面积，ij指针维系一个窗口，i指向前高点，j指向后高点，i向j移动缩小窗口，使得sum加上前高点减去此时坑高度，即可以接的雨水
    for(j = start; j < n; j++) {
        if(height[j] >= max) {
            pre = max;
            max = height[j];
            while(i < j) {
                sum += (pre - height[i]);
                i++; 
            }
        }
    }
    //正序接完还要再接逆序，最后结果是正序逆序接的雨水之和
    // 找到起始位置（逆序）
    for(start = n - 1; start > 0; start--) {
        if(height[start] > 0 && height[start] > height[start-1]) {
            max = height[start];
            break;
        } 
    }
    i = start;
    // 计算盛水面积（逆序）
    for(j = start; j >= 0; j--) {
        if(height[j] > max) {
            pre = max;
            max = height[j];
            while(i > j) {
                sum += (pre - height[i]);
                i--; 
            }
        }
    }
    return sum;
}
```



### 