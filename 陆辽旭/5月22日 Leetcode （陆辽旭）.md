## 5月22日 Leetcode （陆辽旭）

#### 2225.找出输掉零场或一场比赛的玩家（[2225. 找出输掉零场或一场比赛的玩家 - 力扣（LeetCode）](https://leetcode.cn/problems/find-players-with-zero-or-one-losses/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240523175721.png)

##### code：

```go
func findWinners(matches [][]int) [][]int {
    // freq 映射用于记录每个玩家输掉的比赛数量。
    freq := map[int]int{}
    for _, match := range matches {
        winner, loser := match[0], match[1]
        // 如果 freq[winner] 尚未设置，则初始化为 0。
        if freq[winner] == 0 {
            freq[winner] = 0
        }
        // 增加输家输掉的比赛数量。
        freq[loser]++
    }
    ans := make([][]int, 2)
    for key, value := range freq {
        if value < 2 {
            ans[value] = append(ans[value], key)
        }
    }
    // 对输掉零场和一场比赛的玩家编号进行排序。
    sort.Ints(ans[0])
    sort.Ints(ans[1])
    return ans
}
```

