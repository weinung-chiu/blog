---
title: Valid Anagram
date: 2026-06-25
slug: valid-anagram
tags:
  - leetcode
description: LeetCode 242. Valid Anagram
updated: 2026-06-26T11:53:18+08:00
---

## 題目
給定兩個 strings `s` and `t` ，檢查 `s` 是否可以重新排列為 `t`

原文寫得比較簡單 Given two strings `s` and `t`, return `true` if `t` is an of `s`, and `false` otherwise. 但我找不到 anagram. 的好翻譯， Wikipedia 上寫 `相同字母異序詞` 似乎更難懂了。


## Approach : counting
直覺：用一個 Counter 來算 character 出現的次數

TC: O(n) where n = length of strings
SC: O(1) : fixed [127]int{}


```go
func isAnagram(s string, t string) bool {
    // quick check
    if len(s) != len(t) {
        return false
    }
    
    counter := [127]int{}

    for i := 0; i < len(s); i++ {
        counter[s[i]]++
    }

    for j := 0; j < len(t); j++ {
        counter[t[j]]--
        if counter[t[j]] < 0 {
            return false
        }
    }

    return true
}
```

## Approach : sorting
對 `s` and `t` 兩者進行排序，並檢查排序後的結果。

TC: O(n log n) where n = length of strings, 排序的開銷
SC: O(n)  where n = length of strings, for byte slieces.

在 LeetCode 上這個方式會慢一點點

```go
func isAnagram(s string, t string) bool {
    // quick check
    if len(s) != len(t) {
        return false
    }
    bs, bt := []byte(s), []byte(t)
    slices.Sort(bs)
    slices.Sort(bt)
    
    return slices.Equal(bs, bt)
}
```

## Followup:  What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

Golang 的話可以修改 sorting approach , 改用 rune 就可以直接解析 Unicode 了。