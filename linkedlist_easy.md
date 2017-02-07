# Linked List

## [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

>Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,

Given 1->1->2, return 1->2.

Given 1->1->2->3->3, return 1->2->3.

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
        if head == None:
            return head
        node = head
        while node.next:
            if node.val == node.next.val:
                node.next = node.next.next
            else:
                node = node.next
        return head

```
## [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

>Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:

```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.


Notes:

If the two linked lists have no intersection at all, return null.

The linked lists must retain their original structure after the function returns.

You may assume there are no cycles anywhere in the entire linked structure.

Your code should preferably run in O(n) time and use only O(1) memory.


```python
#l1+l2和l2+l1的长度一样，最后如果有交汇，总会同时到达交汇点
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        flg = False #判断是否已经从链A切换到了链B
        p1,p2 = headA,headB
        while p1 and p2:
            if p1 == p2:
                return p1
            p1,p2 = p1.next,p2.next
            if not p1 and not flg:
                p1 = headB
                flg = True
            if not p2 :
                p2 = headA
        return None


```
## [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)
>Remove all elements from a linked list of integers that have value val.

Example

Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6

Return: 1 --> 2 --> 3 --> 4 --> 5

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        dummy = pre = ListNode(0)
        pre.next = head
        while head:
            if head.val == val:
                pre.next = head.next
                head = head.next
            else:
                pre = pre.next
                head = head.next
        return dummy.next
```

## [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/?tab=Description)
>Given a singly linked list, determine if it is a palindrome.

Solution :

Reversed first half == Second half?

Phase 1: Reverse the first half while finding the middle.

Phase 2: Compare the reversed first half with the second half.


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        rev = None
        slow=fast=head
        while fast and fast.next:
            fast = fast.next.next
            cur = slow
            slow = slow.next
            cur.next=rev
            rev = cur
        if fast:
            slow = slow.next
        while rev and rev.val == slow.val:
            rev = rev.next
            slow = slow.next
        return not rev

```
## [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
>Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None


# iteratively
class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        dummy=cur=ListNode(0)
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        cur.next = l1 or l2
        return dummy.next

# recursively

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if not l1 or not l2:
            return l1 or l2
        if l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1,l2.next)
            return l2

```
## [Submission Details](https://leetcode.com/submissions/detail/91702382/)
>Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

#设置两个点，一个快一个慢，如果有圈，则快总会追上慢，否则快点先到链尾
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head == None or head.next == None:
            return False
        slow = head
        fast = head.next
        while slow != fast:
            if fast == None or fast.next == None:
                return False
            slow = slow.next
            fast = fast.next.next
        return True

#使用hash表
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        dic = {}
        while head:
            if dic.has_key(head):
                return True
            else:
                dic[head] = 1
            head = head.next
        return False
```
## [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

>Reverse a singly linked list.

click to show more hints.

Hint:
A linked list can be reversed either iteratively or recursively. Could you implement both?

```python
#递归
class Solution:
# @param {ListNode} head
# @return {ListNode}
def reverseList(self, head):
    prev = None
    while head:
        curr = head
        head = head.next
        curr.next = prev
        prev = curr
    return prev

#迭代

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        return self._reverse(head)

    def _reverse(self,node,pre=None):
        if not node:
            return pre
        n = node.next
        node.next = pre

        return self._reverse(n,node)
```
## [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

>Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

```python


# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution(object):
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next


```
