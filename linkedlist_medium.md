# Linked List

## [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/?tab=Description)
>You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        carry = 0
        res = n = ListNode(0)
        while l1 or l2 or carry:
            if l1:
                carry += l1.val
                l1 = l1.next
            if l2:
                carry += l2.val
                l2 = l2.next
            carry,val = divmod(carry,10)
            n.next = n = ListNode(val)
        return res.next

```
## [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/?tab=Description)
>Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:
Given n will always be valid.
Try to do this in one pass.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        pre = ListNode(0)
        pre.next = head
        fast = slow = pre
        for i in range(n):
            fast = fast.next
        while fast and fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return pre.next

```

## [Rotate List](https://leetcode.com/problems/rotate-list/?tab=Description)
>Given a list, rotate the list to the right by k places, where k is non-negative.

For example:
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        while not head :
            return head
        #计算链长   
        n = 0
        p1 = head
        while p1.next:
            n += 1
            p1 = p1.next
        m = k%(n+1)
        if m==0:
            return head
        #旋转
        pre = ListNode(0)
        pre.next = head

        p2 = head
        for i in range(n-m):
            p2 = p2.next
        dummy = p2.next
        p2.next = None
        p1.next = head
        return dummy
```
## [ Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/?tab=Description)
>Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given 1->2->3->3->4->4->5, return 1->2->5.
Given 1->1->1->2->3, return 2->3.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = pre = ListNode(0)
        dummy.next = head
        while head and head.next:
            if head.val == head.next.val:
                while head and head.next and head.val == head.next.val:
                    head = head.next
                head = head.next
                pre.next = head
            else:
                pre = pre.next
                head = head.next
        return dummy.next
```
## [Partition List](https://leetcode.com/problems/partition-list/?tab=Description)
>Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given 1->4->3->2->5->2 and x = 3,
return 1->2->2->4->3->5.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        h1 = l1 = ListNode(0)
        h2 = l2 = ListNode(0)
        while head:
            if head.val < x:
                l1.next = head
                l1 = l1.next
            else:
                l2.next = head
                l2 = l2.next
            head = head.next
        l1.next = h2.next
        l2.next = None
        return h1.next
```
## [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/?tab=Description)
>Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,

return 1->4->3->2->5->NULL.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """

        if m == n:
            return head
        node = head
        nodeans = pre0 = ListNode(0)
        pre0.next = head
        num = 0
        while m-1 >num:
            pre0 = pre0.next
            num += 1
        pre = pre0
        pretmp = None
        end = start = pre.next
        while num<n:
            tmp = start
            start = start.next
            tmp.next = pretmp
            pretmp = tmp
            num += 1
        pre0.next = pretmp
        end.next = start
        return nodeans.next

```
## [Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/?tab=Description)
>Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def sortedListToBST(self, head):
        """
        :type head: ListNode
        :rtype: TreeNode
        """
        if not head :
            return
        if not head.next:
            return TreeNode(head.val)

        slow,fast = head,head.next.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        tmp = slow.next
        slow.next = None
        root = TreeNode(tmp.val)
        root.left = self.sortedListToBST(head)
        root.right = self.sortedListToBST(tmp.next)
        return root

```
## [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/?tab=Description)
>A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

```python
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """
        p = p1 = p2 = head
        while p:
            Clone = RandomListNode(p.label)
            Clone.next = p.next
            Clone.random = None
            p.next = Clone
            p = Clone.next
        while p1:
            Clone = p1.next
            if p1.random != None:
                Clone.random = p1.random.next
            p1 = Clone.next
        ClonedHead = None
        ClonedNode = None
        if p2:
            ClonedHead = ClonedNode = p2.next
            p2.next = ClonedNode.next
            p2 = p2.next
        while p2 :
            ClonedNode.next = p2.next
            ClonedNode = ClonedNode.next
            p2.next = ClonedNode.next
            p2 = p2.next
        return ClonedHead
```
## [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/?tab=Description)
>Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:
Can you solve it without using extra space?

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        slow = fast = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                break
        else:
            return None
        while head != slow:
            head = head.next
            slow = slow.next
        return head
```
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
