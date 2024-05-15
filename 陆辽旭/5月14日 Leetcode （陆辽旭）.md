## 5月14日 Leetcode （陆辽旭）

#### 2244.完成所有任务需要的最少轮数（[2244. 完成所有任务需要的最少轮数 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-rounds-to-complete-all-tasks/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240514192930.png)

##### code：

```go
func minimumRounds(tasks []int) int {
    cnt := map[int]int{}//map记录次数
    for _, t := range tasks {
        cnt[t]++
    }
    res := 0
    for _, v := range cnt {
        if v == 1 {
            return -1
        } else if v % 3 == 0 {//贪心。可以模3就取3
            res += v / 3
        } else {
            res += v / 3 + 1//取二就是比取3多一次
        }
    }
    return res
}
```





#### 213.打家劫舍II（[213. 打家劫舍 II - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240514192946.png)

##### code：

```go
func rob1(nums []int, start, end int) int { // 函数用于计算给定范围内房屋数组的最大偷窃金额
    pre, cur := 0, 0 // 当前位置和前一个位置的最大偷窃金额
    for i := start; i < end; i++ { // 遍历房屋数组的指定范围
        pre, cur = cur, max(cur, pre+nums[i]) 
    }
    return cur // 现在最终最大偷窃金额
}

func rob(nums []int) int { 
    n := len(nums) 
    // 比较第一间房屋偷或不偷的最大值，第一间偷了就不能偷n-1间，范围是[1, n-1),第一间不偷是[2, n-1]
    return max(nums[0]+rob1(nums, 2, n-1), rob1(nums, 1, n)) 
}
```

