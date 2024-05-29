## 5月23日 Leetcode （陆辽旭）

#### 2831.找出最长等值子数列（[2831. 找出最长等值子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-longest-equal-subarray/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240523185258.png)

##### code：

```go
func longestEqualSubarray(nums []int, k int) (ans int) {
    posLists := make([][]int, len(nums)+1)
    for i, x := range nums {
        // 将当前元素的索引 i 减去 posLists[x] 当前的长度，然后追加到 posLists[x] 中，除去相同的数
        posLists[x] = append(posLists[x], i-len(posLists[x]))
    }
    for _, pos := range posLists {
        if len(pos) <= ans {
            continue
        }
        left := 0
        // 遍历 pos 中的每个元素 p 和它的索引 right
        for right, p := range pos {
            // 当前窗口中需要删除的元素数量超过 k 时，移动 left 指针
            // 直到窗口中需要删除的元素数量不超过 k
            for p-pos[left] > k {
                left++
            }
            // 更新 ans 为 right-left+1 和当前 ans 中的较大值
            ans = max(ans, right-left+1)
        }
    }
    return
}
```





#### 1514.概率最大的路径（[1514. 概率最大的路径 - 力扣（LeetCode）](https://leetcode.cn/problems/path-with-maximum-probability/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240523195321.png)

##### code：

```go
//存储节点
type Node struct {
	x        int
	distance float64
}
//实现堆
type hp []Node
func (h hp) Len() int              { return len(h) }
func (h hp) Less(i, j int) bool    { return h[i].distance > h[j].distance }
func (h hp) Swap(i, j int)         { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v interface{})   { *h = append(*h, v.(Node)) }
func (h *hp) Pop() (v interface{}) { a := *h; *h, v = a[:len(a)-1], a[len(a)-1]; return }

func maxProbability(n int, edges [][]int, succProb []float64, start int, end int) float64 {
	graph := make([][]Node, n)
    //一个二维Node切片，行代表起点，列是相接的所有节点和距离的切片
	for i, ev := range edges {
		graph[ev[0]] = append(graph[ev[0]], Node{x: ev[1], distance: succProb[i]})
		graph[ev[1]] = append(graph[ev[1]], Node{x: ev[0], distance: succProb[i]})
	}

	res := dijkstra(n, start, graph)
	return res[end]
}
//优先队列Dijkstra
func dijkstra(n int, start int, graph [][]Node) []float64 {
	res := make([]float64, n)//概率结果数组
    //初始化优先队列
	q := &hp{}
	heap.Init(q)
	heap.Push(q, Node{x: start, distance: 1})
    //把到自身概率初始化百分百
	res[start] = 1
	for q.Len() > 0 {
        //队头出队，如果结果概率大于当前节点概率，不用松弛直接跳过，Dijkstra的贪心体现
		cur := heap.Pop(q).(Node)
		if cur.distance < res[cur.x] {
			continue
		}
        //节点概率更大，尝试所有边松弛是否大于结果概率
		for _, gv := range graph[cur.x] {
			x := gv.x//相邻节点
			d := gv.distance//相邻接单概率
			if d*cur.distance > res[x] {//如果松弛成功，修改结果数组并入队
				res[x] = d * cur.distance
				heap.Push(q, Node{x: x, distance: res[x]})
			}
		}
	}
	return res
}
```

