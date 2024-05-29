## 5月29日 Leetcode （陆辽旭）

#### 1976.到达目的地的方案数（[1976. 到达目的地的方案数 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-ways-to-arrive-at-destination/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240529200134.png)

##### code：

```go
func countPaths(n int, roads [][]int) int {
    const mod = int64(1e9 + 7)
	e := make([][]Edge, n)
	for _, road := range roads {
		x, y, t := road[0], road[1], road[2]
		e[x] = append(e[x], Edge{y, t})
		e[y] = append(e[y], Edge{x, t})
	}
    //dis是源点到各个点距离
	dis := make([]int64, n)
	for i := range dis {
		dis[i] = math.MaxInt64
	}
    //ways表示源点到各个点最短路径数量
	ways := make([]int64, n)
	q := PriorityQueue{{0, 0}}
	heap.Init(&q)
	dis[0] = 0
	ways[0] = 1
	//优先队列实现Dijkstra
	for len(q) > 0 {
		p := heap.Pop(&q).(Pair)
		t, u := p.precost, p.curpoint//取出队头的pair，得到现在的点和源点到该点的花费
		if t > dis[u] {//当前花费比确定花费大
			continue
		}
		for _, edge := range e[u] {//遍历现在点的所有边尝试松弛
			v, w := edge.to, edge.cost
			if t + int64(w) < dis[v] {
				dis[v] = t + int64(w)//更新确定花费
				ways[v] = ways[u]//松弛成功，相邻点最短路数量等于到现在点最短路数量
				heap.Push(&q, Pair{t + int64(w), v})//松弛成功加入优先队列
			} else if t + int64(w) == dis[v] {
				ways[v] = (ways[u] + ways[v]) % mod//相等表示都是最短路，相加
			}
		}
	}
	return int(ways[n - 1])
}

type Edge struct {
    to   int
    cost int
}

type Pair struct {
    precost int64
    curpoint int
}

type PriorityQueue []Pair

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq PriorityQueue) Len() int {
    return len(pq)
}

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].precost < pq[j].precost
}

func (pq *PriorityQueue) Push(x any) {
    *pq = append(*pq, x.(Pair))
}

func (pq *PriorityQueue) Pop() any {
    n := len(*pq)
    x := (*pq)[n - 1]
    *pq = (*pq)[:n-1]
    return x
}
```





#### 2981.找出出现至少三次的最长特殊字符串I（[2981. 找出出现至少三次的最长特殊子字符串 I - 力扣（LeetCode）](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-i/description/)）

![](https://gitee.com/knoci/picture/raw/master/5.29.png)

##### code：

```go
func maximumLength(s string) int {
	n := len(s)
	//判断是否有长度为k的重复串
	judge := func(k int) bool {
		res := make([]int, 26)
		m := make(map[byte]int)
        //维系一个k长度的滑动窗口
		for i := 0; i < k; i++ {
			m[s[i]]++
		}
		if m[s[0]] == k {
			res[s[0]-'a']++
		}
		for i := 0; i < n-k; i++ {
			m[s[i]]--
			m[s[i+k]]++
			if m[s[i+k]] == k {
				res[s[i+k]-'a']++
				if res[s[i+k]-'a'] == 3 {//长度k的重复串出现三次
					return true
				}
			}
		}
		return false
	}

	l, r := 0, n
	for l <= r {
		m := (l + r) / 2
		if judge(m) { // 调用 judge 函数判断当前长度是否满足条件
			l = m + 1 // 如果满足条件，向右缩小范围
		} else {
			r = m - 1 // 如果不满足条件，向左缩小范围
		}
	}
	if r > 0 {
		return r
	}
	return -1
}
```

