0-1 Knapsack Problems核心就是往一定容量里面装东西，一般涉及到两个维度：**capacity和items**
核心算法就是**内外两个循环**遍历这两个维度，两循环内部则是一个**递归赋值语句**。
核心数据结构是一个长度为(capacity+1)的list `dp` ； `dp[t]` 是该问题下，capacity为t时候的解。也可以用一个二维的list（capacity和items两个维度），但我们这里为了空间复杂度考虑，决定一步到位，直接用最优解：**一维的capacity list**
  
### 遍历方式有讲究：
外循环是items，内循环是capacity：items按顺序放入
	内循环逆序遍历（递归赋值语句使用上次内循环(可以使用tmp，但逆序遍历更优)）：一个item只能放一个
	内循环顺序遍历（递归赋值语句使用本次内循环）：一个item可以放无数个
外循环是capacity，内循环是items：items不按顺序放入，一个item可以放无数个
结合题目可以写成一张表，如下：

| 问题的性质       | 一个item只能放一个                                                                                          | 一个item可以放无数个                                                       |
| ----------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| items不按顺序放入 |                                                                                                      | 外循环是capacity，内循环是items  <br>139 wordBreak  <br>377 combinationSum4 |
| items按顺序放入  | 外循环是items，内循环是capacity，内循环逆序遍历  <br>416 canPartition  <br>494 findTargetSumWays  <br>474 findMaxForm | 外循环是items，内循环是capacity，内循环顺序遍历  <br>322 coinChange  <br>518 change |
### 如何理解：
外循环是items，内循环是capacity： 每一次外循环的含义是：新考虑一个item的情况下尝试优化dp的答案，因此是顺序的
内循环逆序遍历：因为上一个循环没有当前的item，因此要添加新item也只能添加当前的这一个
内循环顺序遍历：因为用的是当前的循环，因此完全可以添加了当前的新item后继续添加
  
外循环是capacity，内循环是items：每一次外循环都有可能添加任何一个item，因此每个item完全不受控制地不按顺序放无数个