
## 題目
給定一個 binary tree 的 root，判斷這是不是 binary search tree (BST)。

BST 的定義如下 (copy-paste)
- The left of a node contains only nodes with keys **strictly less than** the node's key.
- The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

對我來說最常見的陷阱就是個別看都是正確的，但 children 的值不符合定義 ，如 LeetCode 上的 Example 2 這樣，4, 3, 6 是合法的 BST，但如果把 root 5 考慮進來，3 就比 5 還小了。因此我們需要在不同的 stack 中傳遞比單純的 bool  `isBST` 更多的資訊。

我想到幾個可能性：
- 用類似 [[balanced-binary-tree]] 的方式，傳遞 depth, 再用 sentinel value 如 `-1` 來代表 "root 不合法"，但這一題需要最大及最小值。
- 利用 Golang 的 multiple return values (如下)
- 把 return value 包裝成 structure

## Approach: bottom-top
```go
func isValidBST(root *TreeNode) bool {
    isBst, _, _ := dfs(root)

    return isBst
}

// isBst, min, max
func dfs(root *TreeNode) (bool, int, int) {
    if root == nil {
        return true, math.MaxInt, math.MinInt
    }

    lBst, lMin, lMax := dfs(root.Left)
    rBst, rMin, rMax := dfs(root.Right)

    if !lBst || !rBst || lMax >= root.Val || rMin <= root.Val {
        // the value didn't matter if not BST
        return false, 0, 0
    }  

    lMin = min(lMin, root.Val)
    rMax = max(rMax, root.Val)
    return true, lMin, rMax 
}
```

## Approach: top-bottom

另外一個作法.. 回傳多個資訊不簡單，那傳入多個參數就簡單且自然多了 😌

這樣也可以更快的 return if invalid.

```go
func isValidBST(root *TreeNode) bool {
    return valid(root, math.MinInt, math.MaxInt)
}

func valid(root *TreeNode, low, high int) bool {
    if root == nil {
        return true
    }

    // may fail on edge case that node with min int or max int.
    if root.Val <= low || root.Val >= high {
        return false
    }

    return valid(root.Left, low, root.Val) && valid(root.Right, root.Val, high)
}
```

