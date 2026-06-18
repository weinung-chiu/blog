---
title: Add Two Numbers
date: 2026-06-18
slug: add-two-numbers
tags:
  - leetcode
description: LeetCode 02. Add Two Number 解題筆記
---

Leetcode 上的第二號題目，不算難需要一些對加法、進位、及 Linked List 的理解。這一次的我靠筆記及肌肉記憶就解出來了。

## 題目
給兩個 Linked List ，要把它們相起，輸出的位置也不需要特別調整
2 → 4 → 3
5 → 6 → 4
expected:
7 → 8 → 8

## 直覺解法
我自己比較常漏掉或遺忘的有幾個
1. 要怎麼正確的建立新的 Linked List ，插入新 Node，並在最後正確的 return head ，這個我是翻自己的筆記(小抄)
2. sum, carry 之間的關係
3. for loop condition 中的 carry > 0 ，這個我幾乎每次寫都會忘

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{}

    curr := dummy 

    var sum, carry = 0, 0
    for l1 != nil || l2 != nil || carry > 0 {
        sum = carry

        if l1 != nil {
            sum += l1.Val
            l1 = l1.Next
        }

        if l2 != nil {
            sum += l2.Val
            l2 = l2.Next
        }

        carry = sum/10
        sum = sum%10

        curr.Next = &ListNode{Val: sum}
        curr = curr.Next
    }

    return dummy.Next
}
```
