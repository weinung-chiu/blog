---
title: Permutations & Combinations
date: 2026-07-21
slug: permutaions-and-combinations
tags:
  - leetcode
description: LeetCode 46. Permutations & 77. Combinations
updated: 2026-07-21T11:23:01+08:00
---

把排例組合兩個高度相關的一起學，比較好記？

## 如何 Permute ? 以 LeetCode 46. Permutations 為例

它看起來像是不剪枝的 39. combination sum? 我們列舉 (enumerate) 所有的排列。
但因為每個 num 只能使用一次，要如何標記？用  `[]bool`

```go
func permute(nums []int) [][]int {    
    answer := [][]int{}
    
    dfs(nums, make([]bool, len(nums)), []int{}, &answer)

    return answer
}

func dfs(nums []int, used []bool, comb []int, result *[][]int) {
    if len(comb) == len(nums) {
        temp := make([]int, len(nums))
        copy(temp, comb)
        *result = append(*result, temp)
        return
    }

    for i := 0; i < len(nums); i++ {
        if used[i] {
            continue
        }
        used[i] = true
        dfs(nums, used, append(comb, nums[i]), result)
        used[i] = false
    }
}
```



## 如何 Combine ? 以 LeetCode 77. Combinations 為例
題目給 `n` and `k`. 聯結到前述的數學定義：
- `k` 對應到 $r$，也就是實際取出的量
- `n` 與 $n$ 對應，題目給 `numbers chosen from the range [1, n]`
- 用數學來說： 這題要的是 $C(n, k)$ 

```go
func combine(n int, k int) [][]int {
    answer := [][]int{}

    // numbers chosen from the range [1,n]
    nums := make([]int, n)
    for i := range nums {
        nums[i] = i+1
    }
    
    dfs(nums, k, 0, []int{}, &answer)

    return answer
}

func dfs(nums []int, k int, start int, comb []int, result *[][]int) {
    if len(comb) == k {
        temp := make([]int, len(comb))
        copy(temp, comb)
        *result = append(*result, temp)
        return
    }

    for i := start; i < len(nums); i++ {
        dfs(nums, k, i+1, append(comb, nums[i]), result)
    }
}
```