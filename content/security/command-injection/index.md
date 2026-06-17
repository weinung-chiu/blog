---
title: Command Injection 筆記
date: 2026-06-17
slug: command-injection
tags:
  - security
  - web
draft: true
published: false
description: Command Injection 原理與防禦佔位筆記
---

## 原理（佔位）

當應用程式把使用者輸入直接拼進 shell 指令執行時，攻擊者可以注入額外指令。

## 範例（佔位，僅供授權測試與教學）

```
; whoami
```

![測試截圖佔位圖](./screenshot.png)

## 防禦

- 避免呼叫 shell，改用語言內建的 API
- 對輸入做白名單驗證
- 以最小權限執行
