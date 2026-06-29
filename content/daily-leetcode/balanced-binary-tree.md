---
title: Balanced Binary Tree
date: 2026-06-29
slug: balanced-binary-tree
tags:
  - leetcode
description: LeetCode 110. Balanced Binary Tree
updated: 2026-06-29T12:10:37+08:00
---

## 題目
判斷一個 Binary Tree 是不是 Balanced，定義為：對於所有 nodes, 兩個 subtrees 的高度差不大於 `1`

## Approach 1

考慮 Example 2 的狀況： 兩顆子樹是平衡的，但高度差大於 1，我們需要知道樹的最大高度才能判斷。而 `isBalanced` 的回傳值 bool 沒辦法作到這件事。

先用比較簡單粗暴的作法：另外寫一個獨立的 `height` function 來判斷：

```go
func isBalanced(root *TreeNode) bool {
    if root == nil {
        return true
    }

    if !isBalanced(root.Left) || !isBalanced(root.Right) {
        return false
    }

    diff := height(root.Left) - height(root.Right) 
    if diff > 1 || diff < -1 {
        return false
    }

    return true
}

func height(root *TreeNode) int {
    if root == nil {
        return 0
    }

    return max(height(root.Left), height(root.Right)) + 1
}
```

它可以動，但可以注意到 height 每次都會被重覆計算。

## Approach 2
改進一下，這次我們用 `-1` 來代表這個 node unbalanced，這樣就不再繼續算 max depth.

Golang 的話也可以用似類 `(int, bool)` 的多回傳值來處理，但我想用一些簡單粗暴的操作來打基礎。

```go
func isBalanced(root *TreeNode) bool {
    return height(root) != -1
}

// use -1 as sentinel value, means that the tree isn't balanced 
func height(root *TreeNode) int {
    if root == nil {
        return 0
    }

    lHeight := height(root.Left)
    rHeight := height(root.Right)

    // one of children not balanced
    if lHeight == -1 || rHeight == -1 {
        return -1
    }

    // not balanced
    if abs(rHeight - lHeight) > 1 {
        return -1
    }

    return max(rHeight, lHeight) + 1
}
```
