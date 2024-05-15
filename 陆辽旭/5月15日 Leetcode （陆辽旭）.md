## 5月15日 Leetcode （陆辽旭）

#### 2589.完成所有任务的最少时间（[2589. 完成所有任务的最少时间 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-time-to-complete-all-tasks/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240515183012.png)

##### code：

```go
//纯贪心解法
func findMinimumTime(tasks [][]int) int {
    slices.SortFunc(tasks, func(a, b []int) int { return a[1] - b[1] })//尾区间从小到大排序
    run := make([]int, tasks[len(tasks)-1][1]+1)//判断区间内是否有程序运行
    ans := 0
    for _, t := range tasks {
        start, end, duration := t[0], t[1], t[2]
        for _, b := range run[start : end+1] { // 去掉运行中的时间点
            if b > 0{
                duration--
            }
        }
        for i := end; duration > 0; i-- { // 剩余的 d 填充区间后缀，因为如果和下一个工作区间重合的话一定是在后缀重合
            if run[i] == 0 {//如果没有程序运行
                run[i] = 1;//转为运行
                duration--
                ans++
            }
        }
    }
    return ans
}
```





#### 959.由斜杠划分区域（[959. 由斜杠划分区域 - 力扣（LeetCode）](https://leetcode.cn/problems/regions-cut-by-slashes/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240515191033.png)

##### code：

```go
/*每个1x1单元格分隔成4个小格编号
    0
1       2
    3
斜杠/合并0,1和2,3
反斜杠\合并0,2和1,3
和左边单元格合并2,1
和下面单元格合并3,0
*/
type Union struct {//单元格
    parent []int//并查集
    count int//单元格个数
}

func initUnion(n int) *Union {
    p := make([]int, n)
    for i, _  := range p {
        p[i] = i
    }
    return &Union {
        parent : p,
        count : n,
    }
}

func (u *Union) find(x int) int {
    if u.parent[x] != x {
        // 递归地更新x的父节点为根节点
        u.parent[x] = u.find(u.parent[x])
    }
    return u.parent[x]
}

func (u *Union) merge(x , y int) {
    xp := u.find(x)
    yp := u.find(y)
    if xp == yp {
        return
    }
    u.parent[xp] = yp//x父等于y父，向上合并
    u.count--
}

func regionsBySlashes(grid []string) int {
    m := len(grid)
    n := len(grid[0])
    union := initUnion(m*n*4)//单元格初始化
    for i, _ := range grid {//遍历每个1x1小格
        for j, _ := range grid[i] {
            box := i*n +j//当前小格
            if grid[i][j] == '/' || grid[i][j] == ' ' {
				union.merge(box*4, box*4+1)
				union.merge(box*4+2, box*4+3)
			}
			if grid[i][j] == '\\' || grid[i][j] == ' ' {
				union.merge(box*4, box*4+3)
				union.merge(box*4+1, box*4+2)
			}
			//向右合并
			if j+1 < m {
				union.merge(box*4+2, (box+1)*4)
			}
			//向下合并
			if i+1 < n {
				union.merge(box*4+3, (box+n)*4+1)
			}
        }
    }
    return union.count
}
```

