## 5月12日 Leetcode （陆辽旭）

#### 1553.吃掉N个橘子的最少天数（[1553. 吃掉 N 个橘子的最少天数 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-number-of-days-to-eat-n-oranges/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240512194947.png)

##### code：

```go
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func dfs(n, x, k int, f *bool) {
    // 如果超过了k，直接返回
    if x > k {
        return
    }
    if n == 0 {
        *f = true
    }
    if *f {
        return
    }
    // 如果 n 小于等于 2，则继续搜索
    if n <= 2 {
        dfs(n-1, x+1, k, f)
    }
    // 分别尝试减去1、除以3、除以2，并继续搜索
    dfs(n/3, x+1+n%3, k, f)
    dfs(n/2, x+1+n%2, k, f)
}


func minDays(n int) int {
    if n <= 0 {
        return 0
    }
    f := false

    for i := 0; i < 200; i++ {
        dfs(n, 0, i, &f)
        if f {
            return i
        }
    }
    return 0
}
```





#### 198.打家劫舍（[198. 打家劫舍 - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240512212138.png)

##### code：

```go
func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}

func rob(nums []int) int {
    //特殊情况
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    
    dp := make([]int, len(nums))
    dp[0] = nums[0] 
    dp[1] = max(nums[0], nums[1]) // 第二个房屋的最高金额为第一个和第二个房屋中价值较高的那个

    for i := 2; i < len(nums); i++ {
        
        dp[i] = max(dp[i-2]+nums[i], dp[i-1])
    }
    return dp[len(nums)-1]
}
```

