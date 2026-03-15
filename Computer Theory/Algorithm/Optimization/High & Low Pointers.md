 双指针出现在以下场景：
## 数据本身具有双指针属性：
有两个list，或存在palindrome和reverse这样的字眼。此情况一般是很直觉的，不做赘述。
有些题目不许复制list，要求在list上直接修改，这种情况也是要双指针的，一个用来读list，一个用来写list。比如LC 26。
## 利用首尾双指针淘汰数据：
167 Two Sum
11 Container With Most Water
15 Three Sum
存在排列规律的list，涉及到找处list中的两个element去满足一定条件。
本质上是在不断左右淘汰，从而缩小那两个element的范围。用“淘汰”这个关键词更容易理解算法。
```
Given a<=b:
A(low, high) > target, low<=a, high<=b => A(a, b) > target
A(low, high) < target, a<=low, b<=high => A(a, b) < target
```