## 5月13日 Leetcode （陆辽旭）

#### 994.腐烂的橘子（[994. 腐烂的橘子 - 力扣（LeetCode）](https://leetcode.cn/problems/rotting-oranges/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240513190344.png)

##### code：

```go
func orangesRotting(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	q := [][2]int{}
	cnt := 0
	for i, row := range grid {
		for j, x := range row {
			if x == 1 {
				cnt++
			} else if x == 2 {
				q = append(q, [2]int{i, j}) //存放烂橘子位置切片
			}
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for ; len(q) > 0 && cnt > 0; ans++ {//ans表示经过分钟数，每过一次队列遍历加一，cnt表示新鲜橘子数，0表示完成
		for k := len(q); k > 0; k-- {//遍历队列每个元素
			p := q[0]                //取出队头
			q = q[1:]                //队头出队
			for d := 0; d < 4; d++ { //上下左右判断入队
				x, y := p[0]+dirs[d], p[1]+dirs[d+1]
				if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 {
					grid[x][y] = 2
					q = append(q, [2]int{x, y})
					cnt--//新鲜橘子数-1
				}
			}
		}
	}
	if cnt > 0 {
		return -1
	}
	return
}
```





#### 1094.拼车（[1094. 拼车 - 力扣（LeetCode）](https://leetcode.cn/problems/car-pooling/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240513195600.png)

##### code：

```go
func carPooling(trips [][]int, capacity int) bool {
    maxdest := 0 //最远到达距离
    for _, trip := range trips {
        maxdest = max(maxdest, trip[2])
    }
    person := make([]int, maxdest+1)//车上的人数变动情况
    for _, trip := range trips {
        person[trip[1]] += trip[0]//上车
        person[trip[2]] -= trip[0]//下车
    }
    count := 0 //车上人数
    for i := 0; i <= maxdest; i++ {
        count += person[i]
        if count > capacity {
            return false
        }
    }
    return true
}
```

