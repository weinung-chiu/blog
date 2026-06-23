---
title: Valid Parentheses
date: 2026-06-23
slug: valid-parentheses
tags:
  - leetcode
description: LeetCode 20. Valid Parentheses 解題筆記
updated: 2026-06-23T11:59:06+08:00
---

依 Grind 75 作的。

## 題目
給定只包含 `()[]{}` 的字串  `s` ，判斷其中的組合是否合法。

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.


## Approach: stack
是個很基礎的 stack 題，主要難點在於 Go 沒有原生的 stack, 通常都用 slice 來模擬，理論上效能比較差但經驗中不太影響 runtime.

TC: O(n) where n = length of s
SC: O(n) where n = length of s // space for the stack

```go
func isValid(s string) bool {
    // stack problem.
    // append all characters to stack, pop if paired
    // finally result is valid if stack is empty (all paired)

    // for char in s {
    //     if char and top of stack paired  {
    //         pop stack
    //     } else {
    //         append to stack
    //     }
    // }

    // return length of stack equal to 0.
    
    
    stack := []byte{}
    for i := 0; i < len(s); i++ {
        if len(stack) > 0 && stack[len(stack)-1] == '(' && s[i] == ')' || len(stack) > 0 && stack[len(stack)-1] == '[' && s[i] == ']' || len(stack) > 0 && stack[len(stack)-1] == '{' && s[i] == '}' {
            stack = stack[:len(stack)-1]
        } else {
            stack = append(stack, s[i])
        }
    }
    
    // any character will resulting a false since it can never paired.
    return len(stack) == 0
}
```

可以看到 if condition 有點髒，用 map 來改寫成明確的 mapping:

```go
func isValid(s string) bool {
    pairs := map[byte]byte {
        '(':')',
        '[':']',
        '{':'}',
    }
    
    stack := []byte{}
    for i := 0; i < len(s); i++ {
        if len(stack) > 0 && pairs[stack[len(stack)-1]] == s[i] {
            stack = stack[:len(stack)-1]
        } else {
            stack = append(stack, s[i])
        }
    }
    
    return len(stack) == 0
}
```