## 4月6日 Leetcode （陆辽旭）

#### 71.简化路径（[71. 简化路径 - 力扣（LeetCode）](https://leetcode.cn/problems/simplify-path/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240406201933.png)

##### code：

```c
char * simplifyPath(char * path)
{
    char *stack[100];//栈，本题中返回目录是FILO
    int  size = 0;//栈深
    for(char *s = strtok(path, "/"); s != NULL;  s = strtok(NULL, "/"))//strok库函数分隔获得token，即每个/之间的字符串
    {
        if (strcmp(s, ".") == 0)//'.'直接略过
        {    
            continue;
        }
        else if(strcmp(s, "..") == 0)//'..'情况返回，即栈深-1
        {      
            size = fmax(0, size-1);
        }
        else
        {
            stack[size++] = s;//文件目录入栈
        }
    }
    //空目录情况
    if (size == 0) {
        return "/";
    }
    //分配结果字符串数组
    char *res = malloc(1000 * sizeof(char));
    if (res != NULL) {
        memset(res, 0, 1000 * sizeof(char));
    }
    for (int i = 0;i < size; ++i)
     {
         //每次返回/+栈内目录名
        strcat(res, "/");
        strcat(res, stack[i]);
    }
    return res;
}
```



#### 316.去除重复字母（[316. 去除重复字母 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicate-letters/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240406205131.png)

##### code：

```c
#define ALPHABET_SIZE 26
char* removeDuplicateLetters(char* s) {
    int num[ALPHABET_SIZE] = {0}; // 存储每个字母出现的次数
    int vis[ALPHABET_SIZE] = {0}; // 标记哪些字母已经添加到栈中

    // 计算每个字母出现的次数
    for (int i = 0; s[i] != '\0'; i++) {
        num[s[i] - 'a']++;
    }

    // 辅助栈，用于存放不会重复的字母
    char stk[ALPHABET_SIZE];
    int top = -1; // 栈顶指针，初始化为-1表示栈为空

    // 遍历字符串
    for (int i = 0; s[i] != '\0'; i++) {
        int ch = s[i] - 'a';
        if (!vis[ch]) { // 如果当前字母还没有被访问过
            // 贪心，如果栈不是空的，并且栈顶的字母字典序比现在的字母大，就出栈换小的字母进，否则就直接进栈
            while (top != -1 && stk[top] > s[i] && num[stk[top] - 'a'] > 0) {
                vis[stk[top] - 'a'] = 0; // 将栈顶元素标记为未访问
                top--; // 栈顶字母出栈
            }
            vis[ch] = 1; // 标记当前字母为已访问
            stk[++top] = s[i]; // 将当前字母加入栈中
        }
        num[ch]--; //
    }

    // 动态分配内存以存储新的字符串
    char* result = (char*)malloc((top + 2) * sizeof(char)); // 加1是为了存放字符串结束符'\0'
    // 将栈中的字符组合成新的字符串
    int j = 0;
    for (int i = 0; i <= top; i++) {
        result[j++] = stk[i];
    }
    result[j] = '\0'; // 添加字符串结束符
    return result; // 返回新字符串的地址
}
```

