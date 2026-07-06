---
title: 01 Matrix
date: 2026-07-06
slug: 01-matrix
tags:
  - leetcode
description: LeetCode 542. 01 Matrix
updated: 2026-07-06T16:19:30+08:00
---

## 題目
給定一個矩陣 `mat` 找出對每個 cell 來說，到達 `0` 的最近距離。

距離：兩個共用同一個 edge 的 cell 距離為 `1`

## Approach: BFS

直覺告訴我這是一個標準的 BFS 問題：
從每個 0 開始往外擴散，0 旁邊的 cell 最小距離為 1，1 旁邊的 cell 最小距離是 2 ，直到沒有更多 cell 要被更新

```go
func updateMatrix(mat [][]int) [][]int {
    // safe per constraints
    m := len(mat)
    n := len(mat[0])
    
    result := make([][]int, m)
    for i := range result {
        result[i] = make([]int, n)
    }

    queue := [][2]int{}
    for r := 0; r < m; r++ {
        for c := 0; c < n; c++ {
            if mat[r][c] == 0 {
                result[r][c] = 0
                queue = append(queue, [2]int{r,c})
            } else {
                result[r][c] = math.MaxInt
            }
        }
    }

    directions := [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
    for len(queue) > 0 {
        currX, currY := queue[0][0], queue[0][1]
        queue = queue[1:]

        for _, dir := range directions {
            newX, newY := currX+dir[0], currY+dir[1]

            // boudary check
            if newX >= 0 && newX < m && newY >= 0 && newY < n {
                // distance check
                if result[newX][newY] > result[currX][currY] + 1 {
                    result[newX][newY] = result[currX][currY] + 1
                    queue = append(queue, [2]int{newX, newY})
                }
            }
        }
    }

    return result
}
```

## Approach: ?
試了幾次發現總是有更快的作法，看了 code sample 發現這個技巧，
從 problem topic 來推論可能跟 DP 有關，之後弄清楚再回頭來補上。

