### 题目：[200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/description/)

![](https://younglion.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-28%20224547.png)
![](https://younglion.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-28%20224540.png)

### code

```java
class Solution {
    public int numIslands(char[][] grid) {
        //深搜版
        //主要思路：
        //1.遍历整个矩阵，一旦有1就岛屿res++，接着dfs（该点处的周围，遇到1就置0）；
        //2.置0的效果就是：他们之前在同一块岛屿上，未置0的位置就是下一块岛屿的某一点；
        //3.在dfs方法中：
        //  1）合法性判断：数组越界、当前位置处为0时退出；
        //  2) 传入位置处置0；
        //  3）dfs（该点的上下左右）；
        //因此：dfs的方法返回值为void，参数为矩阵（搜索对象）、坐标（位置）
        int res = 0;
        for(int i = 0;i<grid.length;i++){
            for(int j = 0;j<grid[0].length;j++){
                if(grid[i][j] == '1'){
                    res++;
                    dfs(grid,i,j);
                }
            }
        }
        return res;
    }

    public void dfs(char[][] grid,int i,int j){
        if(i<0||i>=grid.length||j<0||j>=grid[0].length||grid[i][j] == '0'){
            return;
        }

        grid[i][j] = '0';

        dfs(grid,i-1,j);
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);

    }
}
```

