DP存在递归的思想，并且是对递归的优化。DP在递归的基础上的改良在于维护一个“记事本”去记录已经解决的子问题，从而避免了递归过程中可能遇到的**重叠子问题**。
## Math Problem: Fib, Stairs, Path
此类问题数学上的解法就是递归，是最直白简单的归纳法思想的问题。数学怎么做，程序就怎么写，有时甚至可以直接通项公式数学求解

Fib: 509
Stairs: 70, 746
Path: 64, 62
## Longest Subsequence
Tips: dp\[i]: information about longest subsequence that has a certain feature and ends at the i-th index ⇒ max/min/? of dp

LIS Longest Increasing Subsequence: 300
LCS Longest Common Subsequence: 1143