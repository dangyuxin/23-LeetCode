## 5月16日 Leetcode （陆辽旭）

#### 1953.你可以工作的最大周数（[1953. 你可以工作的最大周数 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-number-of-weeks-for-which-you-can-work/description/?envType=daily-question&envId=2024-05-16)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240516191734.png)

##### code：

```go
/*
本质是一道数学题，因为不能连续做相同任务，因此每两周都是不同任务(废话)
所以两周的任务，我们可以取最大值和其余任意一个值，交替工作后，任意值为0了
这个过程我们可以看做"抵消"最大值，如果最大值被其余值抵消到小于1，那么就可以完成所有工作
反之，如果其余值不能抵消最大值，返回其余值之和*2+1
*/
type IntSlice []int
func (p IntSlice) Len() int           { return len(p) }
func (p IntSlice) Less(i, j int) bool { return p[i] > p[j] }
func (p IntSlice) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }

func numberOfWeeks(milestones []int) int64 {
    if len(milestones) == 0 || milestones == nil {
        return int64(0)
    }
    if len(milestones) == 1 {
        return 1
    }
	sum := 0
    sort.Sort(IntSlice(milestones))
	for _, x := range milestones {
		sum += x
	}
	if milestones[0] > sum-milestones[0]+1 {
		return int64((sum-milestones[0])*2 + 1)
	}
	return int64(sum)
}
```





#### 128.最长连续子序列（[128. 最长连续序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-consecutive-sequence/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240516194600.png)

##### code：

```go
func longestConsecutive(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    numSet := make(map[int]bool)
    for _, num := range nums {
        numSet[num] = true 
    }
    // 初始化最长连续序列的长度为1，因为即使数组中只有一个数字，也算是长度为1的连续序列。
    longestStreak := 1
    for num := range numSet {
        // 如果前一个数不在哈希，说明当前是起点。
        if !numSet[num-1] {
            // 从当前数字开始，向前查找连续的序列。
            currentNum := num
            currentStreak := 1 // 当前连续序列的长度初始化为1，包括当前数字。
            for numSet[currentNum+1] {
                currentNum++
                currentStreak++
            }
            longestStreak = max(longestStreak, currentStreak)
        }
    }
    // 返回找到的最长连续序列的长度。
    return longestStreak
}
```





#### 922.按奇偶顺序排序数组II（[922. 按奇偶排序数组 II - 力扣（LeetCode）](https://leetcode.cn/problems/sort-array-by-parity-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240516195315.png)

##### code：

```go
func sortArrayByParityII(nums []int) []int {
    // 原地修改，偶数下标遇上奇数，从奇数位调换，直到该位置是偶数为止
	oddIdx := 1
	// 遍历所有的偶数位
	for i:=0;i<len(nums);i+=2 {
		// 偶数位上为奇数，找到奇数位上的偶数调换
		if nums[i] % 2 == 1 {
			// 找奇数位上是偶数的元素
			for nums[oddIdx] % 2 == 1 {
				// 更新奇数位下标
				oddIdx += 2
			}
			// nums[oddIdx]是偶数，调换奇偶
			nums[i], nums[oddIdx] = nums[oddIdx], nums[i]
		}
	}
	return nums
}
```

