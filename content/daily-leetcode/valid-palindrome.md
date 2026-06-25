---
title: Valid Palindrome
date: 2026-06-25
slug: valid-palindrome
tags:
  - leetcode
description: LeetCode 125. Valid Palindrome
updated: 2026-06-25T15:57:45+08:00
---

依 Grind 75 作的。

## 題目
給一個 string `s` , 檢查它是不是迴文

大小寫視作相同，空白應該移除

## Approach: Two Pointers

以前學到的最優解：不新增、更改資料，直接對 s 檢查

TC: O(n) where n = length of s
SC: O(1): 沒有使用額外的儲存空間

```go
func isPalindrome(s string) bool {
    l, r := 0, len(s)-1
    for l <= r {
        // 跳過非字母和數字的字符
        for l < r && !isAlphanumeric(byte(s[l])) {
            l++
        }
        for l < r && !isAlphanumeric(byte(s[r])) {
            r--
        }
        // 比較字元，不需要將每個 byte 轉為 rune
        if toLower(byte(s[l])) != toLower(byte(s[r])) {
            return false
        }
        l++
        r--
    }
    
    return true
}

// 確認是否是字母或數字
func isAlphanumeric(b byte) bool {
    return (b >= 'A' && b <= 'Z') || (b >= 'a' && b <= 'z') || (b >= '0' && b <= '9')
}

// 將字母轉為小寫
func toLower(b byte) byte {
    if b >= 'A' && b <= 'Z' {
        return b + ('a' - 'A')
    }
    return b
}
```



## Approach: Two Pointers follow the instruction

用理論上比較慢的作法，測試自己正確實作的能力：
建立一個新的 string `trimmedS` ，它依題目要求 1. 全部轉小寫 2. 略過空白。然後再對這個預處理過的 `trimmedS` 檢查。

理論上這會比較慢，因為它多了一個 iteration 建立 `trimmedS` , 也需要最大跟 s 一樣的空間來儲存。
但意外的 runtime 仍是 0ms beats 100% 😂

```go
func isPalindrome(s string) bool {
    var sb strings.Builder

    for i := 0; i < len(s); i++ {
        char := s[i]
        if char >= '0' && char <= '9' || char >= 'a' && char <= 'z' {
            sb.WriteByte(char)
        }

        // to lower
        if char >= 'A' && char <= 'Z' {
            lower := char - 'A' + 'a'
            sb.WriteByte(lower)
        }
    }

    trimmedS := sb.String()
    l, r := 0, len(trimmedS)-1
    for l <= r {
        if trimmedS[l] != trimmedS[r] {
            return false
        }

        l++
        r--
    }

    return true
}
```
