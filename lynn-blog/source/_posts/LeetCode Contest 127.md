---
title: LeetCode Contest 127
date: 2019-03-11 08:59:31
categories:
  - Algorithm
tags:
  - LeetCode
---

# LeetCode Contest 127

从上周开始参加 LeetCode Contest。

LynnScarlett  
Rank: 1806 / 4734  
Finish Time: 1:29:22  
Score: 13

## 1005. Maximize Sum Of Array After K Negations

[1005. Maximize Sum Of Array After K Negations](https://leetcode.com/contest/weekly-contest-127/problems/maximize-sum-of-array-after-k-negations/)

翻译：给定一个 Number 数组 A, 有 K 次机会进行操作`A[i]=-A[i]`，使得数组逐项求和最大。

思路:

1. 负数从小到大消耗 K;
2. 如果负数全部变成正数后，仍剩余`K'`次，K 为偶数则跳过（因为偶数次翻转等于不翻转），K' 为奇数则将排序后的 A 的最小值翻转。

实现：

```js
/**
 * @param {number[]} A
 * @param {number} K
 * @return {number}
 */
var largestSumAfterKNegations = function(A, K) {
  A.sort((a, b) => a - b)
  var i = 0
  for (; i < A.length; i++) {
    if (A[i] < 0 && K) {
      K--
      A[i] = -A[i]
    } else if (K === 0 || A[i] >= 0) {
      break
    }
  }
  if (K % 2 === 1) {
    A.sort((a, b) => a - b)
    A[0] = -A[0]
  }
  let sum = 0
  for (let j = 0; j < A.length; j++) {
    sum += A[j]
  }
  return sum
}
```

## 1006. Clumsy Factorial

[1006. Clumsy Factorial](https://leetcode.com/contest/weekly-contest-127/problems/clumsy-factorial/)

翻译：给定一个数 n，把`n!`运算中的每个`*`运算符依次替换成`* / + -`循环，如：`clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1`，除法运算后直接取`floor`。

思路：`clumsy(10)`可以看成`(10 * 9 / 8 + 7) - ( 6 * 5 / 4 - 3) - (2 * 1)`。

实现：

```js
/**
 * @param {number} N
 * @return {number}
 */
var clumsy = function(N) {
  let clumsyRegion = (start, len, flag) => {
    if (len === 1) return flag ? 1 : -1
    if (len === 2) return flag ? 2 : -2
    if (len === 3) return flag ? 6 : -6
    if (len === 4)
      return flag
        ? Math.floor((start * (start - 1)) / (start - 2)) + (start - 3)
        : -Math.floor((start * (start - 1)) / (start - 2)) + (start - 3)
  }
  let sum = 0
  var i = N
  for (; i > 3; i -= 4) {
    sum += clumsyRegion(i, 4, i === N)
  }
  if (i) {
    sum += clumsyRegion(i, i, i === N)
  }
  return sum
}
```

## 1007. Minimum Domino Rotations For Equal Row

[1007. Minimum Domino Rotations For Equal Row](https://leetcode.com/contest/weekly-contest-127/problems/minimum-domino-rotations-for-equal-row/)

翻译：两个 Number 数组 A 和 B，通过交换相同位置的 A 和 B 元素，使得 A 或 B 中各个元素相同，求最小操作次数。

思路：

1. 先出现次数最多的元素 N，如果 N 出现的次数小于 A 的长度，那么无论如何交换都不会有完全相同的数组；
2. 针对 N，遍历 A 和 B，维护两个数组 sumA 和 sumB，分别记录 A 和 B 变成完全相同数组需要的操作数；
3. 如果`A[i]!==N`，`sumA++`，B 同理；
4. 返回`Math.min(sumA, sumB)`。

实现：

```js
/**
 * @param {number[]} A
 * @param {number[]} B
 * @return {number}
 */
var minDominoRotations = function(A, B) {
  let dic = Array(7).fill(0)

  for (let i = 0; i < A.length; i++) {
    dic[A[i]]++
    dic[B[i]]++
  }
  var j = 1
  for (; j < 7; j++) {
    if (dic[j] >= A.length) {
      break
    }
  }
  if (dic[j] < A.length) {
    return 1
  }
  var sumA = 0
  var sumB = 0
  for (let k = 0; k < A.length; k++) {
    if (A[k] === j && B[k] !== j) {
      sumB++
    } else if (A[k] !== j && B[k] === j) {
      sumA++
    } else if (A[k] !== j && B[k] !== j) {
      return -1
    }
  }
  return sumA < sumB ? sumA : sumB
}
```

## 1008. Construct Binary Search Tree from Preorder Traversal

[1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/contest/weekly-contest-127/problems/construct-binary-search-tree-from-preorder-traversal/)

翻译：通过前序遍历序列生成 BST。

思路：分治法，序列第一项为（当前子树的）根节点，序列后第一个比根节点大的开始为右子树，其余的为左子树。

实现：

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} preorder
 * @return {TreeNode}
 */
var bstFromPreorder = function(preorder) {
  if (preorder.length === 0) {
    return null
  }
  let mid = preorder[0]
  let tree = new TreeNode(mid)
  var i = 0
  for (; i < preorder.length; i++) {
    if (preorder[i] > mid) {
      break
    }
  }
  tree.left = bstFromPreorder(preorder.slice(1, i))
  tree.right = bstFromPreorder(preorder.slice(i))
  return tree
}
```
