---
title: Maximum Subarray
date: 2026-07-03
slug: maximum-subarray
tags:
  - leetcode
description: LeetCode 53. Maximum Subarray
updated: 2026-07-03T11:33:53+08:00
---

## 題目
Given an integer array nums, find the with the largest sum, and return its sum.
找最大的子陣列和，並回傳那個和 (不需要 subarray 的其它細節)

## Approach: Brute-Forcing
已知有一個最佳解 Kadane's algorithm, 但我們從頭開始。

從試著 brute-forcing 開始：把 sum for every subarray 都算出來，然後看哪個最大
TC: O(n^3)，稍微改善相加的作法可以減少到 O(n^2) ，但還是會 TLE

```go
func maxSubArray(nums []int) int {
    globalMax := math.MinInt
    for i := 0; i < len(nums); i++ {
        localSum := 0
        for j := i; j < len(nums); j++ {
            localSum += nums[j]
            globalMax = max(globalMax, localSum)
        }
    }
    
    return globalMax
}
```

## Approach: DP

DP: `dp[i]` 代表以 i 結尾的 subarray 中的最大和
子問題是「要不要使用先前的結果？還是用現在的元素重新開始？」

```go
func maxSubArray(nums []int) int {
	// dp, dp[i] represent max sum of subarray which end at i
	dp := make([]int, len(nums))
	dp[0] = nums[0]
    answer := nums[0]
    for i := 1; i < len(nums); i++ {
        dp[i] = max(nums[i], dp[i-1]+nums[i])
        answer = max(answer, dp[i])
    }

    return answer
}Ï
```

## Kadane's Algorithm

可以從 DP 解進一步推導出來，可以注意到我們只需要 `dp[i-1]` ，並不需要整個 dp slice, 所以把它簡化成單一的變數 `answer`

```go
func maxSubArray(nums []int) int {
	curr := nums[0]
    answer := nums[0]
    for i := 1; i < len(nums); i++ {
        curr = max(nums[i], curr+nums[i])
        answer = max(answer, curr)
    }

    return answer
}
```
