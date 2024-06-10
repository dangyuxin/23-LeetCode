## 5月30日 Leetcode （陆辽旭）

#### 734.网络延迟时间（[743. 网络延迟时间 - 力扣（LeetCode）](https://leetcode.cn/problems/network-delay-time/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240530200651.png)

##### code：

```go
type Edge struct {
	from int
	to   int
	cost int
}

type PriorityQueue []Edge

func (pq PriorityQueue) Len() int           { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].cost < pq[j].cost }
func (pq PriorityQueue) Swap(i, j int)      { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) {
	*pq = append(*pq, x.(Edge))
}

func (pq *PriorityQueue) Pop() interface{} {
	n := len(*pq)
	x := (*pq)[n-1]
	*pq = (*pq)[:n-1]
	return x
}

func networkDelayTime(times [][]int, n int, k int) int {
	e := make([][]Edge, n+1) // 邻接矩阵存边，节点编号从1开始，所以大小为n+1
	for _, time := range times {
		x, y, t := time[0], time[1], time[2]
		e[x] = append(e[x], Edge{x, y, t})
	}

	dis := make([]int, n)
	for i := range dis {
		dis[i] = math.MaxInt32
	}

	q := PriorityQueue{{from: k, to: k, cost: 0}} // 从节点k开始
	heap.Init(&q)
	dis[k-1] = 0 // 起始点到自身的距离设为0

	for len(q) > 0 {
		node := heap.Pop(&q).(Edge) // 当前最接近的点
		u := node.to
		for _, edge := range e[u] {
			v, w := edge.to, edge.cost
			if dis[u-1]+w < dis[v-1] { // 尝试更新距离
				dis[v-1] = dis[u-1] + w
				heap.Push(&q, Edge{from: u, to: v, cost: dis[v-1]}) // 将新的节点加入队列
			}
		}
	}

	max := 0
	for _, data := range dis { // 从索引1开始遍历，忽略起始点
		if data == math.MaxInt32 {
			return -1 // 有节点无法到达
		}
		if data > max {
			max = data
		}
	}
	return max
}
```

