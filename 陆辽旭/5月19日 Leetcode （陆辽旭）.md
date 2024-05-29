## 5月19日 Leetcode （陆辽旭）

#### 1535.找出数组游戏的赢家（[1535. 找出数组游戏的赢家 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-winner-of-an-array-game/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240519193047.png)

##### code：

```go
func getWinner(arr []int, k int) int {
    stack := make([]int, len(arr))
    head, tail := 0, 0
    stack[head] = arr[0]
    index := 1
    for index < len(arr) {
        if stack[head] > arr[index] {
            tail++
            stack[tail] = arr[index]
            // 确保 stack 里的元素数量不超过 k
            if tail - head == k {
                return stack[head]
            }
        } else {
            stack[tail] = arr[index]
            head = tail
            tail++//tail++代表替换数赢了一次
            if tail - head == k {
                return stack[head]
            }
        }
        index++
    }
    return stack[head]
}
```





#### 947.移除最多的同行或同列石头（[947. 移除最多的同行或同列石头 - 力扣（LeetCode）](https://leetcode.cn/problems/most-stones-removed-with-same-row-or-column/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240519200710.png)

##### code：

```go
type unionFind struct {
	parent map[int]int
	cnt    int
}

func NewUnionFind() *unionFind {
	return &unionFind{
		parent: make(map[int]int),
	}
}

func (u *unionFind) getCount() int {
	return u.cnt
}

func (u *unionFind) find(x int) int {
	if _, ok := u.parent[x]; !ok {
		u.parent[x] = x
		// 并查集集中新加入一个结点，结点的父亲结点是它自己，所以连通分量的总数 +1
		u.cnt++
	}

	if x != u.parent[x] {
		u.parent[x] = u.find(u.parent[x])
	}
	return u.parent[x]
}

func (u *unionFind) union(x, y int) {
	rootX, rootY := u.find(x), u.find(y)
	if rootX == rootY {
		return
	}
	u.parent[rootX] = rootY
	// 两个连通分量合并成为一个，连通分量的总数 -1
	u.cnt--
}

func removeStones(stones [][]int) int {
	unionf := NewUnionFind()
	for _, s := range stones {
		unionf.union(s[0]+114514, s[1])//增加114514偏移量，确保石头的坐标在并查集中作为节点的标识符时不会重复
	}
	return len(stones) - unionf.getCount()//石头的总数减去连通分量的数量，即最少需要移除的石头数
}
```

