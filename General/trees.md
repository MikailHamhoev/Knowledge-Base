# Binary Trees

## Simplest Form

```go
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}
```

## Tree Traversals

### Depth-First Search (DFS)

```go
// Preorder: Root -> Left -> Right
func preOrder(root *TreeNode) {
    if root == nil {
        return
    }
    fmt.Print(root.Val, " ")
    preOrder(root.Left)
    preOrder(root.Right)
}

// Inorder: Left -> Root -> Right
func inOrder(root *TreeNode) {
    if root == nil {
        return
    }
    inOrder(root.Left)
    fmt.Println(root.Val, " ")
    inOrder(root.Right)
}

// Postorder: Left -> Right -> Root
func postOrder(root *TreeNode) {
    if root == nil {
        return
    }
    postOrder(root.Left)
    postOrder(root.Right)
    fmt.Println(root.Val, " ")
}
```

### Breadth-First Search (BFS)

```go
// Visit each level left to right
func levelOrder(root *TreeNode) {
    if root == nil {
        return
    }

    queue := []*TreeNode{root}
    
    for len(queue) > 0 {
        // Deque
        node := queue[0]
        queue := queue[1:]

        fmt.Print(node.Val, " ")

        // Enque children
        if node.Left != nil {
            queue = append(queue, node.Left)
        }

        if node.Right != nil {
            queue = append(queue, node.Right)
        }
    }
}
```

## Essential Tree Problems

### Maximum Depth of Binary Tree

```go
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }

    leftDepth := maxDepth(root.Left)
    rightDepth := maxDepth(root.Right)

    return max(leftDepth, rightDepth) + 1
}
```

### Same Tree

```go
func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }

    if p == nil || q == nil {
        return false
    }

    if p.Val != q.Val {
        return false
    }

    return isSameTree(p.Left, q.Left) &&
           isSameTree(p.Right, q.Right)
}
```

### Symmetric Tree

```go
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    return isMirror(root.Left, root.Right)
}

func isMirror(left, right *TreeNode) bool {
    if left == nil && right == nil {
        return true
    }

    if left == nil || rigth == nil {
        return false
    }

    return left.Val == right.Val &&
           isMirror(left.Left, right.Right) &&
           isMirror(left.Right, right.Left)
} 
```

### Path Sum

```go
func hasPathSum(root *TreeNode, targetSum int) bool {
    if root == nil {
        return false
    }

    if root.Left == nil && root.Right == nil {
        return root.Val == targetSum
    }

    return hasPathSum(root.Left, targetSum - root.Val) ||
           hasPathSum(root.Right, targetSum - root.Val)
}
```

### Inorder Traversal

```go
func inorderTraversal(root *TreeNode) []int {
    result := []int{}
    stack := []*TreeNode{}
    current := root

    for current != nil || len(stack) > 0 {
        for current != nil {
            stack = append(stack, current)
            current = current.Left
        }

        current = stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        result = append(result, current.Val)
        current = current.Right
    }
    return result
}
```

### Similar Leafs

```go
func leafSimilar(root1  *TreeNode, root2 *TreeNode) bool {
    leaves1 := []int{}
    leaves2 := []int{}

    collectLeaves(root1, &leaves1)
    collectLeaves(root2, &leaves2)

    if len(leaves1) != len(leaves2) {
        return false
    }

    for i := 0; i < len(leaves1); i++ {
        if leaves1[i] != leaves2[i] {
            return false
        }
    }
    return true
}

func collectLeaves (root *TreeNode, leaves *[]int) {
    if root == nil {
        return
    }
    if root.Left == nil && root.Right == nil {
        *leaves = append(*leaves, root.Val)
        return
    }

    collectLeaves(root.Left, leaves)
    collectLeaves(root.Right, leaves)
}
```

### Count Nodes Not Less Than Any On The Path To The Root

```go
func goodNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return countGoodNodes(root, root.Val)
}

func countGoodNodes(node *TreeNode, maxSoFar int) int {
    if node == nil {
        return 0
    }

    count := 0 
    if node.Val >= maxSoFar {
        count = 1
        maxSoFar = node.Val
    }

    count += countGoodNodes(node.Left, maxSoFar)
    count += countGoodNodes(node.Right, maxSoFar)

    return count
}
```

### Find All Paths With Target Sum
```go
func pathSum(root *TreeNode, targetSum int) int {
    prefixSum := make(map[int]int)
    prefixSum[0] = 1

    return dfs(root, targetSum, 0, prefixSum)
}

func dfs(node *TreeNode, targetSum int, currentSum int, prefixSum map[int]int) int {
    if node == nil {
        return 0
    }

    currentSum += node.Val

    count := prefixSum[currentSum - targetSum]

    prefixSum[currentSum]++

    count += dfs(node.Left, targetSum, currentSum, prefixSum)
    count += dfs(node.Right, targetSum, currentSum, prefixSum)

    prefixSum[currentSum]--

    return count
}
```