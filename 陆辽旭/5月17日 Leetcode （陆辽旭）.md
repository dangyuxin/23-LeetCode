## 5月17日 Leetcode （陆辽旭）

#### 826.安排工作以达到最大收益（[826. 安排工作以达到最大收益 - 力扣（LeetCode）](https://leetcode.cn/problems/most-profit-assigning-work/description/?envType=daily-question&envId=2024-05-17)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240517162110.png)

##### code：

```go
type work struct {
	difficulty int
	profit     int
}

func getmoney(w []work, capability int) int {
    maxProfit := 0
	for i := 0; i < len(w) && w[i].difficulty <= capability; i++ {
        maxProfit = max(maxProfit, w[i].profit)
    }
    return maxProfit
}

func maxProfitAssignment(difficulty []int, profit []int, worker []int) int {
	workSlice := make([]work, len(difficulty))
	for i := range difficulty {
		workSlice[i] = work{difficulty[i], profit[i]}
	}
	sort.Slice(workSlice, func(i, j int) bool { return workSlice[i].difficulty < workSlice[j].difficulty })

	sum := 0
	for _, capability := range worker {
		if capability < workSlice[0].difficulty {
			continue
		}
		sum += getmoney(workSlice, capability)
	}
	return sum
}
```





#### 287.寻找重复数（[287. 寻找重复数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-duplicate-number/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240517164039.png)

##### code：

```go
func findDuplicate(nums []int) int {
	left, right := 1, len(nums)-1

	getCnt := func(n int) (cnt int) {
		for i := range nums {
			if nums[i] <= n {
				cnt++
			}
		}
		return cnt
	}

	for left <= right {
		mid := left + (right-left)>>1
		if getCnt(mid) <= mid { //cnt[i]<=i
			left = mid + 1
		} else { //cnt[i]>i
			/*
				1. 当cnt[i]>i时,说明target还有可能在i的左边
				2. 当i已经是最左边或者cnt[i-1]已经不能满足cnt[i-1]>i-1而是
				cnt[i-1]<=i-1时,说明当前的i就是要求的target
			*/
			if mid == 1 || getCnt(mid-1) <= mid-1 {
				return mid
			}
			right = mid - 1
		}
	}
	return -1
}
```

