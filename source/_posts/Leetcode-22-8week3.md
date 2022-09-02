---
title: Leetcode 22.8week3
date: 2022-09-02 23:59:13
tags:
cover: https://whitebaka-1301161068.cos.ap-nanjing.myqcloud.com/image/100917024_p0.jpg/1080
---
# Aug.Week3
## 220827
### daily-662.二叉树最大宽度(algorithms	Medium)
#### 分析
搜索一遍，存一下位置信息就可以搞定。
#### 代码块
```golang
func widthOfBinaryTree(root *TreeNode) int {
	deepleft := make([]int, 0)
	deepright := make([]int, 0)
	var f func(root *TreeNode, deep int, now int)
	f = func(root *TreeNode, deep int, now int) {
		if root == nil {
			return
		}
		if len(deepleft) <= deep {
			deepleft = append(deepleft, now)
			deepright = append(deepright, now)
		} else if deepleft[deep] > now {
			deepleft[deep] = now
		} else if deepright[deep] < now {
			deepright[deep] = now
		}
		f(root.Left, deep+1, 2*now-1)
		f(root.Right, deep+1, 2*now)
	}
	f(root, 0, 1)
	max := -1
	for i := 0; i < len(deepleft); i++ {
		if deepright[i]-deepleft[i] > max {
			max = deepright[i] - deepleft[i]
		}
	}
	return max + 1
}
```
#### review
最后还是写成了深搜，也许广搜更适合这题一点，最后出来一看时间还行空间是有点偏高了。
## 220828
### daily-793.阶乘函数后-k-个零(algorithms	Hard)
#### 分析
数学题，首先统计阶乘后k个0，本质就是统计里面5的个数是否为k（2的个数永远比5多）。然后每五次就必定会多至少一个五，所以return的结果不是5个，就是0个。问题就转化成了什么时候返回0个。  
对基本例的判研可以发现，如果一个k返回0值，说明他被跳过了，即阶乘里新加入的数包含多个5。因而我们只要对pow(5,i)的情况作处理即可。  
预生成一个数组，test[i]的数据为pow(5,i)!的末尾0个数。然后拿k去比对，k应该可以由test的特定项组成，且每一项的系数一定小于5，如若等于5，则说明他是被跳过的k值。
#### 代码块
```golang
func preimageSizeFZF(k int) int {
	test := make([]int, 0)
	i := 1
	for i < k {
		test = append(test, i)
		i = i*5 + 1
	}
	fivelen := len(test)
	i = fivelen - 1
	for k > 0 {
		if k == 5*test[i] {
			return 0
		}
		for k >= test[i] {
			k -= test[i]
		}
		i--
	}
	return 5
}
```
#### review
纯数学题，分析时间比较长代码简单，但写表述好难感觉没说到点上。

## 220829
### daily-1470.重新排列数组(algorithms	Easy)
#### 分析
比较简单，参数还多给一个n，乱跑。
#### 代码块
```golang
func shuffle(nums []int, n int) []int {
	ans := make([]int, n*2)
	for i := 0; i < n; i++ {
		ans[2*i] = nums[i]
		ans[2*i+1] = nums[n+i]
	}
	return ans
}
```
#### review
交完感觉时间不快，不知道快的怎样写的。


## 220830
### daily-998.最大二叉树-ii(algorithms	Medium)
#### 分析
最后一步和上次的654一样，只要先还原列表再跑一次654就行，树还原列表和列表构建数思路一致，已经保证是列表产生，只要左右递归的处理即可。
#### 代码块
```golang
func insertIntoMaxTree(root *TreeNode, val int) *TreeNode {
	a := maxTreeToSlice(root)
	b := append(a, val)
	return constructMaximumBinaryTree(b)
}

func maxTreeToSlice(root *TreeNode) []int {
	ans := []int{}
	if root == nil {
		return ans
	}
	ans = append(maxTreeToSlice(root.Left), root.Val)
	ans = append(ans, maxTreeToSlice(root.Right)...)
	return ans
}

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
速度很快，因为递归构建的关系内存占用有点多。然后看了一眼题解发现其实只要插入一个新val就是了，因为val添加在列表最后，故只可能是根结点或右节点，一路搜过去然后插入构建一下就完了，我怎么没想到可恶啊。

## 220831
### daily-946.验证栈序列(algorithms	Medium)
#### 分析
模拟，能出就出。
#### 代码块
```golang
func validateStackSequences(pushed []int, popped []int) bool {
	lenth := len(pushed)
	stacknow := make([]int, 0)
	stackend := -1
	popnow := 0
	for i := 0; i < lenth; i++ {
		stackend++
		stacknow = append(stacknow, pushed[i])
		for stacknow[stackend] == popped[popnow] {
			stacknow = stacknow[:stackend]
			stackend--
			popnow++
			if stackend < 0 {
				break
			}
		}
	}
	if stackend == -1 {
		return true
	}
	return false
}
```
#### review
做一下边际处理就完事了。

## 2200901
### daily-1475.商品折扣后的最终价格(algorithms	Easy)
#### 分析
九月第一题，好水，好水呀！
#### 代码块
```golang
func finalPrices(prices []int) []int {
	for i := 0; i < len(prices); i++ {
		for j := i + 1; j < len(prices); j++ {
			if prices[j] <= prices[i] {
				prices[i] -= prices[j]
				break
			}
		}
	}
	return prices
}
```
#### review
利用单调性的处理方法是一开始想到的，顺着写完发现不太对还是改为直接模拟了。这题需要逆着然后维护一个单调栈，能做到o(n)复杂度，会更漂亮一点。

## 2200902
### daily-687.最长同值路径(algorithms	Medium)
#### 分析
本质遍历，注意到一个节点作通路根节点和作左右枝时在通路的表现不同。左枝右枝时传最长单链，作根时可以取左右子节点之和。
#### 代码块
```golang
func longestUnivaluePath(root *TreeNode) int {
	var f func(root *TreeNode) int
	max := 0
	f = func(root *TreeNode) int {
		if root == nil {
			return 0
		}
		left := f(root.Left)
		right := f(root.Right)
		if root.Left == nil || root.Left.Val != root.Val {
			left = 0
		}
		if root.Right == nil || root.Right.Val != root.Val {
			right = 0
		}
		if left+right > max {
			max = left + right
		}
		if left > right {
			return left + 1
		} else {
			return right + 1
		}
	}
	f(root)
	return max
}
```
#### review
习惯性给max赋-1然后被空树谋杀了，可恶啊。