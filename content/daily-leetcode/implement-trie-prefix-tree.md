---
updated: 2026-07-15T12:05:22+08:00
---
## 題目

實作 Trie。

一個常用於 autocomplete 及 spellchecker 的資料結構。

很多東西是從資源看過原理，動手作才會知道細節，對我來說 Trie 就是其中一種。

## Improved Implementation

主要修改處：我們可以看到一開始的實作中，`Search` and `StartsWIth` 的code 幾乎是完全重複的。用一個 `find` 來重複使用它

除此之外，因為 test cases 只包含 lowercase letter ，改用 array `[26]*Node` 來儲存 children 試試看會不會更快。 (試跑了幾次都慢於原本用 map 實作的  approach ，原因不明，我也暫不想深入研究)

```go
type Node struct {
    children [26]*Node
    isEnd bool
}


type Trie struct {
    root *Node
}


func Constructor() Trie {
    return Trie {
        root: &Node{
            children: [26]*Node{},
        },
    }    
}

func (t *Trie) Insert(word string)  {
    curr := t.root
    for i := 0; i < len(word); i++ {
        char := word[i] - 'a'
        if curr.children[char] == nil  {
            curr.children[char] = &Node{
                children: [26]*Node{},
            }
        }
        curr = curr.children[char]
    }
    
    curr.isEnd = true
}


func (t *Trie) Search(word string) bool {
    result := t.find(word)
    
    return result != nil && result.isEnd
}


func (t *Trie) StartsWith(prefix string) bool {
    return t.find(prefix) != nil
}

func (t *Trie) find (word string) *Node {
    curr := t.root
    for i := 0; i < len(word); i++ {
        char := word[i] - 'a'
        if curr.children[char] == nil {
            return nil
        }
        curr = curr.children[char]
    }

    return curr
}

```

## Standard Implementation
```go
type Node struct {
    children map[byte]*Node
    isEnd bool
}


type Trie struct {
    root *Node
}


func Constructor() Trie {
    return Trie {
        root: &Node{
            children: make(map[byte]*Node),
        },
    }    
}


func (t *Trie) Insert(word string)  {
    curr := t.root
    for i := 0; i < len(word); i++ {
        char := word[i]
        if _, ok := curr.children[char]; !ok {
            curr.children[char] = &Node{
                children: make(map[byte]*Node),
            }
        }
        curr = curr.children[char]
    }
    
    curr.isEnd = true

}


func (t *Trie) Search(word string) bool {
    curr := t.root
    for i := 0; i < len(word); i++ {
        char := word[i]
        if _, ok := curr.children[char]; !ok {
            return false
        }
        curr = curr.children[char]
    }

    return curr.isEnd
}


func (t *Trie) StartsWith(prefix string) bool {
    curr := t.root
    for i := 0; i < len(prefix); i++ {
        char := prefix[i]
        if _, ok := curr.children[char]; !ok {
            return false
        }
        curr = curr.children[char]
    }

    return true
}

```