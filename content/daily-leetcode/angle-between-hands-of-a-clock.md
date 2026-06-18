---
title: Angle Between Hands of a Clock
date: 2026-06-19
slug: angle-between-hands-of-a-clock
tags:
  - leetcode
description: 看起來很有趣的時鐘題目
---

不是原計劃要解的題目，但時鐘看起來很有趣就拿來當今日的練習。

## 題目

給定一個整數陣列 `nums` 和一個目標值 `target`，找出陣列中兩個數字相加等於 `target` 的索引。
給定兩個數值 `hour` and  `minute` ，分別代表時鐘上的時、分，求短針長針的夾角 (較小的那個)

## 直覺解
總之，先算出短針長針的角度，然後再算夾角就好了。

TC: O(1): 計算過程固定
SC: O(1): 沒有使用額外的儲存空間

倒是意外的不曉得怎麼求出較小的那個角，還要考慮 11:05 這種兩個針很近但跨越12點的狀況。
邊洗澡邊想，想出了能用的土炮解如下。
```go
const (
    hourHandDegPerHour   = 30.0 // 360 / 12
    hourHandDegPerMinute = 0.5  // 30 / 60，時針隨分鐘緩慢漂移
    minuteHandDegPerMin  = 6.0  // 360 / 60
)

func angleClock(hour int, minutes int) float64 {
    hourAngle := hourHandDegPerHour*float64(hour%12) + hourHandDegPerMinute*float64(minutes)
    minuteAngle := minuteHandDegPerMin * float64(minutes)

    gapA := hourAngle - minuteAngle
    if gapA < 0 {
        gapA += 360
    }
    gapB := minuteAngle - hourAngle
    if gapB < 0 {
        gapB += 360
    }
    
    return min(gapA, gapB)

```

## 改善
請 LLM 幫忙調整命名的過程中，提到可以簡化成這樣子：

> gap 是兩針夾角，360-gap 是它的優角（reflex）
```go
gap := math.Abs(hourAngle - minuteAngle)
return math.Min(gap, 360-gap)
```
但我把已經把三角形的各種定義還給數學老師了，就維持自己能看得懂的作法吧。
