## 5月9日 Leetcode （陆辽旭）

#### 2105.给植物浇水II（[2105. 给植物浇水 II - 力扣（LeetCode）](https://leetcode.cn/problems/watering-plants-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240509185807.png)

##### code：

```go
func minimumRefill(plants []int, capacityA, capacityB int) (ans int) {
    a, b := capacityA, capacityB
    i, j := 0, len(plants)-1
    for i < j {
        // Alice 给植物 i 浇水
        if a < plants[i] {
            // 没有足够的水，重新灌满水罐
            ans++
            a = capacityA
        }
        a -= plants[i]
        i++
        // Bob 给植物 j 浇水
        if b < plants[j] {
            // 没有足够的水，重新灌满水罐
            ans++
            b = capacityB
        }
        b -= plants[j]
        j--
    }
    // Alice 和 Bob 到达同一株植物，那么当前水罐中水更多的人会给这株植物浇水
    if i == j && max(a, b) < plants[i] {
        // 没有足够的水，重新灌满水罐
        ans++
    }
    return
}
```





#### 5.最长回文子串（[5. 最长回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-substring/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240509194304.png)

##### code：

```go
func longestPalindrome(s string) string {
	len := len(s)
	if len < 2 {
		return s
	}

	maxLen := 1
	begin := 0

	// dp[i][j] 表示 s[i..j] 是否是回文串
	dp := make([][]bool, len)
	for i := range dp {
		dp[i] = make([]bool, len)
	}

	// 初始化：所有长度为 1 的子串都是回文串
	for i := 0; i < len; i++ {
		dp[i][i] = true
	}

	charArray := []rune(s)
	// 递推开始，左下角先填
	for j := 1; j < len; j++ {
		for i := 0; i < j; i++ {
			if charArray[i] != charArray[j] {
				dp[i][j] = false
			} else {
				if j-i < 3 { //剩余子串不足2
					dp[i][j] = true
				} else {
					dp[i][j] = dp[i+1][j-1]
				}
			}

			// 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
			if dp[i][j] && j-i+1 > maxLen {
				maxLen = j - i + 1
				begin = i
			}
		}
	}
	return s[begin : begin+maxLen]
}
```

