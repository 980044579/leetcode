# Linked List

## [Sort List](https://leetcode.com/problems/sort-list/?tab=Description)
>Sort a linked list in O(n log n) time using constant space complexity.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def sortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head
        pre,slow,fast = None,head,head
        while fast and fast.next:
             pre,slow, fast = slow, slow.next, fast.next.next

        pre.next = None  #把链断开！
        return self.merge(*map(self.sortList, (head, slow)))



    def merge(self,h1,h2):
        dummy = tail = ListNode(None)
        while h1 and h2:
            if h1.val < h2.val:
                tail.next,tail,h1 = h1,h1,h1.next
            else:
                tail.next,tail,h2 = h2,h2,h2.next
        tail.next = h1 or h2
        return dummy.next

```
## [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/?tab=Description)

>Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

Example:
Given 1->2->3->4->5->NULL,
return 1->3->5->2->4->NULL.

Note:
The relative order inside both the even and odd groups should remain as it was in the input.
The first node is considered odd, the second node even and so on ...


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def oddEvenList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head
        p1,odd,p2,even = head,head,head.next,head.next
        while p2 and p2.next:
            p1.next = p2.next
            p1 = p2.next
            p2.next = p1.next
            p2 = p1.next
        p1.next = even
        return odd

```
## [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/?tab=Description)

>Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head:
            return head
        if not head.next:
            return head
        dummy = head.next
        cur = ListNode(0)
        while head and head.next:
            cur.next = head.next
            tmp = head.next
            head.next = tmp.next
            tmp.next = head
            cur = head
            head = head.next
        return dummy

```
