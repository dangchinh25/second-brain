## Pattern
### Dummy Head
- Saves you from creating special edge condition logic in order to operate on the head of a linked list
- Only involves creating one extra dummy node pointing to the final answer or list that you will return

Example: Delete a node in a linked list given the value of the node you want to delete
```python
class Node(object):
    def __init__(self, v):
        self.val = v
        self.next = None
    
def delete_node(head, val):
    d = Node("dummy") # 1
    d.next = head
    p = d
    c = head
    while c:
        if c.val == val:
            p.next = c.next # 2
            return d.next   # 3
        p = c
        c = c.next
    return d.next # 4
```
- The creation of the dummy head, in this case it is initialized to point to the original list
- Because we created the dummy head we don't have to treat deleting the head of the original list any different from other elements in the list
- The dummy head points to the answer list so we simply return the next node as the head of the answer
- If we don't happen to find the item setting the dummy head to the original list makes it still point to the correct answer

### Multiple Pass Technique
- Most computation on a list will require *O(N)* time complexity, so a simple but very technique is to **pass through the list some number of times to calculate some summary of the list that will simplify your algorithm.**
- One example that we see a lot is the need to calculate the length of the list

Example: Find the intersection of 2 list
- We can iterate through the 2 list, find out the length of the 2 list
- Compare the length of the 2 list, whichever is longer will be strip down so that both list will have the same length
- Now the problem of iterating 2 list at the same time to find the intersection (node with same value) is trivial

### Linked List Two Pointer
- Normal two pointer
- [[14 Patterns DSA#3. Fast and Slow pointers | Fast and Slow Pointer]]

Example: Detect cycle in Linked List using Fast and Slow pointer
```python
def hasCycle(self, head: Optional[ListNode]) -> bool:
        if head is None or head.next is None:
            return False
        
        slow, fast = head, head
        while fast.next is not None and fast.next.next is not None:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
            
        return False
```
- The intuition here is that if there is a cycle, then the list can be thought of as a circle. Similar to a race track, the fast pointer must eventually cross paths with the slower pointer, whereas if there is not a cycle they will never cross paths.

### Reverses a Linked List
- This not really a pattern or technique, but it is a LC problem on its own
- However, there is a lot of Linked List problem can be solved using reverse a list or a portion of the list
- **Almost need to memorize this**
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        
        while head:
            cur_next = head.next
            head.next = prev
            prev = head
            head = cur_next
        
        return prev
```

## Example
1.**Middle of the linked List**: Given a non empty singly linked list with head node head, return a middle node of linked list
**Intuition**:  ^af80bb
1. We can use [[Linked List#Linked List Two Pointer | Two Pointer]] moving at different speed, one move twice at fast at the other so when the fast one reach the end, the slow one will be the middle node
```python
def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
        
        slow, fast = head, head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        return slow
```

2. **Palindrome Linked**: Given a singly linked list, determine if it is a palindrome
**Intuition**:
1. A palindrome is the same reading forward and backward, or in other word, can be divided into 2 part with the later being the reverse of the former
2. With the above understanding in mind, we can 
	1. Find the middle node of the linked list ([[#^af80bb | Middle of the linked list]])
	2. Reverse the second part ([[Linked List#Reverses a Linked List | Reverse a Linked list]])
	3. Start from the beginning of 2 list, check if they are similar

3. **Reorder List**: Given a singly linked list: 0 -> 1 -> 2 -> ... -> n. Reorder it to: 0 -> n -> 2 -> n-1 -> ... (Example: 1-2-3-4 reorder to 1-4-2-3)
**Intuition**:
1. We can see that it is similar to having two pointer, one going forward and one going backward and connect the two node at the two pointer together. However we can't going backward in a singly linked list. But we can see that if we keep moving backward and forward the pointers will meet at the middle of the list and then stop there.
2. We can find the [[#^af80bb | middle of the linked list]]
3. Reverse the second part ([[Linked List#Reverses a Linked List | Reverse a Linked list]])
4. Starting from the beginning of 2 list, merge them alternatively.


## References
- https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc