## 6月2日 Leetcode （陆辽旭）

#### 575.分糖果（[575. 分糖果 - 力扣（LeetCode）](https://leetcode.cn/problems/distribute-candies/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240602183026.png)

##### code：

```go
func distributeCandies(candyType []int) int {
    n := len(candyType) / 2
    candyMap := make(map[int]bool)
    for _, candy := range candyType {
        candyMap[candy] = true
    }
    kinds := len(candyMap)
    return min(kinds, n)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```





#### 2101.引爆最多的炸弹（[2101. 引爆最多的炸弹 - 力扣（LeetCode）](https://leetcode.cn/problems/detonate-the-maximum-bombs/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240602185701.png)

##### code：

```go
func maximumDetonation(bombs [][]int) (ans int) {
	n := len(bombs)
	g := make([][]int, n)
	for i, p := range bombs {
		for j, q := range bombs {
			if j != i && (q[0]-p[0])*(q[0]-p[0])+(q[1]-p[1])*(q[1]-p[1]) <= p[2]*p[2] {
				g[i] = append(g[i], j) // 有向图，从 i 向 j 连边，表示引爆 i 可以引爆 j，类似并查集
			}
		}
	}
	for i := range g { // 暴力枚举所有起点，跑 DFS 看能访问到多少节点
		vis := make([]bool, n)
		cnt := 0
		var dfs func(int)
		dfs = func(v int) {
			vis[v] = true
			cnt++
			for _, w := range g[v] {
				if !vis[w] {//连通且没有被访问过
					dfs(w)
				}
			}
		}
		dfs(i)
		if cnt > ans {
			ans = cnt
		}
	}
	return
}
```





#### 1631.最小体力消耗路径（[1631. 最小体力消耗路径 - 力扣（LeetCode）](https://leetcode.cn/problems/path-with-minimum-effort/description/)）

![]()![QQ截图20240602195155](https://gitee.com/knoci/picture/raw/master/QQ截图20240602195155.png)

##### code：

```go
type point struct{ x, y, maxDiff int }
type hp []point
func (h hp) Len() int              { return len(h) }
func (h hp) Less(i, j int) bool    { return h[i].maxDiff < h[j].maxDiff }
func (h hp) Swap(i, j int)         { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v interface{})   { *h = append(*h, v.(point)) }
func (h *hp) Pop() (v interface{}) { a := *h; *h, v = a[:len(a)-1], a[len(a)-1]; return }

type pair struct{ x, y int }
var dir4 = []pair{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

func minimumEffortPath(heights [][]int) int {
    n, m := len(heights), len(heights[0])
    // maxDiff 存储从起点到每个点的最大高度差，相当于Dijkstra的dis数组
    maxDiff := make([][]int, n)
    for i := range maxDiff {
        maxDiff[i] = make([]int, m)
        for j := range maxDiff[i] {
            maxDiff[i][j] = math.MaxInt64
        }
    }
    maxDiff[0][0] = 0
    h := &hp{{}}
    for {
        p := heap.Pop(h).(point)// 从堆中取出当前消耗体力最小的点
        //最小堆保证到达终点时p.maxDiff最小
        if p.x == n-1 && p.y == m-1 {
            return p.maxDiff
        }
        //当前位置已更新过最少消耗，跳过
        if maxDiff[p.x][p.y] < p.maxDiff {
            continue
        }
        for _, d := range dir4 {
            if x, y := p.x+d.x, p.y+d.y; 0 <= x && x < n && 0 <= y && y < m {
                if diff := max(p.maxDiff, abs(heights[x][y]-heights[p.x][p.y])); diff < maxDiff[x][y] {
                    maxDiff[x][y] = diff
                    heap.Push(h, point{x, y, diff})
                }
            }
        }
    }
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

