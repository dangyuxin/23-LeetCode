## 5月20日 Leetcode （陆辽旭）

#### 1177.构建回文串检测（[1177. 构建回文串检测 - 力扣（LeetCode）](https://leetcode.cn/problems/can-make-palindrome-from-substring/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240520200558.png)

##### code：

```go
func canMakePaliQueries(s string, queries [][]int) []bool {
    sum := make([][26]int, len(s)+1)
    for i, c := range s {
        sum[i+1] = sum[i]
        sum[i+1][c-'a'] ^= 1 // 奇数变偶数，偶数变奇数
    }

    ans := make([]bool, len(queries))
    for i, q := range queries {
        left, right, k, m := q[0], q[1], q[2], 0
        for j := 0; j < 26; j++ {
            m += sum[right+1][j] ^ sum[left][j]
        }
        //如果有m种字母出现奇数次，只需修改其中m/2个字母，也就是判断m/2<=k
        ans[i] = m/2 <= k
    }
    return ans
}
```





#### 1915.最美子字符串的数目（[1915. 最美子字符串的数目 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-wonderful-substrings/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240520202544.png)

##### code：

```go
func wonderfulSubstrings(word string) int64 {
	count := make([]int64, 1024)  // 计数数组，这里的1024是2的10次方，意味着我们用10位二进制数来表示状态
	count[0] = 1  // 空字符串的状态为0
	mask := 0  // 当前前缀的状态
	result := int64(0)
	for _, char := range word {
		bit := int(char - 'a')  // 计算当前字符对应的二进制位
		mask ^= 1 << bit  // 更新当前前缀的状态
		result += count[mask]  // 计算与当前前缀状态相差一个字符的状态
		for i := 0; i < 10; i++ {
			result += count[mask ^ (1 << i)]  // 计算与当前前缀状态相差一个字符的状态（每个字符出现偶数次）
		}
		count[mask]++  // 更新计数数组
	}
	return result
}
```