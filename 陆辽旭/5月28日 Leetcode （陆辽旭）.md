## 5月28日 Leetcode （陆辽旭）

#### 2951.找出峰值（[2951. 找出峰值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-peaks/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240528183334.png)

##### code：

```go
func findPeaks(mountain []int) []int {
    ans := make([]int, 0)
    len := len(mountain)
    if len < 3 {
        return ans
    }
    for i := 1; i < len -1; i++ {
        if mountain[i] > mountain[i-1] && mountain[i] > mountain[i+1] {
            ans =append(ans, i)
            i++//跳过下一个
        }
    }
    return ans
}
```





#### 1584.连接所有点的最小费用（[1584. 连接所有点的最小费用 - 力扣（LeetCode）](https://leetcode.cn/problems/min-cost-to-connect-all-points/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240528193229.png)

##### code：

```go
type edge struct {
	start, end int
	cost       int
}

type unionFind struct {
	parent []int
	rank   []int //深度
}

// 初始化并查集
func newUnionFind(n int) *unionFind {
	uf := &unionFind{parent: make([]int, n), rank: make([]int, n)}
	for i := range uf.parent {
		uf.parent[i] = i
	}
	return uf
}

func (uf *unionFind) find(x int) int {
	if uf.parent[x] != x {
		uf.parent[x] = uf.find(uf.parent[x])
	}
	return uf.parent[x]
}

// 合并两个点，深度大的优先
func (uf *unionFind) union(x, y int) {
	xRoot, yRoot := uf.find(x), uf.find(y)
	if xRoot == yRoot {
		return
	}
	if uf.rank[xRoot] < uf.rank[yRoot] {
		uf.parent[xRoot] = yRoot
	} else if uf.rank[xRoot] > uf.rank[yRoot] {
		uf.parent[yRoot] = xRoot
	} else {
		uf.parent[yRoot] = xRoot
		uf.rank[xRoot]++
	}
}

// 检查两个点是否已经连接
func (uf *unionFind) connected(x, y int) bool {
	return uf.find(x) == uf.find(y)
}

func abs(x, y int) int {
	if x < y {
		return y - x
	}
	return x - y
}

func distance(points [][]int, a int, b int) int {
	return abs(points[a][0], points[b][0]) + abs(points[a][1], points[b][1])
}

//Kruskal算法
func minCostConnectPoints(points [][]int) int {
	n := len(points)
	if n < 2 {
		return 0
	}
	// 创建并查集
	uf := newUnionFind(n)
	// 存储所有边按成本排序
	edges := []edge{}
	for i := 0; i < n-1; i++ {
		for j := i + 1; j < n; j++ {
			edges = append(edges, edge{start: i, end: j, cost: distance(points, i, j)})
		}
	}
	// 按成本对边进行排序
	sort.Slice(edges, func(i, j int) bool {return edges[i].cost < edges[j].cost})
	cost := 0
	for _, e := range edges {
		if !uf.connected(e.start, e.end) {
			uf.union(e.start, e.end)
			cost += e.cost
		}
	}
	return cost
}
```

