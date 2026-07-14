---
title: Course Schedule
date: 2026-07-14
slug: course-schedule
tags:
  - leetcode
description: LeetCode 207. Course Schedule
updated: 2026-07-14T15:02:22+08:00
---

## 題目

graph 實在是很不熟的東西，學看看。

這題的本質是：判斷一個有向圖是否為有向無環圖 (Directed Acyclic Graph, DAG ), 把課程依它們的關係連成圖，如果有環就代表沒有辦法完成所有課程

這次試兩個 approaches，有一樣的 TC/SC
TC: O(V + E)
SC: O(V + E)
 V 代表頂點數，E 則是邊數

## Approach: Topological Sorting

我們嘗試對這個圖作拓撲排序，如果可以完成的話代表所有課程都能順利修完，反之即代表有 cycle (無法全部修完)。

這裡我們把拓撲排序的輸出丟棄，只用簡化的 `taken` 來判斷是不是有全部修完



```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    unlocks := make([][]int, numCourses)
    remainingPrereqs := make([]int, numCourses)

    for _, p := range prerequisites {
        course, prereq := p[0], p[1]
        unlocks[prereq] = append(unlocks[prereq], course)
        remainingPrereqs[course]++
    }

    queue := []int{}
    for course, remainingPrereq := range remainingPrereqs {
        if remainingPrereq == 0 {
            queue = append(queue, course)
        }
    }

    taken := 0
    for len(queue) > 0 {
        curr := queue[0]
        queue = queue[1:]

        for _, next := range unlocks[curr] {
            remainingPrereqs[next]--
            if remainingPrereqs[next] == 0 {
                queue = append(queue, next)
            }
        }

        taken++
    }

    return taken == numCourses
}
```

## Approach: DFS

```go
const (
    unvisited = iota
    visiting
    done
)

func canFinish(numCourses int, prerequisites [][]int) bool {
    // unlocks: an adjacency List
    unlocks := make([][]int, numCourses)
    for _, p := range prerequisites {
        course, prereq := p[0], p[1]
        unlocks[prereq] = append(unlocks[prereq], course)
    }

    state := make([]int, numCourses)

    var hasCycle func(course int) bool
    hasCycle = func(course int) bool {
        if state[course] == visiting {
            return true // cycle detected
        } else if state[course] == done {
            return false
        }

        state[course] = visiting
        for _, next := range unlocks[course] {
            if hasCycle(next) {
                return true
            }
        }
        state[course] = done

        return false 
    }

    for course := range numCourses {
        if hasCycle(course) {
            return false
        }
    }

    return true
}
```