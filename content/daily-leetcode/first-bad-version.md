---
title: First Bad Version
date: 2026-06-30
slug: first-bad-version
tags:
  - leetcode
description: 278. First Bad Version
updated: 2026-06-30T14:11:19+08:00
---

## 題目
你的 product 在某個版本壞！掉！了！找出引入錯誤的那一版

輸入為 `n` ，代表你有 `1, 2, ... ,n` 個版本

例：G 代表好的版本，B代表壞掉的，找出壞掉的第一版
```
1, 2, 3, 4, 5, ... ,n
G, G, G, G, B, ... ,B
            ↑
ans: 5
```

提供一個 API 用以確認該版本是否為 BadVersion, `isBadVersion(version)`

## Approach : brute-forcing
從 version 1 開始確認，逐版本檢查直到找到 BadVersion 為止，TC: O(n)，LeetCode 上會 TLE.

## Approach : binary search

利用 binary search 來找。 TC: O(log n)
```go
func firstBadVersion(n int) int {
    l, r := 1, n
    bestAns := 0

    for l <= r {
        m := l + (r-l)/2
        if isBadVersion(m) {
            bestAns = m
            r = m - 1
        } else {
            l = m + 1
        }
    }

    return bestAns
}
```

binary search 很常見，實際開始寫才發現各種細節、跟邊界處理還滿多的。

2026-6 的我對這個解法的認知是 `search on answer` 用來搜空間中可能的答案並一個一個驗證。可以用於答案帶有單調性的時候，意即，如果版本 N 是 BadVersion, 那所有大於 N 的 version must be bad version.

