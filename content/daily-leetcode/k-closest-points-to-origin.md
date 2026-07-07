---
title: K Closest Points to Origin
date: 2026-07-07
slug: k-closest-points-to-origin
tags:
  - leetcode
description: LeetCode 973. K Closest Points to Origin
updated: 2026-07-07T10:40:51+08:00
---

算出了 distance 之後，還要把這個 distance 跟 points 關聯在一起。可以用 index mapping 一個獨立的 struct 來解決



## Approach: sorting
不錯的起點，找到方式處理前述的對應問題之後，再 sorting 解決，當然，這樣是浪費了部份的排序效能

```go
// we can order without the √, save the cost.
// start by sort it!
type myPoint struct {
    x, y int
    distance int // not actual distance, just for comparison
}

func kClosest(points [][]int, k int) [][]int {
    mps := make([]myPoint, len(points))

    for i, point := range points {
        mps[i] = myPoint {
            x: point[0],
            y: point[1],
            distance: point[0]*point[0] + point[1]*point[1],
        }
    }
    
    slices.SortFunc(mps, func(a, b myPoint) int {
        return a.distance - b.distance
    })

    result := make([][]int, k)
    for i := 0; i < k; i++ {
        result[i] = []int{mps[i].x, mps[i].y}
    }

    return result
}
```

## Approach: heap
有可能是比較標準的解。

雖然 go 有內建的 `container/heap` 可以用，但為了記得 heap 到底怎麼運作的還是手刻一下🥶

```go
type myPoint struct {
    x, y int
    distance int // not actual distance, just for comparison
}

func NewMaxHeap(cap int) *maxHeap {
    return &maxHeap {
        data: make([]myPoint, 0, cap+1),
        capacity: cap,
    }
}

type maxHeap struct {
    data []myPoint
    capacity int
}

func (h *maxHeap) Len() int {
    return len(h.data)
}

func (h *maxHeap) Push(mp myPoint) {
    h.data = append(h.data, mp)
    h.heapifyUp(len(h.data)-1)
    if len(h.data) > h.capacity {
        h.Pop()
    }
}

func (h *maxHeap) Pop() myPoint {
    if len(h.data) == 0 {
        panic("empty heap")
    }

    pop := h.data[0]
    h.data[0] = h.data[len(h.data)-1]
    h.data = h.data[:len(h.data)-1]
    if len(h.data) > 0 {
        h.heapifyDown(0)
    }

    return pop
}

func (h *maxHeap) heapifyUp(i int) {
    for i > 0 {
        parent := (i-1)/2
        // heapify up if child greater than parent
        if h.data[i].distance > h.data[parent].distance {
            h.data[i], h.data[parent] = h.data[parent], h.data[i]
            i = parent
        } else {
            break
        }
    }
}

func (h *maxHeap) heapifyDown(i int) {
    for {
        lChild, rChild := i*2+1, i*2+2
        largest := i

        if lChild < len(h.data) && h.data[lChild].distance > h.data[largest].distance {
            largest = lChild    
        }
        
        if rChild < len(h.data) && h.data[rChild].distance > h.data[largest].distance {
            largest = rChild    
        }

        if largest == i {
            break
        }

        h.data[i], h.data[largest] = h.data[largest], h.data[i]
        i = largest
    }
}

func kClosest(points [][]int, k int) [][]int {
    maxHeap := NewMaxHeap(k)
    
    for _, point := range points {
        mp := myPoint {
            x: point[0],
            y: point[1],
            distance: point[0]*point[0] + point[1]*point[1],
        }
        maxHeap.Push(mp)
    }
    
    result := [][]int{}
    for maxHeap.Len() > 0 {
        pop := maxHeap.Pop()
        result = append(result, []int{pop.x, pop.y})
    }

    return result
}
```