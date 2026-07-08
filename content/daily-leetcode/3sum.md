---
title: 3Sum
date: 2026-07-08
slug: 3sum
tags:
  - leetcode
description: LeetCode 15. 3Sum
updated: 2026-07-08T16:25:51+08:00
---

這題比看起來難好多 🥲

## 題目
給定一個陣列 nums, 找出所有和為零的 triplets `[nums[i], nums[j], nums[k]]`, 且
- `i != j`, `i != k`, and `j != k`
- 不能有重複的 triplets
- the order of the output and the order of the triplets does not matter.

## Approach: Brute-Forcing
列舉所有組合，TC: O(n^3) , where n = length of nums.

除了列舉之外還要想辦法處理 **不重複** 的要求，想了幾個作法都好複雜，最後用這個單純且正確的作法：用 set 來 deduplicate. 

用  `map[[3]int]bool` 來模擬 set  ，並在加入 set 前利用 sorting 來作 canonicalization.

```go
func threeSum(nums []int) [][]int {
    set := make(map[[3]int]bool)
    for i := 0; i < len(nums); i++ {
        for j := i+1; j < len(nums); j++ {
            for k := j+1; k < len(nums); k++ {
                if nums[i]+nums[j]+nums[k] == 0 {
                    triplet := []int{nums[i], nums[j], nums[k]}
                    slices.Sort(triplet)
                    arr := [3]int{triplet[0],triplet[1],triplet[2]}
                    set[arr] = true
                }
            }
        }
    }

    result := [][]int{}
    for arr, _ := range set {
        result = append(result, []int{arr[0],arr[1],arr[2]})
    }

    return result
}
```

## Approach: Two-Pointers

利用 sorting 來 deduplicate，固定 `i` 之後，用 2 pointers 來找 sum == 0 的組合

```go
func threeSum(nums []int) [][]int {
    slices.Sort(nums)
    result := [][]int{}
    
    for i := 0; i < len(nums)-1; i++ {
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }

        j, k := i+1, len(nums)-1
        for j < k {
            sum := nums[i]+nums[j]+nums[k]
            if sum == 0 {
                result = append(result, []int{nums[i],nums[j],nums[k]})
                j++
                k--
                
                for j < k && nums[j] == nums[j-1] {
                    j++
                }
                for j < k && nums[k] == nums[k+1] {
                    k--
                }
            } else if sum > 0 {
                k--
            } else if sum < 0 {
                j++
            }
        }
    } 

    return result
}
```