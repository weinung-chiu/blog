---
title: Merge Two Sorted Lists
date: 2026-06-24
slug: merge-two-sorted-lists
tags:
  - leetcode
draft: true
published: false
description: LeetCode 21. Merge Two Sorted Lists
updated: 2026-06-24T11:49:50+08:00
---

## 題目

給定兩個 sorted  的 LinkedList, 要求我們把它們同樣以 sorted 的順序接在一起。並要求 `The list should be made by splicing together` ...

## 解法
一樣用 dummy head 來建立新的 List, 建立時我總會下意識的建立全新的 Linked List, 然後出這種東西：

```go
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    dummy := &ListNode{}
    curr := dummy
    for list1 != nil || list2 != nil {
        var nextVal int
        if list1 == nil {
            nextVal = list2.Val
            list2 = list2.Next
        } else if list2 == nil {
            nextVal = list1.Val
            list1 = list1.Next
        } else {
            if list1.Val < list2.Val {
                nextVal = list1.Val
                list1 = list1.Next
            } else {
                nextVal = list2.Val
                list2 = list2.Next
            }
        }

        curr.Next = &ListNode{
            Val:nextVal,
        }
        curr = curr.Next
    }

    return dummy.Next
}
```

但依照題目，我們應該要把 List 接在一起。
 在 LeetCode 上都能跑過，但其實不太一樣

```go
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    dummy := &ListNode{}
    curr := dummy

    for list1 != nil && list2 != nil {
        if list1.Val < list2.Val {
            curr.Next = list1
            list1 = list1.Next
        } else {
            curr.Next = list2
            list2 = list2.Next
        }
        curr = curr.Next
    }

    if list1 != nil {
        curr.Next = list1
    }

    if list2 != nil {
        curr.Next = list2
    }
    
    return dummy.Next
}
```