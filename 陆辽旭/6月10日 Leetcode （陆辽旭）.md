## 6月10日 Leetcode （陆辽旭）

#### 881.救生艇（[881. 救生艇 - 力扣（LeetCode）](https://leetcode.cn/problems/boats-to-save-people/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240610192157.png)

##### code：

```go
func numRescueBoats(people []int, limit int) int {

	sort.Ints(people)
	// 初始化两个指针：light 指向最轻的人，heavy 指向最重的人。
	light, heavy := 0, len(people)-1
	ans := 0 
	for light <= heavy {
		// 如果最轻和最重的人的重量之和大于船的载重限制，
		// 则最重的人需要单独一条船，将 heavy 指针向左移动。
		if people[light]+people[heavy] > limit {
			heavy--
		} else {
			// 否则，他们可以共享一条船，将 light 和 heavy 指针都向中间移动。
			light++
			heavy--
		}
		// 无论哪种情况，都表示已经安排了一条船，因此船数加一。
		ans++
	}
	return ans
}
```

