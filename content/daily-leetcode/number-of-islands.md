---
title: Number of Islands
date: 2026-06-17
slug: number-of-islands
tags:
  - leetcode
draft: false
description: LeetCode 200. Number of Islands
updated: 2026-07-13T14:04:31+08:00
---


## 題目
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

有點懶惰直接 copy-paste 這次試著用兩種方式來解， 兩個 BFS / DFS 可以互換不影響功能。

## Approach: visited + BFS
另外開一個 `visited` 2d slice 來標記哪些陸地是已經走訪過的. 

TC: O(mn)
SC: O(mn) — visited 所佔的空間

```go
var directions = [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

func numIslands(grid [][]byte) int {
    currLabel := 0

    rows, cols := len(grid), len(grid[0])
    
    visited := make([][]bool, rows)
    for r := range rows {
        visited[r] = make([]bool, cols)
    }

    for r := range rows {
        for c := range cols {
            if grid[r][c] == '1' && !visited[r][c] {
                // has land but not labeled
                currLabel++
                bfs(grid, visited, r, c, rows, cols)
            }
        }
    }

    return currLabel
}

func bfs(grid [][]byte, visited [][]bool, startR, startC, rows, cols int) {
	queue := [][2]int{{startR, startC}}
	visited[startR][startC] = true

	for len(queue) > 0 {
		cur := queue[0]
		queue = queue[1:]
		r, c := cur[0], cur[1]

		for _, d := range directions {
			nr, nc := r+d[0], c+d[1]
			if nr >= 0 && nr < rows && nc >= 0 && nc < cols &&
				grid[nr][nc] == '1' && !visited[nr][nc] {
				visited[nr][nc] = true
				queue = append(queue, [2]int{nr, nc})
			}
		}
	}
}
```

## Approach: sink (modify grid)
另外一個作法是，直接把走訪過的 cell 改為 `0` ，適用於不能修改輸入值的狀況。


TC: O(mn)
SC: O(1) — 修改原有的 grid, 不佔額外空間

寫筆記時想到，如果有不能修改的要求，我們也可以直接修改這邊的程式， deep clone 一個新的 grid 出來 sink. TC, SC 是相同的

```go
var directions = [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

func numIslands(grid [][]byte) int {
    count := 0

    rows, cols := len(grid), len(grid[0])
    
    var dfs func(grid [][]byte, r, c int)
    dfs = func(grid [][]byte, r, c int) {
        if r < 0 || r >= rows || c < 0 || c >= cols || grid[r][c] == '0' {
            return
        }

        // sink
        grid[r][c] = '0' 

        for _, d := range directions {
            dfs(grid, r+d[0], c+d[1])
        }
    }

    for r := range rows {
        for c := range cols {
            if grid[r][c] == '1' {
                count++
                dfs(grid, r, c)
            }
        }
    }

    return count
}
```

