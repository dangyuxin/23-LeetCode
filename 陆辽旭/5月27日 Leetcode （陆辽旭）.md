## 5月27日 Leetcode （陆辽旭）

#### 2028.找出缺失的观测数（[2028. 找出缺失的观测数据 - 力扣（LeetCode）](https://leetcode.cn/problems/find-missing-observations/description/?envType=daily-question&envId=2024-05-27)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240527202212.png)

##### code：

```go
//纯模拟
func missingRolls(rolls []int, mean int, n int) []int {
	ans := make([]int, 0)
    null := make([]int, 0)
	m := len(rolls)
	sum, pre := 0, 0
	for _, rool := range rolls {
		sum += rool
	}
	less := mean*(m+n) - sum
	for i := n; i > 1; i-- {//贪心最小公倍数，每次都更新
        pre = less / i
        if pre > 6 || pre < 1 {
            return null
        }
		ans = append(ans, pre)
		less -= pre
	}
	if less <= 6 && less >= 1 {
		ans = append(ans, less)
		return ans
	} else {
		return null
	}
}
```





#### 2662.前往目标的最小代价（[2662. 前往目标的最小代价 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-cost-of-a-path-with-special-roads/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240527212047.png)

##### code：

```go
type pair struct{ x, y int }

func minimumCost(start, target []int, specialRoads [][]int) int {
    end := pair{target[0], target[1]}
    // 定义dis映射，存储点到终点的距离
    dis := make(map[pair]int, len(specialRoads)+2)
    // 初始化终点到自身的距离为无穷大
    dis[end] = math.MaxInt
    // 初始化起点到自身的距离为0
    dis[pair{start[0], start[1]}] = 0
    // book用于记录已经访问过的点，初始化容量为特殊道路的数量加1（终点不用记录）
    book := make(map[pair]bool, len(specialRoads)+1)
    //Dijkstra
    for {
        // 初始化变量close_pair为pair{}，close_dis为-1，用于存储当前最近的点及其距离
        close_pair, close_dis := pair{}, -1
        // 遍历dis，找到未访问且距离最小的点
        for pair, distan := range dis {
            if !book[pair] && (close_dis < 0 || distan < close_dis) {
                close_pair, close_dis = pair, distan
            }
        }
        // 如果找到的最近点是终点，则返回其距离
        if close_pair == end {
            return close_dis
        }
        // 标记最近点close_pair为已访问
        book[close_pair] = true
        // 更新终点到自身的最短距离
        dis[end] = min(dis[end], close_dis + end.x - close_pair.x + end.y - close_pair.y)
        // 遍历特殊道路，尝试松弛
        for _, road := range specialRoads {
            // 特殊道路的新点坐标
            special_end := pair{road[2], road[3]}
            // 计算通过特殊道路到达新点的距离
            new_dis := close_dis + abs(road[0]-close_pair.x) + abs(road[1]-close_pair.y) + road[4]
            // 如果新点special_end尚未记录或者找到了更短的路径，则更新dis
            if pre_dis, ok := dis[special_end]; !ok || new_dis < pre_dis {
                dis[special_end] = new_dis
            }
        }
    }
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func min(a, b int) int {
    if b < a {
        return b
    }
    return a
}
```

