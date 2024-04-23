## 4月7日 Leetcode （陆辽旭）

#### 394.字符串解码（[394. 字符串解码 - 力扣（LeetCode）](https://leetcode.cn/problems/decode-string/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240407193925.png)

##### code：

```c
char * decodeString(char * s){
int len = (int)strlen(s);
    int stackSize = 50;//栈的原始尺寸
    char* stack = (char*)malloc(stackSize * sizeof(char));//动态分配
    int top = -1;
    for (int i=0; i<len; i++) {
        char c = s[i];
        if (c != ']') {//如果字符不是]则一直入栈
            if (top==stackSize-1) {//如果此时栈满了 就扩容
                stack = realloc(stack, (stackSize += 50) * sizeof(char));
            }
            stack[++top] = c;
        } else {
            //创建临时数组 存放要复制的字符
            int tempSize = 10;
            char* tempStack = (char*)malloc(tempSize * sizeof(char));
            int tempTop = -1;
            //出栈[之前的数据，即得到要复制的字符串
            while (stack[top] != '[') {
                if (tempTop==tempSize-1) {
                    tempStack = realloc(tempStack, (tempSize += 10)*sizeof(char));
                }
                //栈顶元素存入临时数组，顺序变为正序
                tempStack[++tempTop] = stack[top];
                //stack 出栈,最后top指向[
                top--;
            }
            //得到要复制的字符串
            char *cpyStr = (char*)malloc((tempTop+2)*sizeof(char));
            for (int i=tempTop; i>=0; i--) {
                cpyStr[tempTop-i] = tempStack[i];
            }
            cpyStr[++tempTop] = '\0';
            //'['出栈
            top--;
            //取出需要复制的数量
            int cpyNum = 0;//数字
            int j = 0;//位数
            while (top>=0 && stack[top]>='0' && stack[top]<='9') {
                cpyNum += (stack[top]-'0')*pow(10, j);
                top--;
                j++;
            }
            //复制的字符重新入栈
            for (int i=0; i<cpyNum; i++) {
                for (int j=0; j<strlen(cpyStr); j++) {
                    if (top==stackSize-1) {//如果此时栈满了 就扩容
                        stack = realloc(stack, (stackSize += 50) * sizeof(char));
                    }
                    stack[++top] = cpyStr[j];//最后结果字符入栈
                }
            }
            free(tempStack);
        }
    }
    char* result = realloc(stack, (top + 2) * sizeof(char));
    result[++top] = '\0';
    return result;
}
```



#### 496.下一个更大元素([496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/description/))

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240407202406.png)

##### code:

```c
struct hash {
    int key;
    int value;
    UT_hash_handle hh;
};

int* nextGreaterElement(int* nums1, int nums1Size, int* nums2, int nums2Size, int* returnSize)
{
    struct hash *hashtable = NULL; // 初始表需要 = NULL
    int stack[nums2Size]; // 声明单调栈
    int top = -1; // 声明和初始化单调栈顶指针
    // 一次遍历nums2数组
    for (int i = 0; i < nums2Size; i++) {
        if (top == -1) { // 如果是空栈，直接入栈
            stack[++top] = nums2[i];
            continue;
        }
        if (nums2[i] < stack[top]) { // 如果取值小于栈顶数值，也直接入栈
            stack[++top] = nums2[i];
            continue;
        } 
        // 只要取值一直大于栈顶数值，其栈顶数值出栈并和取值一同加入hash表中
        while (top > -1 && nums2[i] > stack[top]) { 
            struct hash *tmp = malloc(sizeof(struct hash));
            tmp->key = stack[top];
            tmp->value = nums2[i];
            HASH_ADD_INT(hashtable, key, tmp);
            top--;
        }
        stack[++top] = nums2[i]; // while出来后的取值还得进行入栈操作
    }
    int *res = malloc(sizeof(int) * nums1Size);
    // 一次遍历nums1数组
    for (int i = 0; i < nums1Size; i++) {
        struct hash *tmp;
        HASH_FIND_INT(hashtable, nums1 + i, tmp);//取出对应位置的hash表
        if (tmp) { // 只要在hash表中，那就有对应的value值
            res[i] = tmp->value;
        } else { // 没在hash表中，赋值-1
            res[i] = -1;
        }
    }
    *returnSize = nums1Size;
    return res;
}
```

