## 4月5日 Leetcode （陆辽旭）

#### 255.用队列实现栈（[225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240405210437.png)

##### code：

```c
// 定义栈结构体MyStack
typedef struct {
    int myQueuedat[101]; // 存储队列数据的数组
    int myQueueFront;    // 主队列的头部指针
    int myQueueRear;     // 主队列的尾部指针
    int dummyQueuedat[101]; // 存储临时数据的数组
    int dummyQueueFront;  // 临时队列的头部指针
    int dummyQueueRear;   // 临时队列的尾部指针
} MyStack;

MyStack* myStackCreate() {
    MyStack* newStack = (MyStack*)malloc(sizeof(MyStack));
    if (newStack == NULL) {
        return NULL; // 如果内存分配失败，返回NULL
    }
    newStack->dummyQueueFront = 0;
    newStack->dummyQueueRear = 0;
    newStack->myQueueFront = 0;
    newStack->myQueueRear = 0;
    return newStack; // 返回新创建的栈对象
}

bool myStackEmpty(MyStack* obj) {
    return obj->myQueueFront == obj->myQueueRear; // 头尾指针相等则栈为空
}

void myStackPush(MyStack* obj, int x) {
    if (obj == NULL) {
        return;
    }
    // 如果主队列为空，则直接将元素添加到队尾
    if (myStackEmpty(obj)) {
        obj->myQueuedat[obj->myQueueRear++] = x;
    } else {
        // 否则，使用临时队列来实现队列的出队操作，从而实现栈的后进先出特性
        obj->dummyQueueFront = 0;
        obj->dummyQueueRear = 0;
        while (obj->myQueueFront < obj->myQueueRear) {
            obj->dummyQueuedat[obj->dummyQueueRear++] = obj->myQueuedat[obj->myQueueFront++];
        }
        // 此时，dummyQueue中存储了myQueue中的所有元素，且顺序为后进先出
        // 现在将新元素添加到myQueue的队尾
        obj->myQueueFront = 0;
        obj->myQueueRear = 0;
        obj->myQueuedat[obj->myQueueRear++] = x;
        // 再次将dummyQueue中的元素出队并添加到myQueue中，完成后进先出的操作
        while (obj->dummyQueueFront < obj->dummyQueueRear) {
            obj->myQueuedat[obj->myQueueRear++] = obj->dummyQueuedat[obj->dummyQueueFront++];
        }
        // 清空dummyQueue
        obj->dummyQueueFront = 0;
        obj->dummyQueueRear = 0;
    }
}

int myStackPop(MyStack* obj) {
    if (myStackEmpty(obj)) {
        return -1; // 如果栈为空，返回-1
    }
    int value = obj->myQueuedat[obj->myQueueFront++];
    if (obj->myQueueFront == obj->myQueueRear) {
        // 如果出栈后栈为空，则重置头尾指针
        obj->myQueueFront = 0;
        obj->myQueueRear = 0;
    }
    return value;
}

int myStackTop(MyStack* obj) {
    if (myStackEmpty(obj)) {
        return -1; 
    }
    return obj->myQueuedat[obj->myQueueFront];
}


void myStackFree(MyStack* obj) {
    if (obj != NULL) {
        free(obj);
    }
}
```



#### 232.用栈实现队列（[232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240405210628.png)

##### code：

```c
// 定义队列结构体MyQueue
typedef struct {
    int top;             // 栈顶指针
    int dummyTop;       // 哑栈顶指针
    int stack[1001];     // 主栈，用于存放实际数据
    int dummyStack[1001]; // 哑栈，用于辅助操作
} MyQueue;

MyQueue* myQueueCreate() {
    MyQueue* myQueue = (MyQueue*)malloc(sizeof(MyQueue));
    if (myQueue == NULL) {
        return NULL; // 内存分配失败
    }
    myQueue->top = -1;          // 初始化栈顶指针为-1，表示队列为空
    myQueue->dummyTop = -1;    // 初始化哑栈顶指针为-1
    return myQueue;             // 返回新创建的队列
}

void myQueuePush(MyQueue* obj, int x) {
    if (obj == NULL) {
        return; // 如果队列为空，直接返回
    }
    // 如果主栈为空，则直接将元素压入主栈
    if (obj->top == -1) {
        obj->stack[++obj->top] = x;
    } else {
        // 否则，先将主栈中的所有元素转移到哑栈中
        obj->dummyTop = -1; // 重置哑栈顶指针
        while (obj->top != -1) {
            obj->dummyStack[++obj->dummyTop] = obj->stack[obj->top--];
        }
        // 将新元素压入主栈
        obj->stack[++obj->top] = x;
        // 再将哑栈中的元素移回主栈，实现队列的FIFO特性
        while (obj->dummyTop != -1) {
            obj->stack[++obj->top] = obj->dummyStack[obj->dummyTop--];
        }
    }
}

int myQueuePop(MyQueue* obj) {
    if (obj == NULL || obj->top == -1) {
        return -1;
    }
    int value = obj->stack[obj->top--]; // 获取队列前端的元素，并弹出
    return value;
}

int myQueuePeek(MyQueue* obj) {
    if (obj == NULL || obj->top == -1) {
        return -1; 
    }
    return obj->stack[obj->top]; // 返回队列前端的元素
}


bool myQueueEmpty(MyQueue* obj) {
    return obj->top == -1; 
}

void myQueueFree(MyQueue* obj) {
    if (obj != NULL) {
        free(obj); // 释放队列内存
    }
}
```



#### 25.k个一组翻转链表（[25. K 个一组翻转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240405210808.png)

##### code：

```c
struct ListNode* reverseList(struct ListNode* head, struct ListNode* tail);

struct ListNode* reverseKGroup(struct ListNode* head, int k) {
    struct ListNode* start = head;
    struct ListNode* end = head;
    struct ListNode* next_group = NULL;
    int count = 0;
    
    // 遍历链表，找到下一个分组的起始节点
    while (end != NULL && count < k) {
        end = end->next;
        count++;
    }
    
    // 如果当前分组节点数量不足 k 个，则直接返回 head
    if (count < k) {
        return head;
    }
    
    // 反转当前分组
    struct ListNode* new_head = reverseList(start, end);
    
    // 递归反转剩余节点，并将结果连接到当前分组的末尾
    start->next = reverseKGroup(end, k);
    
    return new_head;
}

// 反转链表函数
struct ListNode* reverseList(struct ListNode* head, struct ListNode* tail) {
    struct ListNode* prev = NULL;
    struct ListNode* curr = head;
    while (curr != tail) {
        struct ListNode* next_temp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next_temp;
    }
    return prev;
}
```

