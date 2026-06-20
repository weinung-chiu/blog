---
title: Longest Substring Without Repeating Characters
date: 2026-06-20
slug: longest-substring-without-repeating-characters
tags:
  - leetcode
description: LeetCode 03. Longest Substring Without Repeating Characters
 解題筆記
---


## 題目

給定一個 string `s` , 找出最長的、不包含重複字元的 substring，注意這裡僅需要長度而不需要實際的  substring. 


## Approach: sliding window
也是寫過好幾次了，直接從已知的最佳解開始。

維護一個 counter 記錄每個字元的出現次數，用以取代每次從頭檢查整個 substring.
main loop 控制 window 的未端，如果有出現重複字就縮小 window 至不再重複為止。


TC: O(n) where n = length of s
SC: O(1): 固定的 table

```go
func lengthOfLongestSubstring(s string) int {
    // sliding window, control the end.

    // 用 map 比較標準，但是在 LeetCode 上用 array 可以顯著的減少 runtime，競爭心 😌
    // 因還包含了 number, symbols，用 127 來包含整個 ascii table
    counter := [127]int{}
    
    start := 0
    longest := 0

    for end := 0; end < len(s); end++ {
        counter[s[end]]++
        for counter[s[end]] > 1 {
            counter[s[start]]--
            start++       
        }

        longest = max(longest, end-start+1)
    }    

    return longest
}
```

