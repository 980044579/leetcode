# Tree

## [Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/?tab=Description)
>Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
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
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
          return []
        ans = []
        stack = [root]
        while stack:
            num = len(stack)
            tmp = []
            for i in range(num):
                m = stack.pop(0)
                if m.left:
                    stack.append(m.left)
                if m.right:
                    stack.append(m.right)
                tmp.append(m.val)
            ans.insert(0,tmp)
        return ans

```
## [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/?tab=Description)
>Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def sortedArrayToBST(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        if not nums:
            return
        mid = len(nums)//2
        node = TreeNode(nums[mid])
        node.left = self.sortedArrayToBST(nums[:mid])
        node.right = self.sortedArrayToBST(nums[mid+1:])
        return node
```
## [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/?tab=Description)
>Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root :
            return True
        return abs(self.maxdepth(root.left)-self.maxdepth(root.right))<2 and self.isBalanced(root.left) and self.isBalanced(root.right)
    def maxdepth(self,root):
        if not root:
            return 0
        return 1+max(self.maxdepth(root.left),self.maxdepth(root.right))

```
## [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/?tab=Description)
>Find the sum of all left leaves in a given binary tree.

Example:
```
    3
   / \
  9  20
    /  \
   15   7
There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

```python

# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def sumOfLeftLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        if root.left and not root.left.left and not root.left.right:
            return root.left.val+self.sumOfLeftLeaves(root.right) #node is a leave
        else:
            return self.sumOfLeftLeaves(root.left) + self.sumOfLeftLeaves(root.right) #node is not a leave


```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
## [Path Sum III](https://leetcode.com/problems/path-sum-iii/?tab=Description)
>You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
```

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1
```

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11

```python

##暴力方法，穷尽所有路径返回总符合条件路径
class Solution(object):
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: int
        """
        if root :
            return self.findPath(root,sum)+self.pathSum(root.left,sum)+self.pathSum(root.right,sum)
        return 0
    def findPath(self,root,target):
        if root :
            return int(root.val == target) + self.findPath(root.left,target-root.val) + self.findPath(root.right,target-root.val)
        return 0


```
```java
    高效法
    public int pathSum(TreeNode root, int sum) {
        HashMap<Integer, Integer> preSum = new HashMap();
        preSum.put(0,1);
        helper(root, 0, sum, preSum);
        return count;
    }
    int count = 0;
    public void helper(TreeNode root, int currSum, int target, HashMap<Integer, Integer> preSum) {
        if (root == null) {
            return;
        }

        currSum += root.val;

        if (preSum.containsKey(currSum - target)) {
            count += preSum.get(currSum - target);
        }

        if (!preSum.containsKey(currSum)) {
            preSum.put(currSum, 1);
        } else {
            preSum.put(currSum, preSum.get(currSum)+1);
        }

        helper(root.left, currSum, target, preSum);
        helper(root.right, currSum, target, preSum);
        preSum.put(currSum, preSum.get(sum) - 1);
    }
```
## [Same Tree](https://leetcode.com/problems/same-tree/?tab=Description)
>Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        if not p and not q:
            return True
        elif p and q and p.val == q.val:
            left = self.isSameTree(p.left, q.left)
            right = self.isSameTree(p.right, q.right)
        else:
            return False
        return left and right
```

##  [Path Sum](https://leetcode.com/problems/path-sum/?tab=Description)
>Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        nleft = nright = False
        if not root :
            return False
        elif not root.left and not root.right and root.val == sum:
            return True
        else:
            if root.left:
                nleft = self.hasPathSum(root.left, sum-root.val)
            if root.right:
                nright = self.hasPathSum(root.right, sum-root.val)
        return nleft or nright
```
## [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/?tab=Description)
>Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        left = self.maxDepth(root.left)
        right = self.maxDepth(root.right)
        return max(left+1,right+1)
```
## [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/?tab=Description)
>Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:
```
   1
 /   \
2     3
 \
  5
```
All root-to-leaf paths are:

["1->2->5", "1->3"]

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        if not root:
            return []
        if not root.left and not root.right:
            return [str(root.val)]
        treepaths = [str(root.val) + '->' + path for path in self.binaryTreePaths(root.left)]
        treepaths += [str(root.val) + '->' + path for path in self.binaryTreePaths(root.right)]
        return treepaths
```
