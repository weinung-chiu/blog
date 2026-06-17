---
title: Two Sum
date: 2026-06-17
slug: two-sum
tags:
  - leetcode
draft: false
description: 用 hash map 把 O(n²) 降到 O(n)
---

## 題目

給定一個整數陣列 `nums` 和一個目標值 `target`，找出陣列中兩個數字相加等於 `target` 的索引。

## 解法：Hash Map

邊掃描陣列，邊用 hash map 紀錄「數值 → 索引」，查表取代暴力雙迴圈，把時間複雜度從 O(n²) 降到 O(n)。

```python
def two_sum(nums, target):
    seen = {}
    for i, n in enumerate(nums):
        if target - n in seen:
            return [seen[target - n], i]
        seen[n] = i
```

## 複雜度

- 時間：O(n)
- 空間：O(n)
