---
title: Maximum Number of Balloons
date: 2026-06-22
slug: maximum-number-of-balloons
tags:
  - leetcode
description: LeetCode 1189. Maximum Number of Balloons 解題筆記
updated: 2026-06-22T15:46:30+08:00
---

Daily Question ，作為臨場挑戰就解看看

## 題目
給定一個 `text` ，算出用裡面的字元能拼出多少 `balloon`，預期的輸出是最大的 `balloon` 數量。


## Approach: count

幾乎毫無限制，也沒有排序、是否可重複之類的進階要求，就算一下有多少字可以運用就好。

這裡我選擇
- 用最笨的方式，全部寫成 if condition 逐次模擬消耗掉可用的字元，但這可以用算單的計算來一次取得
- 用 array 來取代 map , 在 runtime 上差異很大

TC: O(n) where n = length of text

```go
func maxNumberOfBalloons(text string) int {
	// can count a, b, n, o, and l only for extreme performance
    // but I covered all ascii table since I'm lazy
	frequency := [127]int{}

	for i := 0; i < len(text); i++ {
		frequency[int(text[i])]++
	}

	result := 0
	for frequency[int('b')] >= 1 &&
		frequency[int('a')] >= 1 &&
		frequency[int('l')] >= 2 &&
		frequency[int('o')] >= 2 &&
		frequency[int('n')] >= 1 
        {
			frequency[int('b')] -= 1
			frequency[int('a')] -= 1
			frequency[int('l')] -= 2
			frequency[int('o')] -= 2
			frequency[int('n')] -= 1
			
			result++
	}

	return result
}
```

