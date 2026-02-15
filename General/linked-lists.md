## Linked List Basics in Go

```go
type ListNode struct {
    Val  int
    Next *ListNode
}
```

## Essential Patterns & Techiniques

### Pattern 1: Traversal

```go
// Print all values
func traverse(head *ListNode) {
    current := head
    for current != nil {
        fmt.Println(current.Val)
        current = current.Next
    }
}
```
### Pattern 2: Two Pointers (Fast & Slow)
```go
// Find middle node
func middleNode(head *ListNode) *ListNode {
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    return slow
}

// Detect cycle
func hasCycle(head *ListNode) bool {
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
        if slow == fast {
            return true
        }
    }
    return false
}
```

### Pattern 3: Dummy Node

```go
// Delete a node with specific value
func deleteNode(head *ListNode, val int) *ListNode {
    dummy := &ListNode{Next: head}
    current := dummy

    for current.Next != nil {
        if current.Next.Val == val {
            current.Next = current.Next.Next
            break
        }
        current = current.Next
    }
    return dummy.Next
}
```

### Pattern 4: Reversal

```go
// Reverse entire list
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    current := head

    for current != nil {
        next := current.Next
        current.Next = prev
        prev = current
        current = next
    }
    return prev
}
```

## Common Problems
### Problem 1: Reverse Linked List II (Reverse form position m to n)

```go
func reverseBetween(head *ListNode, m int, n int) *ListNode {
    if head == nil || m == n {
        return head
    }

    dummy := &ListNode{Next: head}
    prev := dummy

    // Move to position before m
    for i := 0; i < m-1; i++ {
        prev = prev.Next 
    }

    // Reverse m to n 
    current := prev.Next
    for i := 0; i < n-m; i++ {
        next := current.Next
        current.Next = next.Next
        next.Next = prev.Next
        prev.Next = next
    }

    return dummy.Next 
}
```

### Problem 2: Remove Nth Node From End

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    fast, slow := dummy, dummy

    // Move fast n steps ahead
    for i := 0; i <= n; i++ {
        fast = fast.Next
    }

    // Move both until fast reaches end
    for fast != nil {
        slow = slow.Next
        fast = fast.Next
    }

    // Remove nth node
    slow.Next = slow.Next.Next

    return dummy.Next
}
```

### Problem 3: Merge Two Sorted Lists

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    current := dummy

    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            current.Next = l1
            l1 = l1.Next
        } else {
            current.Next = l2
            l2 = l2.Next
        }
        current = current.Next
    }

    // Attach remaining nodes
    if l1 != nil {
        current.Next = l1
    }

    if l2 != nil {
        current.Next = l2
    }

    return dummy.Next
}
```

### Problem 4: Intersection of Two Linked Lists

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }

    a, b := headA, headB

    // When one reaches end, continue from other's head
    for a != b {
        if a == nil {
            a = headB
        } else {
            a = a.Next
        }

        if b == nil {
            b = headA
        } else {
            b = b.Next
        }
    }

    return a
}
```

### Problem 5: Odd-Even Reordering

```go
func oddEvenList(head *listNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    odd := head // First odd node
    even := head.Next // First even node
    evenHead := even // Remember where even start

    for even != nil && even.Next != nil {
        // Connect odds
        odd.Next = even.Next // odd -> next odd
        odd = odd.Next // move odd forward

        // Connect evens
        even.Next = odd.Next // even -> next even
        even = even.Next // move even forward
    }

    // Connect old chain to even chain
    odd.Next = evenHead

    return head
}
```

### Problem 6: Twin Sums

```go
func pairSum(head *ListNode) int {
    if head == nil {
        return 0
    }

    // Step 1: Find the middle of the list
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }

    // Step 2: Reverse the second half
    var prev *ListNode
    current := slow
    for current != nil {
        next := current.Next
        current.Next = prev
        prev = current
        current = next
    }

    // Step 3: Calculate twin sums
    first, second := head, prev
    maxSum := 0

    for second != nil {
        sum := first.Val + second.Val
        if sum > maxSum {
            maxSum = sum
        }
        first = first.Next
        second = second.Next
    }

    return maxSum
}
```

## Helper Functions

```go
// Create a linked list from slice
func createList(nums []int) *ListNode {
    if len(nums) == 0 {
        return nil
    }

    head := &ListNode{Val: nums[0]}
    current := head

    for i := 1; i < len(nums); i++ {
        current.Next = &ListNode{Val: nums[i]}
        current = current.Next
    }

    return head
}

// Print linked list
func printList(head *ListNode) {
    for head != nil {
        fmt.Printf("%d", head.Val)
        if head.Next != nil {
            fmt.Print(" -> ")
        }
        head = head.Next
    }
    fmt.Println()
}

// Get length
func getLength(head *ListNode) int {
    length := 0
    for head != nil {
        length++
        head = head.Next
    }
    return length
}
```