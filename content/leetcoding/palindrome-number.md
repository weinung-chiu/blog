---
title: Palindrome Number
date: 2026-06-17
slug: palindrome-number
tags:
  - leetcode
draft: true
description: 不轉成字串，只反轉一半數字判斷回文
---

## 題目

給定一個整數 `x`，判斷它是否為回文數，但不能把它轉成字串處理。

## 解法（草稿）

- 負數一律不是回文數
- 把數字尾端反向「建構」出一半，跟剩下的另一半比較，不用處理整個數字
- TODO：補上程式碼與邊界案例（例如以 0 結尾的數字）

## 複雜度

- 時間：O(log n)
- 空間：O(1)
