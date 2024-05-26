## 5月26日 Leetcode （陆辽旭）

#### 2285.道路的最大总重要性（[2285. 道路的最大总重要性 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-total-importance-of-roads/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240526195233.png)

##### code：

```go
func maximumImportance (n int, roads [][]int) int64 {
	dp := make([]int, n)
	for _, road := range roads {
		from, to := road[0], road[1]
		dp[from]++
		dp[to]++
	}
	sort.Ints(dp)
	res := int64(0)
    //贪心，连通数越多的城市越重要
	for i := n - 1; i >= 0; i-- {
		// 乘以当前的n，然后n自减
		res += int64(dp[i]) * int64(n)
		n--
	}
	return res
}
```





#### 1738.找出第K大的异或坐标值（[1738. 找出第 K 大的异或坐标值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-kth-largest-xor-coordinate-value/description/?envType=daily-question&envId=2024-05-26)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240526195346.png)

##### code：

```go
func kthLargestValue(matrix [][]int, k int) int {
    m, n := len(matrix), len(matrix[0]) 
    results := make([]int, 0, m*n) 
    pre := make([][]int, m+1) // 前缀和
    pre[0] = make([]int, n+1) 

    for i, row := range matrix {
        pre[i+1] = make([]int, n+1)
        for j, val := range row {
            // 计算当前元素的前缀异或和
            // pre[i+1][j+1] 表示从 (0,0) 到 (i,j) 的所有元素异或结果
            pre[i+1][j+1] = pre[i+1][j] ^ pre[i][j+1] ^ pre[i][j] ^ val
            // 将当前元素的异或结果添加到结果切片中
            results = append(results, pre[i+1][j+1])
        }
    }

    sort.Sort(sort.Reverse(sort.IntSlice(results)))
    return results[k-1]
}
```

