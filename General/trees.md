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