## 4月3日 Leetcode （陆辽旭）

#### 141.环形链表（[141. 环形链表 - 力扣](https://leetcode.cn/problems/linked-list-cycle/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403182949.png)

##### **code：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool hasCycle(struct ListNode *head) {
    //create fast and slow ptr
    struct ListNode *fast = head;
    struct ListNode *slow = head;
    while(fast != NULL && fast->next != NULL) {//for determining the cases of empty and single-element linklists
        //the fast ptr travels twice as fast as the slow ptr
        fast = fast->next->next;
        slow = slow->next;
        if(fast == slow) {//if the linklist is loop, two ptr will finaly encounter
            return 1;
        }
    }
    return 0;
}
```



#### 21.合并两个有序链表（[21. 合并两个有序链表 - 力扣](https://leetcode.cn/problems/merge-two-sorted-lists/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403183317.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* mergeTwoLists(struct ListNode* list1, struct ListNode* list2) {
    struct ListNode* dummy = malloc(sizeof(struct ListNode));//create dummy node
    dummy->next = NULL;
    struct ListNode* head = dummy;
    while (list1 != NULL && list2 != NULL) {
        if (list1->val <= list2->val) {//compare the val of two linklist, pick the small one put in new linklist
            dummy->next = list1;
            list1 = list1->next;
        } else {
            dummy->next = list2;
            list2 = list2->next;
        }
        dummy = dummy->next;
    }
    // handle remaining nodes
    if (list1 != NULL) {
        dummy->next = list1;
    } else {
        dummy->next = list2;
    }
    return head->next;
}
```



#### 876.链表的中间结点（[876. 链表的中间结点 - 力扣](https://leetcode.cn/problems/middle-of-the-linked-list/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403183915.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* middleNode(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {
        // Handle the case for empty list or single-element list
        return head;
    }
    //classic fast slow ptr
    struct ListNode *fast = head;
    struct ListNode *slow = head;
    while (fast != NULL && fast->next != NULL) {
        //when fast reach the tail, slow just right in the middle
        fast = fast->next->next;
        slow = slow->next;
    }
    return slow;
}
```



#### 234.回文链表（[234. 回文链表 - 力扣](https://leetcode.cn/problems/palindrome-linked-list/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403184716.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool isPalindrome(struct ListNode* head) {
    //first pick the val of each node and count its length, then put it into char arrays
    char num[100000];
    int length = 0;
    while (head != NULL) {
        num[length++] = head->val;
        head = head->next;
    }
    //determine if it is a palindrome linklist
    for (int i = 0, j = length - 1; i <= j; ++i, --j) {
        if (num[i] != num[j]) {
            return false;
        }
    }
    return true;
}
```



#### 160.相交链表（[160. 相交链表 - 力扣](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403193211.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    // traversal a linklist and change every nodes' val to minus
    struct ListNode *pA = headA;
    struct ListNode *pB = NULL;
    while (pA) {
        pA->val = -pA->val; 
        pA = pA->next;
    }

    // traversal b linklist find whether exsit minus val
    pA = headB;
    while (pA) {
        if (pA->val < 0) { // if minus val appear, means the node is encounter node
            pB = headA;
            while (pB) {
                pB->val = -pB->val; 
                pB = pB->next;
            }
            return pA;
        }
        pA = pA->next;
    }
    pB = headA;
    while (pB) {
        pB->val = -pB->val; 
        pB = pB->next;
    }
    return NULL;
}
```



#### 155.最小栈（[155. 最小栈 - 力扣](https://leetcode.cn/problems/min-stack/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403233249.png)

##### code：

```c
//a sub stack  that help to store the current min val
struct stack
{
    int val;
    int min;
};

typedef struct 
{
    //main stack with stack arrays
    struct stack stackDat[10000];
    int stackTop;
} MinStack;


MinStack* minStackCreate() 
{
    MinStack* newStack = (MinStack *) malloc(sizeof(MinStack));
    newStack->stackTop = 0;
    return newStack;
}

void minStackPush(MinStack* obj, int val) 
{
    //push on top of stack
    obj->stackDat[obj->stackTop].val = val;
    //refresh the min
    if(!obj->stackTop || val <= obj->stackDat[obj->stackTop-1].min )
    {
        obj->stackDat[obj->stackTop].min = val;
    }
    else
    {
        obj->stackDat[obj->stackTop].min = obj->stackDat[obj->stackTop-1].min;
    }
    obj->stackTop++;
}

void minStackPop(MinStack* obj)
{
    obj->stackTop--;
}

int minStackTop(MinStack* obj) 
{
    //when access top, stackTop need to minus one
    return obj->stackDat[obj->stackTop-1].val; 
}

int minStackGetMin(MinStack* obj) 
{
    return obj->stackDat[obj->stackTop-1].min; 
}

void minStackFree(MinStack* obj) 
{
    free(obj);
}
```



#### 81.搜索旋转排序数组||（[81. 搜索旋转排序数组 II - 力扣](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240403205824.png)

##### code：

```c
int binarySearch(int *nums,int low,int high,int key) {
    while(low<=high) {
        int mid = (low+high)/2;
        if(nums[mid] == key) {
            return mid;
        }
        else if(nums[mid] < key) {
            low = mid+1;
        }
        else {
            high = mid-1;
        }
    }
    return -1;
}

bool search(int* nums, int numsSize, int target) {
    //excluding cases of small size 
    if(numsSize == 1) return nums[0] == target;
    if(numsSize == 2) return nums[0] == target || nums[1] == target;
    if(numsSize == 3) return nums[0] == target || nums[1] == target || nums[2] == target;
    if(numsSize == 4) return nums[0] == target || nums[1] == target || nums[2] == target || nums[3] == target;
    if(numsSize == 5) return nums[0] == target || nums[1] == target || nums[2] == target || nums[3] == target || nums[4] == target;
    int i;
    for (i = 1; i < numsSize - 1; i++) {
        if (nums[i] < nums[i - 1]) { // find a min point, in which can consider as the pivot
            break;
        }
    }
    if (nums[i] == target) return true; // first judge i
    int left = binarySearch(nums, 0, i - 1, target); // search left
    int right = binarySearch(nums, i, numsSize - 1, target); // search right
    if (left != -1 || right != -1) {
        return true;
    }
    return false;
}
```

