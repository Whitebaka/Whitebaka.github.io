---
title: Leetcode 22.8week1
date: 2022-08-19 22:40:45
tags:
cover: https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/1f8b4d40a1070b007a77e1a6dd86d1b59a71b506.jpg/1080
---

# Aug.Week1
## 220812
### daily-1282.用户分组(algorithms	Medium)
#### 分析
基本的hashmap就可以完成，学习一下go的map，真好使。
#### 代码块
```golang
func groupThePeople(groupSizes []int) [][]int {
	ans := make([][]int, 0)
	group := make(map[int][]int)
	for i, v := range groupSizes {
		group[v] = append(group[v], i)
		if len(group[v]) == v {
			ans = append(ans, group[v])
			group[v] = []int{}
		}
	}
	return ans
}
```
#### review
### 加餐-993.二叉树的堂兄弟节点(algorithms	Easy)
#### 分析
一遍前序遍历，记录先找到的x或y的深度和父节点数据，找到另一个值时判断他们深度是否一致、父节点是否不一致。
#### 代码块
```golang
var kx, ky, xdeep, ydeep, ffather int
var ans bool

func isCousins(root *TreeNode, x int, y int) bool {
	ffather = -2
	ans = true
	kx = x
	ky = y
	ergodic(root, 0, -1)
	return ans
}

func ergodic(now *TreeNode, deep int, tmp int) {
	if now.Val == kx {
		xdeep = deep
		if ffather == -2 {
			ffather = tmp
		} else {
			if ffather == tmp || xdeep != ydeep {
				ans = false
			}
		}
	}
	if now.Val == ky {
		ydeep = deep
		if ffather == -2 {
			ffather = tmp
		} else {
			if ffather == tmp || xdeep != ydeep {
				ans = false
			}
		}
	}
	if now.Left != nil {
		ergodic(now.Left, deep+1, now.Val)
	}
	if now.Right != nil {
		ergodic(now.Right, deep+1, now.Val)
	}
	return
}
```
#### review
为了取值方便开了全局变量并在递归中更改这个值，总觉得有一点微妙。官方题解给的匿名函数方法应该会好很多。这边数值不敏感就这样过了。

## 220813
### daily-768.最多能完成排序的块 II(algorithms	Hard)
#### 分析
首先对原数组排序得到一个结果sorted  
假如我们已经将0~k-1项分成了最多的块，从第k项起m项可以形成一个块的充要条件是构成arr[k:k+m]与sorted[k:k+m]的元素相同。  
而由于选取块时是连续选取的，而sorted又是有序的，可以预见，如果从第k项起m项或n项可以形成一个满足条件的块(m<n),那么一定可以拆成两个块[k:k+m]与[k+m:k+n]。  
从而利用一张hashdelta的hashmap，比对两个数组差值，一旦满足delta==0就立刻计数并切分。  
#### 代码块
```golang
import (
	"sort"
)
func maxChunksToSorted(arr []int) int {
	var ans int = 0
	hashdelta := make(map[int]int)
	sorted := make([]int, len(arr))
	copy(sorted, arr)
	sort.Ints(sorted)
	for i, v := range arr {
		hashdelta[v]++
		hashdelta[sorted[i]]--
		trigger := true
		for k, _ := range hashdelta {
			if hashdelta[k] != 0 {
				trigger = false
				break
			}
		}
		if trigger == true {
			ans++
			hashdelta = make(map[int]int)
		}
	}
	return ans
}
```
#### review
效率将将能过，一开始在hashdelta==0后切分但没有重置哈希表，存了很多多余的0数据影响了判断速度，增加重置后就过了。只能说girigiri。  

## 220814
### daily-1422.分割字符串的最大得分(algorithms	Easy)
#### 分析
以全字符串为基准右子串，统计右子串的最高得分。  
接着考虑左子串，左侧每引入一个0，总得分得1分，每引入一个1，总得分减1分。从而可以引入一个deltaleft记录左子串对总得分的最高贡献。
#### 代码块
```golang
func maxScore(s string) int {
	left := 0
	right := 0
	len := len(s)
	max := -1
	for i := 0; i < len; i++ {
		if s[i] == '0' {
			left++
		} else {
			left--
			right++
		}
		if left > max && i != (len-1) {
			max = left
		}
	}
	return right + max
}
```
#### review
注意题目有条件说是非空的子串，没注意到就被杀害了。初始定义时max:=-1暗中排除了左空的最坏情况，单独处理右空的可能性就行。

## 220815
### daily-641.设计循环双端队列(algorithms	Medium)
#### 分析
蛮常规的但我确实不太会的数据结构.jpg  
#### 代码块
```golang
type MyCircularDeque struct {
	content     []int
	front, rear int
}

func Constructor(k int) MyCircularDeque {
	return MyCircularDeque{content: make([]int, k+1), front: 0, rear: 0}
}

func (this *MyCircularDeque) InsertFront(value int) bool {
	if this.IsFull() {
		return false
	}
	this.front = (this.front - 1 + len(this.content)) % len(this.content)
	this.content[this.front] = value
	return true
}

func (this *MyCircularDeque) InsertLast(value int) bool {
	if this.IsFull() {
		return false
	}
	this.content[this.rear] = value
	this.rear = (this.rear + 1) % len(this.content)
	return true
}

func (this *MyCircularDeque) DeleteFront() bool {
	if this.IsEmpty() {
		return false
	}
	this.front = (this.front + 1) % len(this.content)
	return true
}

func (this *MyCircularDeque) DeleteLast() bool {
	if this.IsEmpty() {
		return false
	}
	this.rear = (this.rear - 1 + len(this.content)) % len(this.content)
	return true
}

func (this *MyCircularDeque) GetFront() int {
	if this.IsEmpty() {
		return -1
	}
	return this.content[this.front]
}

func (this *MyCircularDeque) GetRear() int {
	if this.IsEmpty() {
		return -1
	}
	return this.content[(this.rear-1+len(this.content))%len(this.content)]
}

func (this *MyCircularDeque) IsEmpty() bool {
	return this.front == this.rear
}

func (this *MyCircularDeque) IsFull() bool {
	return (this.rear+1)%len(this.content) == this.front
}
```
#### review
学习一个struct的赋值写法。  
然后就是整个双端队列的理解。本来想直接用append加元素删前后，后来意识到好像会有容量问题，写回了比较标准的数组处理法。但看了看题解应该还是可以做的，但append不能实现头部插入，需要先反转再插入再反转。

## 220816
### daily-1656.设计有序流(algorithms	Easy)
#### 分析
设计题，偏水，按题目要求写就是。
#### 代码块
```golang
type OrderedStream struct {
	elements []string
	ptr      int
}

func Constructor(n int) OrderedStream {
	return OrderedStream{elements: make([]string, n+1), ptr: 1}
}

func (this *OrderedStream) Insert(idKey int, value string) []string {
	ans := make([]string, 0)
	this.elements[idKey] = value
	i := 0
	for i = this.ptr; i < len(this.elements); i++ {
		if len(this.elements[i]) == 0 {
			break
		}
	}
	ans = append(ans, this.elements[this.ptr:i]...)
	this.ptr = i
	return ans
}
```
#### review

### daily-1925.统计平方和三元组的数目(algorithms	Easy)
#### 分析
纯水题。
#### 代码块
```golang
func countTriples(n int) int {
	ans := 0
	for a := 1; a <= n; a++ {
		for b := a + 1; b <= n; b++ {
			c := int(math.Sqrt(float64(a*a + b*b)))
			if c > n {
				continue
			}
			if c*c == a*a+b*b {
				ans++
			}
		}
	}
	return ans * 2
}
```
#### review
想优化，但不是很顺利，也许要用二分查找才能进一步提升时效？

## 220817
### daily-1302.层数最深叶子节点的和(algorithms	Medium)
#### 分析
遍历，然后捡到最终答案。
#### 代码块
```golang
func deepestLeavesSum(root *TreeNode) int {
	maxdeep := -1
	deepsum := 0
	var f func(now *TreeNode, deep int)
	f = func(now *TreeNode, deep int) {
		if now == nil {
			return
		}
		if deep > maxdeep {
			maxdeep = deep
			deepsum = now.Val
		} else if deep == maxdeep {
			deepsum += now.Val
		}
		f(now.Left, deep+1)
		f(now.Right, deep+1)
	}
	f(root, 0)
	return deepsum
}
```
#### review
相对轻松，学一个go里函数赋值给变量的写法。
## 220818
### daily-1224.最大相等频率(algorithms	Hard)
#### 分析
hashmap统计所有元素出现次数，从后往前搜索，更新hashmap  
如果一组数据满足条件，可以有如下结论：  
组内所有数出现次数只有两种可能(记min和max)  
有如下四种满足可能的方式:  
1. min只出现一次且min=1（去掉这个数满足条件）
2. max只出现一次且max-min=1（去掉max满足条件）
3. 组内只有一种可能的数（去掉任意一个数满足条件）
4. 组内所有的数只出现一次（去掉任意一个满足条件）
#### 代码块
```golang
func maxEqualFreq(nums []int) int {
	var check func() bool
	hashmap := make(map[int]int)
	check = func() bool {
		max := -1
		maxtimes := -1
		min := -1
		mintimes := -1
		for _, v := range hashmap {
			if v == 0 {
				continue
			}
			if max == -1 {
				max = v
				maxtimes = 1
			} else if max == v {
				maxtimes++
			} else if min == -1 {
				min = v
				mintimes = 1
			} else if min == v {
				mintimes++
			} else {
				return false
			}
		}
		temp := 0
		if max < min {
			temp = max
			max = min
			min = temp
			temp = maxtimes
			maxtimes = mintimes
			mintimes = temp
		}
		fmt.Println(max, maxtimes, min, mintimes)
		if min == -1 {
			if max == 1 {
				return true
			} else if maxtimes == 1 {
				return true
			} else {
				return false
			}
		}
		if maxtimes == 1 && max-min == 1 {
			return true
		}
		if mintimes == 1 && min == 1 {
			return true
		}
		return false
	}
	for _, v := range nums {
		hashmap[v]++
	}
	for i := len(nums) - 1; i >= 0; i-- {
		if check() {
			return i + 1
		}
		hashmap[nums[i]]--
	}
	return 1
}
```
#### review
实际上复杂度处理不难，适当剪枝很轻松。主要是情况种类比较多，容易遗漏。(3,4情况都没有考虑到)

## 220819
### daily-1450.在既定时间做作业的学生人数(algorithms	Easy)
#### 分析
水题。
#### 代码块
```golang
func busyStudent(startTime []int, endTime []int, queryTime int) int {
	ans := 0
	for i, _ := range startTime {
		if startTime[i] <= queryTime && queryTime <= endTime[i] {
			ans++
		}
	}
	return ans
}
```
#### review
太水了反而不知道怎么说。差分数组和二分查找都有点意味不明。

