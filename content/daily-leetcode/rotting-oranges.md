---
title: 994. Rotting Oranges
date: 2026-07-10
slug: rotting-oranges
tags:
  - leetcode
description: LeetCode 994. Rotting Oranges
updated: 2026-07-10T11:29:55+08:00
---

爛橘子，情境跟內容都很有趣的題目。用到的技巧都不難但要正確的把它們組合在一起：

- 因為要求 minute 所以必須一層一層的向外擴展 → BFS
- 承上，因為要記錄 minute ，所以需要某個手段來讓 minute 在正確的時機增加
- 用簡單的 count 來處理 `-1` - 不是所有橘子都爛掉的狀況


## My Approach

```go
const (
	empty  = 0
	fresh  = 1
	rotten = 2
)

func orangesRotting(grid [][]int) int {
	if len(grid) == 0 || len(grid[0]) == 0 {
		return -1
	}

	rows, cols := len(grid), len(grid[0])
	minute := 0
    freshCount := 0

	queue := [][2]int{}

	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			if grid[r][c] == fresh {
                freshCount++
            }
            
			if grid[r][c] == rotten {
				queue = append(queue, [2]int{r, c})
			}
		}
	}

	directions := [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
	for len(queue) > 0 {
		minuteSize := len(queue)

		for range minuteSize {
			currX, currY := queue[0][0], queue[0][1]
			queue = queue[1:]

			for _, dir := range directions {
				newX, newY := currX+dir[0], currY+dir[1]
				if newX >= 0 && newX < rows && newY >= 0 && newY < cols {
					if grid[newX][newY] == fresh {
						grid[newX][newY] = rotten
						queue = append(queue, [2]int{newX, newY})
                        freshCount--
					}
				}
			}
		}

		if len(queue) > 0 {
			minute++
		}
	}

    if freshCount > 0 {
        return -1
    }
    
	return minute
}
```

## Improved by AI
LLM 真是學習的好幫手 😌
挑出了這些可改善的地方：
- 這是我本來最覺得有機會改善之處：它把 `if len(queue) > 0 { minute++ }` 改成與 `for len(queue) > 0` 並列的 condition `freshCount > 0`，很優雅且自然的處理的這個判斷
- 雖然 LeetCode 上的 test case 沒有這個狀況，但 `if len(grid) == 0 || len(grid[0]) == 0` 時應該是 return 0 才更符合題意
- 在 BFS 不小心把 row, column 打成 x, y 了，後者是直角坐標系的標記，不可混用

```go
const (
	empty  = 0
	fresh  = 1
	rotten = 2
)

var directions = [4][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

func orangesRotting(grid [][]int) int {
	rows, cols := len(grid), len(grid[0])
	freshCount := 0
	queue := [][2]int{}

	for r := 0; r < rows; r++ {
		for c := 0; c < cols; c++ {
			switch grid[r][c] {
			case fresh:
				freshCount++
			case rotten:
				queue = append(queue, [2]int{r, c})
			}
		}
	}

	minute := 0
	for len(queue) > 0 && freshCount > 0 {
		minute++
		for range len(queue) { // len evaluated once, so this is the level size
			curr := queue[0]
			queue = queue[1:]
			for _, d := range directions {
				r, c := curr[0]+d[0], curr[1]+d[1]
				if r >= 0 && r < rows && c >= 0 && c < cols && grid[r][c] == fresh {
					grid[r][c] = rotten
					freshCount--
					queue = append(queue, [2]int{r, c})
				}
			}
		}
	}

	if freshCount > 0 {
		return -1
	}
	return minute
}
```