# Tree
## [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/#/description)
>Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree [1,null,2,3],
```
   1
    \
     2
    /
   3
```
return [1,3,2].

Note: Recursive solution is trivial, could you do it iteratively?

```python
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res,stack = [],[]
        while True:
            while root:
                stack.append(root)
                root = root.left
            if not stack:
                return res
            root = stack.pop()
            res.append(root.val)
            root = root.right

```

## [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/#/description)
>Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.
```

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3

```
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        return self.core(n,1)

    def core(self,n,start_num):
        if n == 0:
            return []
        ans = []
        for i in range(1,n+1):
            for m in self.core(i-1,start_num) or [None]:
                for k in self.core(n-i,i+start_num) or [None]:
                    root = TreeNode(i+start_num-1)
                    root.left = m
                    root.right = k
                    ans.append(root)
        return ans
```            


## [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/#/description)
>Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

```python
class Solution(object):
    def numTrees(self, n):
        """
        :type n: int
        :rtype: int
        """
        ans = [1]
        for i in range(1,n+1):
            tmp = 0
            for j in range(i):
                tmp += ans[j]*ans[i-j-1]
            ans.append(tmp)
        return ans[-1]
```
## [Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/#/description)
>Given a binary tree, flatten it to a linked list in-place.

For example,
Given
```
         1
        / \
       2   5
      / \   \
     3   4   6
The flattened tree should look like:
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

#使用一个镜像的后序遍历，先保证右树是flatten再把右树移到左树下最后对头结点操作。
class Solution(object):
    def __init__(self):
        self.prev = None
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """
        if not root:
            return None
        self.flatten(root.right)
        self.flatten(root.left)

        root.right = self.prev
        root.left = None
        self.prev = root
```
## [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/#/description)
>Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
```
For example:
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]
```
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        stack1 = [root]
        stack2 = []
        ans = []
        flg = 1
        while stack1 or stack2:
            tmpans = []
            if flg == 1:
                while stack1:
                    tmp = stack1.pop()
                    tmpans.append(tmp.val)
                    if tmp.left:
                        stack2.append(tmp.left)
                    if tmp.right:
                        stack2.append(tmp.right)
                ans.append(tmpans)
                flg = -flg
            elif flg == -1:
                while stack2:
                    tmp = stack2.pop()
                    tmpans.append(tmp.val)
                    if tmp.right:
                        stack1.append(tmp.right)
                    if tmp.left:
                        stack1.append(tmp.left)
                ans.append(tmpans)
                flg = -flg

        return ans            
```
## [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/#/description)
>Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if not inorder or not preorder:
            return None
        root = TreeNode(preorder.pop(0))
        index = inorder.index(root.val)
        root.left = self.buildTree(preorder[:index],inorder[:index])
        root.right = self.buildTree(preorder[index:],inorder[index+1:])
        return root     
```


## [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/#/description)
>Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        if not inorder or not postorder:
            return None
        root = TreeNode(postorder.pop())
        index = inorder.index(root.val)

        root.left = self.buildTree(inorder[:index], postorder[:index])
        root.right = self.buildTree(inorder[index+1:], postorder[index:])
        return root
```

## [Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/#/description)
>Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.
For example,
Given the following binary tree,
```
         1
       /  \
      2    3
     / \    \
    4   5    7
After calling your function, the tree should look like:
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```
```python
# Definition for binary tree with next pointer.
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None

class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        tail = dummy = TreeLinkNode(0)
        while root:
            tail.next = root.left
            if root.left:
                tail = tail.next
            tail.next = root.right
            if root.right:
                tail = tail.next
            root = root.next
            if not root:
                tail = dummy
                root = dummy.next
```
## [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/#/description)
>Given a binary tree

    struct TreeLinkNode {
      TreeLinkNode left;
      TreeLinkNode right;
      TreeLinkNode next;
    }
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
```
Given the following perfect binary tree,
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
After calling your function, the tree should look like:
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```
```python
# Definition for binary tree with next pointer.
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None

class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        if not root:
            return None
        stack = [root]
        while stack:
            lennum = len(stack)
            header = stack.pop(0)
            if header.left:
                stack.append(header.left)
            if header.right:
                stack.append(header.right)
            while lennum > 1:
                tmp = stack.pop(0)
                if tmp.left:
                    stack.append(tmp.left)
                if tmp.right:
                    stack.append(tmp.right)
                header.next = tmp
                header = tmp
                lennum -= 1
```
## [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/#/description)
> Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,
```
    1
   / \
  2   3
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.
```

```python
class Solution(object):
    def sumNumbers(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        self.num = 0
        self.helper(root,0)
        return self.num

    def helper(self,node,sum):
        if node.left:
            self.helper(node.left,sum*10+node.val)
        if node.right:
            self.helper(node.right,sum*10+node.val)
        if not node.left and not node.right:
            self.num += sum*10+node.val
            return
```
## [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/#/description)
>Given a binary tree, return the preorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
```
return [1,2,3].

Note: Recursive solution is trivial, could you do it iteratively?
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        ans = []
        stack = [root]
        while stack:
            tmp = stack.pop()
            if tmp.right:
                stack.append(tmp.right)
            if tmp.left:
                stack.append(tmp.left)
            ans.append(tmp.val)
        return ans

```
## [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/#/description)
>Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

```python
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.stack = []
        self.putall(root)

    def hasNext(self):
        """
        :rtype: bool
        """
        if self.stack:
            return True
        else:
            return False

    def next(self):
        """
        :rtype: int
        """
        tmp = self.stack.pop()
        self.putall(tmp.right)
        return tmp.val

    def putall(self,root):
        while  root:
            self.stack.append(root)
            root = root.left



# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())

```
## [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/#/description)
>Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```
You should return [1, 3, 4].
```python
class Solution(object):
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        ans = []
        stack = [root]
        while stack:
            ans.append(stack[-1].val)
            num = len(stack)
            while num > 0:
                tmp = stack.pop(0)
                if tmp.left:
                    stack.append(tmp.left)
                if tmp.right:
                    stack.append(tmp.right)
                num -= 1
        return ans
```
## [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/#/description)
>Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2**h nodes inclusive at the last level h.

```python
class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        left = self.getDepth(root.left)
        right = self.getDepth(root.right)
        if left == right:
            return pow(2,left) + self.countNodes(root.right)
        else:
            return pow(2,right) + self.countNodes(root.left)

    def getDepth(self,root):
        if not root:
            return 0
        else:
            return 1+self.getDepth(root.left)
```

## [ Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/#/description)
>Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note:
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

```python
class Solution(object):
    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        self.k = k
        self.ans = 0
        self.inorder(root)
        return self.ans

    def inorder(self,root):
        if not root:
            return
        self.inorder(root.left)
        self.k -= 1
        if self.k == 0:
            self.ans = root.val
            return
        self.inorder(root.right)

```
## [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/#/description)
>Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”
```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

```


```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        stack = [root]
        path = {root:None}
        while p not in path or q not in path:
            node = stack.pop()
            if node.left:
                path[node.left] = node
                stack.append(node.left)
            if node.right:
                path[node.right] = node
                stack.append(node.right)
        ancestor = set()
        while p:
            ancestor.add(p)
            p = path[p]
        while q not in ancestor:
            q = path[q]
        return q
```
## [House Robber III](https://leetcode.com/problems/house-robber-iii/#/description)
>The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

Example 1:
```
     3
    / \
   2   3
    \   \
     3   1
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
Example 2:
     3
    / \
   4   5
  / \   \
 1   3   1

Maximum amount of money the thief can rob = 4 + 5 = 9.
```

```python
class Solution(object):
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        def superrob(node):
            # returns tuple of size two (now, later)
            # now: max money earned if input node is robbed
            # later: max money earned if input node is not robbed

            # base case
            if not node: return (0, 0)

            # get values
            left, right = superrob(node.left), superrob(node.right)

            # rob now
            now = node.val + left[1] + right[1]

            # rob later
            later = max(left) + max(right)

            return (now, later)

        return max(superrob(root))
```
## [Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst/#/description)
>Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```python
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.

        :type root: TreeNode
        :rtype: str
        """
        def preorder(node):
            if node:
                vals.append(str(node.val))
                preorder(node.left)
                preorder(node.right)
        vals = []
        preorder(root)
        return ' '.join(vals)

    def deserialize(self, data):
        """Decodes your encoded data to tree.

        :type data: str
        :rtype: TreeNode
        """
        preorder = map(int,data.split())
        inorder = sorted(preorder)
        return self.buildTree(preorder,inorder)

    def buildTree(self,preorder,inorder):
        def build(stop):
            if inorder and inorder[-1] != stop:
                root = TreeNode(preorder.pop())
                root.left = build(root.val)
                inorder.pop()
                root.right = build(stop)
                return root
        preorder.reverse()
        inorder.reverse()
        return build(None)




# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))

```
## [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/#/description)
>Total Accepted: 11456
Total Submissions: 32384
Difficulty: Medium
Contributors: tsipporah5945
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
Note: Time complexity should be O(height of tree).

Example:

root = [5,3,6,2,4,null,7]
key = 3
```
    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7
```
```python
class Solution(object):
    def deleteNode(self, root, key):
        """
        :type root: TreeNode
        :type key: int
        :rtype: TreeNode
        """
        if not root: return None

        if root.val == key:
            if root.left:
                # Find the right most leaf of the left sub-tree
                left_right_most = root.left
                while left_right_most.right:
                    left_right_most = left_right_most.right
                # Attach right child to the right of that leaf
                left_right_most.right = root.right
                # Return left child instead of root, a.k.a delete root
                return root.left
            else:
                return root.right
        # If left or right child got deleted, the returned root is the child of the deleted node.
        elif root.val > key:
            root.left = self.deleteNode(root.left, key)
        else:
            root.right = self.deleteNode(root.right, key)

        return root
```


## [Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/#/description)
>Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.
```
Examples 1
Input:

  5
 /  \
2   -3
return [2, -3, 4], since all the values happen only once, return all of them in any order.
Examples 2
Input:

  5
 /  \
2   -5
return [2], since 2 happens twice, however -5 only occur once.
Note: You may assume the sum of values in any subtree is in the range of 32-bit signed integer.
```

```python
from collections import defaultdict
class Solution(object):
    def findFrequentTreeSum(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        self.index = defaultdict(int)
        self.maxnum = 0
        self.helper(root)
        return [i for i,v in self.index.items() if v == self.maxnum]


    def helper(self,root):
        if not root :
            return 0
        root.val += self.helper(root.left)+self.helper(root.right)
        self.index[root.val] += 1
        self.maxnum = max(self.maxnum,self.index[root.val])
        return root.val
```

## [Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/#/description)
>Given a binary tree, find the leftmost value in the last row of the tree.

Example 1:
Input:
```
    2
   / \
  1   3

Output:
1
Example 2:
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```
Note: You may assume the tree (i.e., the given root node) is not NULL.

```python
class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return
        stack = [root]
        while stack:
            num = len(stack)
            for i in range(num):
                m = stack.pop(0)
                if i==0:
                    ans = m.val
                if m.left:
                    stack.append(m.left)
                if m.right:
                    stack.append(m.right)
        return ans
```

## [Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/#/description)
>You need to find the largest value in each row of a binary tree.

Example:
Input:
```
          1
         / \
        3   2
       / \   \  
      5   3   9
```

Output: [1, 3, 9]

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def largestValues(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = [root]
        ans = []

        while stack:
            num = len(stack)
            for i in range(num):
                m = stack.pop(0)
                if i==0:
                    tmpmax = m.val
                else:
                    tmpmax =max(tmpmax,m.val)
                if m.left:
                    stack.append(m.left)
                if m.right:
                    stack.append(m.right)
            ans.append(tmpmax)
        return ans
```


## [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/?tab=Description)
>Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example 1:
```
    2
   / \
  1   3
```
Binary tree [2,1,3], return true.
Example 2:
```
    1
   / \
  2   3
```
Binary tree [1,2,3], return false.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        output = []
        self.inOrder(root,output)
        for i in range(1,len(output)):
            if output[i-1] >= output[i]:
                return False
        return True
    def inOrder(self,root,output):
        if not root:
            return
        self.inOrder(root.left,output)
        output.append(root.val)
        self.inOrder(root.right,output)

```
