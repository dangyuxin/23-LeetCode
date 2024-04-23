## 4月2日 Leetcode （陆辽旭）

#### 203.移除链表元素（[203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240402220839.png)

##### **code：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* removeElements(struct ListNode* head, int val) {
    struct ListNode *dummy = (struct ListNode*)malloc(sizeof(struct ListNode));
    dummy->next = head;//创建哑节点
    if(head == NULL) return head;//空链表直接返回
    struct ListNode *pre = dummy;//前指针
    struct ListNode *cur = head;//后指针
    while(cur->next != NULL) {//遍历后指针，查找val，找到就移除
        if(cur->val == val) {
            pre->next = cur->next;
            cur = cur->next;
        }
        else {
            pre = pre->next;
            cur = cur->next;
        }
    }
    if(cur->val == val) pre->next = NULL;//最后val的情况
    return dummy->next;
}
```



#### 206.反转链表（[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)）

![](C:\Users\11484\Desktop\实验室\3g\每日一题\4.2\QQ截图20240402220908.png)

##### **code：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL || head->next == NULL) return head; //空链表或者只有一个
    //就地逆置法
    // 定义beg和end两个节点，其中beg指向第一节点(头)，
    // end指向第二节点。然后进行操作把end指向节点取出来，并且改为指向beg，
    // 即把这个节点取出来作为链表的头节点，同时使beg指向下一个节点(第三节点)，这样就可以保持链表的完整了。
    // 随后遍历使beg位置不变end向下走，会取出第三节点当表头，第四当表头，第五...当逆置完成时，
    // 原来的尾节点成了头节点，此时beg节点走到新链表的尾部等同于尾指针，end节点是指向beg下一个节点即NULL
    struct ListNode *beg = head;
	struct ListNode *end = head->next;
	while(end != NULL)//end=NULL逆置完成 
	{
		beg->next = end->next;//把end节点取出 
		//将 end 移动至链表头
		end->next = head;
		head = end;
		//调整 end 的指向，另其指向 beg 后的一个节点，为反转下一个节点做准备
		end = beg->next; 
	}
    return head;
}
// 迭代法
// 创建begin，mid，end三个指针，指定begin指向NULL，mid指向头节点，end指向第一个节点
// 随后先让mid指向begin，即倒置了mid和begin间指针的指向
// 然后迭代使得begin，mid，end都向下走一位，继续进行上一行的操作
// 当end=NULL时，已经来到尾节点，结束迭代，最后使头指针head与mid指针指向相同，即倒置尾节点
// void reverse1(linklist *head)
// {
// 	linklist *beg = NULL;
// 	linklist *mid = head;
// 	linklist *end = head->next;
// 	mid->next = beg;//mid指向beg 
// 	while(1)
// 	{
// 		if(end == NULL) break;
// 		beg = mid;
// 		mid = end;
// 		end = end->next;
// 	} 
// 	head = mid;	//最后head头指针的指向改为和mid指针相同。
// }

// 头插法
// 头插法就是把原链表的头节点一个一个单独取出来，接到新链表上形成新头节点，实现方向的逆转
// 因为取出的是原链表的头节点，插入形成的也是新链表的头节点，因此叫头插法*/
// linklist *reverse3(linklist *head)
// {
// 	linklist *new_head = NULL;//新链表头节点 
// 	linklist *temp = NULL;//临时节点 
// 	while(head != NULL)//head = NULL表示原列表所有节点都被取完了，即逆置完成 
// 	{
// 		temp = head;//取出原链表头节点，用temp储存 
// 		head = head->next;//原链表进一位 
// 		temp->next = new_head;
// 		temp = new_head;//把原链表头节点接入新链表，成为新头节点new_head 
// 	}
// 	return new_head;
// }
// 
```



#### 82.删除排序链表中的重复元素||（[82. 删除排序链表中的重复元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/)）

![](C:\Users\11484\Desktop\实验室\3g\每日一题\4.2\QQ截图20240402220953.png)

##### **code：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* deleteDuplicates(struct ListNode* head) {
    if (head == NULL || head->next == NULL) return head; //空链表或者只有一个
    struct ListNode *dummy = (struct ListNode*)malloc(sizeof(struct ListNode));
    dummy->next = head;//哑节点
    struct ListNode *pre = dummy;//前后指针创建
    struct ListNode *cur = head;
    while (cur != NULL) 
    {
        while (cur->next != NULL && cur->val == cur->next->val)//使cur指向重复部分尾部 
        {
            cur = cur->next;
        }
        if (pre->next == cur)//无重复
        {
            pre = pre->next;
        } 
        else 
        {
            pre->next = cur->next;//pre指向cur->next跳过cur，即跳过重复部分
        }
        cur = cur->next;
    }
    return dummy->next;
}
```



#### 200.岛屿数量（[200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/)）

![](C:\Users\11484\Desktop\实验室\3g\每日一题\4.2\QQ截图20240402222628.png)

**code：**

```c
int numIslands(char** grid, int gridSize, int* gridColSize) {
    int count = 0;
    for(int i = 0; i < gridSize; i++) {
        for(int j = 0; j < *gridColSize; j++) { 
            if(grid[i][j] == '1') { 
                dfs(grid, i, j, gridSize, gridColSize);//"1"岛屿进入搜索
                count++;
            }
        }
    }
    return count;
}

//dfs深搜
void dfs(char** grid, int i, int j, int gridSize, int* gridColSize) {
    if(i < 0 || i >= gridSize || j < 0 || j >= *gridColSize || grid[i][j] == '0') { //越界或者是海
        return;
    }
    grid[i][j] = '0'; //已搜索，状态改变
    //上下左右继续深入
    dfs(grid, i-1, j, gridSize, gridColSize);
    dfs(grid, i+1, j, gridSize, gridColSize);
    dfs(grid, i, j-1, gridSize, gridColSize);
    dfs(grid, i, j+1, gridSize, gridColSize);
}
```

