---
title: Leetcode 22.8week2
date: 2022-08-26 19:41:34
tags:
cover: https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/Fa57u5sacAEYmUC.jpg/1080
---
# Aug.Week2
## 220820
### daily-654.最大二叉树(algorithms	Medium)
#### 分析
在数组里找最大的，然后取值，拆分数组左右递归再找最大的。
#### 代码块
```golang
func constructMaximumBinaryTree(nums []int) *TreeNode {
	if len(nums) == 0 {
		return nil
	}
	best := 0
	for i, num := range nums {
		if num > nums[best] {
			best = i
		}
	}
	return &TreeNode{nums[best], constructMaximumBinaryTree(nums[:best]), constructMaximumBinaryTree(nums[best+1:])}
}
```
#### review
struct的递归构造还是有点问题，官方这个写法要好看很多。

## 220821
### daily-1455.检查单词是否为句中其他单词的前缀(algorithms	Easy)
#### 分析
简单字符串处理，对着查就完了。
#### 代码块
```golang
func isPrefixOfWord(sentence string, searchWord string) int {
	ans := -1
	wordnow := 1
	right := true
	for j := 0; j < len(searchWord); j++ {
		if j < len(sentence) && searchWord[j] != sentence[j] {
			right = false
		}
	}
	if right == true {
		ans = wordnow
		return ans
	}
	for i := 0; i < len(sentence); i++ {
		if sentence[i] == ' ' {
			wordnow++
			right = true
			for j := 0; j < len(searchWord); j++ {
				if i+j+1 < len(sentence) && searchWord[j] != sentence[i+j+1] {
					right = false
				}
			}
			if right == true {
				ans = wordnow
				return ans
			}
		}
	}
	return ans
}

```
#### review
没啥内容。


## 220822
### daily-655.输出二叉树(algorithms	Medium)
#### 分析
虽然是medium但其实只要跟着他的写法写，规则都定好了。
#### 代码块
```golang
func depth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	dleft := depth(root.Left) + 1
	dright := depth(root.Right) + 1
	if dleft > dright {
		return dleft
	} else {
		return dright
	}
}

func printTree(root *TreeNode) [][]string {
	height := depth(root) - 1
	m := height + 1
	n := 1<<(height+1) - 1
	ans := make([][]string, m)
	for i := 0; i < m; i++ {
		ans[i] = make([]string, n)
	}
	var f func(root *TreeNode, r int, c int)
	f = func(root *TreeNode, r int, c int) {
		if root == nil {
			return
		}
		ans[r][c] = strconv.Itoa(root.Val)
		if root.Left != nil {
			f(root.Left, r+1, c-(1<<(height-r-1)))
		}
		if root.Right != nil {
			f(root.Right, r+1, c+(1<<(height-r-1)))
		}
	}
	f(root, 0, (n-1)/2)
	return ans
}
```
#### review
要习惯$2^x$的$1<<x$写法

## 220823
### daily-782.变为棋盘(algorithms	Hard)
#### 分析
这周的hard，虽然想到了应该是比较数学的，但在具体算法上止步到了棋盘初始检验和行变列不变列变行不变，卡在最后一步统计上了。
#### 代码块
```golang
func getMoves(mask uint, count, n int) int {
	ones := bits.OnesCount(mask)
	if n&1 > 0 {
		// 如果 n 为奇数，则每一行中 1 与 0 的数目相差为 1，且满足相邻行交替
		if abs(n-2*ones) != 1 || abs(n-2*count) != 1 {
			return -1
		}
		if ones == n>>1 {
			// 偶数位变为 1 的最小交换次数
			return n/2 - bits.OnesCount(mask&0xAAAAAAAA)
		} else {
			// 奇数位变为 1 的最小交换次数
			return (n+1)/2 - bits.OnesCount(mask&0x55555555)
		}
	} else {
		// 如果 n 为偶数，则每一行中 1 与 0 的数目相等，且满足相邻行交替
		if ones != n>>1 || count != n>>1 {
			return -1
		}
		// 偶数位变为 1 的最小交换次数
		count0 := n/2 - bits.OnesCount(mask&0xAAAAAAAA)
		// 奇数位变为 1 的最小交换次数
		count1 := n/2 - bits.OnesCount(mask&0x55555555)
		return min(count0, count1)
	}
}

func movesToChessboard(board [][]int) int {
	n := len(board)
	// 棋盘的第一行与第一列
	rowMask, colMask := 0, 0
	for i := 0; i < n; i++ {
		rowMask |= board[0][i] << i
		colMask |= board[i][0] << i
	}
	reverseRowMask := 1<<n - 1 ^ rowMask
	reverseColMask := 1<<n - 1 ^ colMask
	rowCnt, colCnt := 0, 0
	for i := 0; i < n; i++ {
		currRowMask, currColMask := 0, 0
		for j := 0; j < n; j++ {
			currRowMask |= board[i][j] << j
			currColMask |= board[j][i] << j
		}
		if currRowMask != rowMask && currRowMask != reverseRowMask || // 检测每一行的状态是否合法
			currColMask != colMask && currColMask != reverseColMask { // 检测每一列的状态是否合法
			return -1
		}
		if currRowMask == rowMask {
			rowCnt++ // 记录与第一行相同的行数
		}
		if currColMask == colMask {
			colCnt++ // 记录与第一列相同的列数
		}
	}
	rowMoves := getMoves(uint(rowMask), rowCnt, n)
	colMoves := getMoves(uint(colMask), colCnt, n)
	if rowMoves == -1 || colMoves == -1 {
		return -1
	}
	return rowMoves + colMoves
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func min(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```
#### review
掩码的写法很不错，自己位运算用的还是少太多了。

## 220824
### daily-1460.通过翻转子数组使两个数组相等(algorithms	Easy)
#### 分析
不要求求最短，一张hashmap写完。
#### 代码块
```golang
func canBeEqual(target []int, arr []int) bool {
	hashmap := make(map[int]int)
	for i := 0; i < len(target); i++ {
		hashmap[target[i]]++
		hashmap[arr[i]]--
	}
	for key, _ := range hashmap {
		if hashmap[key] != 0 {
			return false
		}
	}
	return true
}

```
#### review
偏水。
## 220825
### daily-658.找到-k-个最接近的元素(algorithms	Medium)
#### 分析
题给的数组排好序了，直接跟着条件搜就行。
#### 代码块
```golang
func findClosestElements(arr []int, k int, x int) []int {
	ans := make([]int, k)
	ans = arr[:k]
	for now := k; now < len(arr); now++ {
		if abs(arr[now]-x) < abs(ans[0]-x) {
			ans = arr[now+1-k : now+1]
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```
#### review
因为数据规模不大直接o(n)了，实际上用二分可以更快的。

## 220826
### daily-1464.数组中两元素的最大乘积(algorithms	Easy)
#### 分析
找一个数组的最大值和次大值，你好水啊。
#### 代码块
```golang
func maxProduct(nums []int) int {
	max1 := 0
	max2 := 1 //larger
	if nums[0] > nums[1] {
		max2 = 0
		max1 = 1
	}
	for i := 2; i < len(nums); i++ {
		if nums[i] > nums[max2] {
			max1 = max2
			max2 = i
		} else if nums[i] > nums[max1] {
			max1 = i
		}
	}
	return (nums[max1] - 1) * (nums[max2] - 1)
}
```
#### review
好水！下班！