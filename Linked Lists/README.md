# Linked Lists

## [Insert a node at a specific position in a linked list](https://www.hackerrank.com/challenges/insert-a-node-at-a-specific-position-in-a-linked-list/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists)

```go
func insertNodeAtPosition(llist *SinglyLinkedListNode, data int32, position int32) *SinglyLinkedListNode {
    if position == 0 {
        return &SinglyLinkedListNode{data, llist}
    }
    tmp := llist
    for i := int32(0); i < position - 1; i++ {
        if tmp.next != nil {
            tmp = tmp.next
        } else {
            break
        }
    }
    tmp.next = &SinglyLinkedListNode{data, tmp.next}
    return llist
}
```

## [Inserting a Node Into a Sorted Doubly Linked List](https://www.hackerrank.com/challenges/insert-a-node-into-a-sorted-doubly-linked-list/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists)

```go
func sortedInsert(head *DoublyLinkedListNode, data int32) *DoublyLinkedListNode {
    node := &DoublyLinkedListNode{
        data: data,
        next: head,
        prev: nil,
    }
    if head.data > data {
        head.prev = node
        return node
    }

    temp := head
    for temp.next != nil && temp.next.data < data {
        temp = temp.next
    }
    tempNext := temp.next
    
    node.next = tempNext
    node.prev = temp

    temp.next = node

    if tempNext != nil {
        tempNext.prev = node
    }

    return head
}
```

## [Reverse a doubly linked list](https://www.hackerrank.com/challenges/reverse-a-doubly-linked-list/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists)

```go
func reverse(head *DoublyLinkedListNode) *DoublyLinkedListNode {
    temp := head
    data := []int32{temp.data}
    for temp.next != nil {
        temp = temp.next
        data = append(data, temp.data)
    }

    temp = head
    for i := len(data) - 1; i >= 0; i-- {
        temp.data = data[i]
        temp = temp.next
    }

    return head
}
```

## [Find Merge Point of Two Lists](https://www.hackerrank.com/challenges/find-the-merge-point-of-two-joined-linked-lists/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists)

```cpp
int findMergeNode(SinglyLinkedListNode* head1, SinglyLinkedListNode* head2) {
    SinglyLinkedListNode* temp1 = head1;
    while (temp1 != NULL) {
        SinglyLinkedListNode* temp2 = head2;
        while (temp2 != NULL) {
            if (temp1 == temp2) {
                return temp1->data;
            }
            temp2 = temp2->next;
        }
        temp1 = temp1->next;
    }
    return 0;
}
```

## [Linked Lists: Detect a Cycle](https://www.hackerrank.com/challenges/ctci-linked-list-cycle/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=linked-lists)

```cpp
#include <unordered_set>;

bool has_cycle(Node* head) {
    if (head == NULL) {
        return false;
    }
    unordered_set<Node*> visited;
    Node* temp = head;
    while (temp != NULL) {
        if (visited.find(temp) != visited.end()) {
            return true;
        }
        visited.insert(temp);
        temp = temp->next;
    }
    return false;
}
```
