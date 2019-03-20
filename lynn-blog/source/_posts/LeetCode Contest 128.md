---
title: LeetCode Contest 127
date: 2019-03-20 09:18:54
categories:
  - Algorithm
tags:
  - LeetCode
---

# LeetCode Contest 128

LynnScarlett  
Rank: 1307 / 5165  
Finish Time: 0:14:32  
Score: 6

## 1012. Complement of Base 10 Integer

[1012. Complement of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer/)

翻译：非负数 N，表示成二进制形式后取反，返回该十进制表示。

思路：用最接近 N 的 2 的 i 次幂-1，减 N。如：`101 = 111 - 010`

实现：

```js
/**
 * @param {number} N
 * @return {number}
 */
var bitwiseComplement = function(N) {
  let i = 1
  while (N > 2 ** i - 1) {
    i++
  }
  return 2 ** i - 1 - N
}
```

## 1013. Pairs of Songs With Total Durations Divisible by 60

[1013. Pairs of Songs With Total Durations Divisible by 60](https://leetcode.com/problems/pairs-of-songs-with-total-durations-divisible-by-60/)

翻译：一个数组中求两个元素相加能被 60 整除的数量。

思路：暴力遍历

实现：

```js
/**
 * @param {number[]} time
 * @return {number}
 */
var numPairsDivisibleBy60 = function(time) {
  let res = 0
  for (let i = 0; i < time.length - 1; i++) {
    for (let j = i + 1; j < time.length; j++) {
      if (i === j) {
        continue
      }
      if ((time[i] + time[j]) % 60 === 0) {
        res++
      }
    }
  }
  return res
}
```

## 1014. Capacity To Ship Packages Within D Days

[1014. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

翻译： 有一个货物重量数组，要求在 D 天内运送完毕，从传送带一侧到另一侧需要用一天时间，求传送带最少需要支撑的重量

思路：遍历，从最大的货物重量，到货物总重量，进行二分查找

实现：

```js
/**
 * @param {number[]} weights
 * @param {number} D
 * @return {number}
 */
var shipWithinDays = function(weights, D) {
  let max = weights.reduce((x, y) => x + y)
  let min = Math.max(...weights)

  function binarySearch(start, end) {
    let mid = Math.floor((start + end) / 2)
    let total = 0
    let day = 1
    for (let i = 0; i < weights.length; i++) {
      if (total + weights[i] <= mid) {
        total += weights[i]
      } else {
        total = 0
        i--
        day++
        if (day > D) {
          if (start === end) return 0
          return binarySearch(mid + 1, end)
        }
      }
    }
    if (start === end) return start
    return binarySearch(start, mid)
  }

  return binarySearch(min, max)
}
```

## 1015. Numbers With Repeated Digits

[1015. Numbers With Repeated Digits](https://leetcode.com/problems/numbers-with-repeated-digits/)

翻译：求不大于 N 的正整数中，含有相同数字的数的个数

思路：

1. 遍历（超时）
2. 排列组合，详见：[discuss](<https://leetcode.com/problems/numbers-with-repeated-digits/discuss/257281/Java-O(1)-0ms-beats-all>)
