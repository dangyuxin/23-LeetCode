## 5月21日 Leetcode （陆辽旭）

#### 2769.找出最大的可达成数字（[2769. 找出最大的可达成数字 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-maximum-achievable-number/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240521212142.png)

##### code：

```go
//leetcode try to find out who is retard
func theMaximumAchievableX(num int, t int) int {
    return num + t << 1
}
```





#### 973.最接近原点的k个点（[2769. 找出最大的可达成数字 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-maximum-achievable-number/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240521220504.png)

##### code：

```go
type pair struct {
    dist  int
    point []int
}
type hp []pair
//实现堆接口的方法
func (h hp) Len() int            { return len(h) }
func (h hp) Less(i, j int) bool  { return h[i].dist > h[j].dist }
func (h hp) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v interface{}) { *h = append(*h, v.(pair)) }//类型断言添加pair
func (h *hp) Pop() interface{}   { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }//队头出队

func kClosest(points [][]int, k int) (ans [][]int) {
    h := make(hp, k)
    for i, p := range points[:k] {
        h[i] = pair{p[0]*p[0] + p[1]*p[1], p}
    }
    heap.Init(&h) // O(k) 初始化堆
    for _, p := range points[k:] {
        if dist := p[0]*p[0] + p[1]*p[1]; dist < h[0].dist {//最大堆中比最大的0位小
            h[0] = pair{dist, p}
            heap.Fix(&h, 0) // 在0位维持最大堆
        }
    }
    for _, p := range h {
        ans = append(ans, p.point)
    }
    return
}
```

