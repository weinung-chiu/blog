---
title: Add Binary
date: 2026-07-01
slug: add-binary
tags:
  - leetcode
description: LeetCode 67. Add Binary
updated: 2026-07-01T17:07:01+08:00
---

## 題目
給兩個用 string 表示的 binary ，把它們加起來，一樣回傳 string

**Constraints** : `1 <= a.length, b.length <= 10^4` 


## Approach
想到兩個解法：先把 `a`, `b` 都轉成 int , 相加， 最後再把結果轉成 string，完美。
但是注意 **Constraints** : `1 <= a.length, b.length <= 10^4` , a, b 及結果都有可能會 overflow.

所以還是用簡單但有效的相加吧，基本上就是考驗我們的加法實作能力

```go
// naive approach, just add a and b.
// is this simplest and the best solution ?
// if yes, it seems the problem (or this approach at least) just ask us handle addition and string/byte correct (in go)

const base = 2

func addBinary(a string, b string) string {
    iA, iB := len(a)-1, len(b)-1
    sum, carry := 0, 0
    result := []byte{}
    
    for iA >= 0 || iB >= 0 || carry > 0 {
        sum = carry

        if iA >= 0 {
            sum += int(a[iA] - '0')
            iA--
        }

        if iB >= 0 {
            sum += int(b[iB] - '0')
            iB--
        }

        carry = sum / base
        sum = sum % base

        result = append(result, byte(sum + '0'))
    }

    //reverse the result
    l, r := 0, len(result)-1
    for l < r {
        result[l], result[r] = result[r], result[l]
        l++
        r--
    }

    return string(result)
}
```
