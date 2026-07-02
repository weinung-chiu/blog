---
title: 543. Diameter of Binary Tree
date: 2026-06-29
slug: diameter-of-binary-tree
tags:
  - leetcode
description: LeetCode 543. Diameter of Binary Tree
updated: 2026-07-02T11:20:25+08:00
---

## 題目

求二元樹的直徑

直徑的定義：樹裡面任兩個 node 中最長的路徑 (有可能不經過 root)

LeetCode 上的跟 Google 到的定義相同，應該沒有岐義

## Approach

不難，就是計算算 height ，然後再把 「path 不經過 root 」的狀況考慮進去就好。

```go
var longest int

func diameterOfBinaryTree(root *TreeNode) int {
    longest = 0
    
    _ = height(root)

    return longest
}

// how to naming this help function?
func height(root *TreeNode) int {
    if root == nil {
        return 0
    }    

    // prefered naming? heightL ?
    lHeight, rHeight := height(root.Left), height(root.Right)

    longest = max(longest, lHeight + rHeight)

    return 1 + max(lHeight, rHeight)
}
```

這可以 accepted ，但有幾個問題值得思考：
1. global var `longest` : 有許多避免 global var 的建議是有其道理的，太 magical 了。事實上如果沒有 line 28 的 `longest = 0` 它會在 test cases 間被重複使用，體現了這個問題
2. `height` 的命名不精確，雖然它表面上是計算 height ，但中間還偷渡了 longest. 的維護。


```go
func diameterOfBinaryTree(root *TreeNode) int {
    longest := 0

    var dfs func(root *TreeNode) int 
    dfs = func(root *TreeNode) int {
        if root == nil {
            return 0
        }

        l, r := dfs(root.Left), dfs(root.Right)

        longest = max(longest, l+r)

        return 1 + max(l, r)
    }
    
    _ = dfs(root)

    return longest
}

```

研究了一下，作了這些修改
1. 改用 closure, 把 `longest` 收進 `diameterOfBinaryTree` 內
2. 改名為 `dfs` ，現在的我覺得最合理的選擇：直接就說了是走訪一次，之後遇到 `bfs` 的題型也可以用。其它選項有： `helper` : 很常見的命名但我覺得這就是「懶得想」的意思。