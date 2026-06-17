---
title: Two Sum
date: 2026-06-17
slug: two-sum
tags:
  - leetcode
draft: false
description: 用 hash map 把 O(n²) 降到 O(n)
---

Leetcode 上的天字第一號題目，也得上難度適中？

## 題目

給定一個整數陣列 `nums` 和一個目標值 `target`，找出陣列中兩個數字相加等於 `target` 的索引。

## 暴力解 - nested loop (solved at 2026-06-17)
用 i, j 兩個變數 iterate 所有組合
TC: O(n²) where n = length of nums: 我們需要檢查所有組合
SC: O(1): 沒有使用額外的儲存空間

```go
func twoSum(nums []int, target int) []int {
// brute-forcing: nested loop
    for i := 0; i < len(nums); i++ {
        for j := i+1; j < len(nums); j++ {
            if nums[i] + nums[j] == target {
                return []int{i,j}
            }
        }
    }

    // should never happen per problem's constraints
    return []int{}
}
```

## 解法 - Hash Map
用空間換時間，邊 iterate slice 邊記錄數值出現的位置。
TC: O(n) where n = length of nums: 只要 iterate 一次就一定可以找到答案組合
SC: O(n): 沒有使用額外的儲存空間

```go
func twoSum(nums []int, target int) []int {
    // hash table
    // map[diff]index of another number
    table := make(map[int]int)

    for i := 0; i < len(nums); i++ {
        if j, ok := table[nums[i]]; ok {
            return []int{i,j}
        }
        
        table[target - nums[i]] = i
    }

    return []int{}
}
```
