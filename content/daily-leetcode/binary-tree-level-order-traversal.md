---
title: Binary Tree Level Order Traversal
date: 2026-07-09
slug: binary-tree-level-order-traversal
tags:
  - leetcode
description: LeetCode 102. Binary Tree Level Order Traversal
updated: 2026-07-09T11:41:51+08:00
---

## 題目
給定一個 binary tree `root`  ，以 level order traversal 的順序走訪所有 nodes 的 value. (`[][]int` in Go)


## Approach: level order traversal
相當單純的題目，照課本來(???)就好了

可能有差異的部份是怎麼「分隔」不同的 level ，早期我是建立了兩個 queue : `nextLevelQueue` and `currLevelQueue` ，後來發現可以再簡化成用一個單純的 loop 搞定如下

```go
func levelOrder(root *TreeNode) [][]int {
    result := [][]int{}
    if root == nil {
        return result
    }    

    queue := []*TreeNode{root}

    for len(queue) > 0 {
        levelSize := len(queue)
        levelVals := []int{}
        
        for range levelSize {
            curr := queue[0]
            queue = queue[1:]
            
            levelVals = append(levelVals, curr.Val)

            if curr.Left != nil {
                queue = append(queue, curr.Left)
            }
            
            if curr.Right != nil {
                queue = append(queue, curr.Right)
            }
        }

        result = append(result, levelVals)
    }

    return result
}
```