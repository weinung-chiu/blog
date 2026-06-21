---
title: String to Integer (atoi)
date: 2026-06-21
slug: string-to-integer-atoi
tags:
  - leetcode
description: LeetCode 8. String to Integer (atoi) 解題筆記
---


## 題目
實作 myAtoi function (ASCII To Integer). 輸入為 string , 輸出為 integer

題目已經把處理過程寫出來了，也沒有太過複雜的處理要求，只要依說明實作即可。話雖如此，但底子不夠紮實(像前幾年的我)的話也不是容易的事。


## Approach: 用 index 逐字解析
也是寫過好幾次了，直接從已知的最佳解開始。

一開始練習還不熟，用了一些 TrimSpace, unicode.IsDigit(ch) 之類的 built-in functions，到後來對 ascii 比較熟以後就比較可以穩定解出。

另外一個要注意的是結果範圍最大是 Int32, 雖然可以用這種方式作弊但還是學一下 32-bit 環境的處理比較好。

作弊的方法：在 64-bit 的環境運作但限縮到 int32
```go
if sign*res < math.MinInt32 {
	return math.MinInt32
} else if sign*res > math.MaxInt32 {
	return math.MaxInt32
}
```

TC: O(n) where n = length of s

```go
func myAtoi(s string) int {    
    i := 0
    positive := true
    result := 0

    for i < len(s) && s[i] == ' ' {
        i++
    }

    if i < len(s) && (s[i] == '-' || s[i] == '+') {
        if s[i] == '-' {
            positive = false
        }
        i++
    }

    for i < len(s) && s[i] >= '0' && s[i] <= '9' {
        digit := int(s[i] - '0')
        
        if result > (math.MaxInt32 - digit) / 10  {
            if positive {
                return math.MaxInt32
            } else {
                return math.MinInt32
            }        
        }

        result = result * 10 + digit
        i++
    }


    if positive {
        return result
    } else {
        return -result
    }
}
```

