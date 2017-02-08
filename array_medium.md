# Array_medium

## [3Sum Closest](https://leetcode.com/problems/3sum-closest/)
>Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        ans = nums[0]+nums[1]+nums[2]
        nums.sort()
        for i in range(len(nums)-2):
            if i>0 and nums[i] == nums[i-1]:
                continue
            l,r = i+1,len(nums)-1
            while l<r:
                s = nums[i]+nums[l]+nums[r]
                if abs(ans - target)> abs(s - target):
                    ans = s
                if s > target:
                    r -= 1
                if s < target:
                    l += 1
                if s == target:
                    return s
        return ans
```
## [3Sum](https://leetcode.com/problems/3sum/)
>Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```python
def threeSum(self, nums):
    res = []
    nums.sort()
    for i in xrange(len(nums)-2):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        l, r = i+1, len(nums)-1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s < 0:
                l +=1
            elif s > 0:
                r -= 1
            else:
                res.append((nums[i], nums[l], nums[r]))
                while l < r and nums[l] == nums[l+1]:
                    l += 1
                while l < r and nums[r] == nums[r-1]:
                    r -= 1
                l += 1; r -= 1
    return res
```
## [Search for a Range](https://leetcode.com/problems/search-for-a-range/)
>Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].

```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return [-1,-1]
        ret = [-1,-1]
        lo,hi = 0,len(nums)-1
        while lo < hi:
            mid = (lo+hi)//2
            if nums[mid] < target:
                lo = mid + 1
            else:
                hi = mid
        if target != nums[lo]:
            return ret
        ret[0] = lo
        hi = len(nums)-1
        while lo < hi:
            mid = (lo + hi )//2+1
            if nums[mid] > target:
                hi = mid -1
            else:
                lo = mid
        ret[1] = hi
        return ret
```

## [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
>Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0

```python
class Solution(object):
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        lo,hi = 0,len(nums)-1
        while lo <= hi :
            mid = lo + (hi - lo)//2
            if target < nums[mid] :
                hi = mid - 1
            elif target > nums[mid]:
                lo = mid + 1
            else:
                return mid
        return lo

```
## [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
>Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        candidates = sorted(candidates)
        res = []
        self.dfs(candidates,target,0,[],res)
        return res

    def dfs(self,nums,target,index,path,res):
        if target == 0:
            res.append(path)
            return
        for i in range(index,len(nums)):
            if (i>index and nums[i]==nums[i-1]):
                continue
            if nums[i] > target:
                break
            self.dfs(nums,target-nums[i],i+1,path+[nums[i]],res)

```
## [Combination Sum](https://leetcode.com/problems/combination-sum/)
>Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[
  [7],
  [2, 2, 3]
]

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        res = []
        self.dfs(candidates,target,0,[],res)
        return res
    def dfs(self,candidates,target,index,path,res):
        if target < 0:
            return
        if target ==0:
            res.append(path)
            return
        for i in range(index,len(candidates)):
            self.dfs(candidates,target-candidates[i],i,path+[candidates[i]],res)    
```
## [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
>Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

>Tip:
>this problem was discussed by Jon Bentley (Sep. 1984 Vol. 27 No. 9 Communications of the ACM P885)

>the paragraph below was copied from his paper (with a little modifications)

>algorithm that operates on arrays: it starts at the left end (element A[1]) and scans through to the right end (element A[n]), keeping track of the maximum sum subvector seen so far. The maximum is initially A[0]. Suppose we've solved the problem for A[1 .. i - 1]; how can we extend that to A[1 .. i]? The maximum
sum in the first I elements is either the maximum sum in the first i - 1 elements (which we'll call MaxSoFar), or it is that of a subvector that ends in position i (which we'll call MaxEndingHere).

>MaxEndingHere is either A[i] plus the previous MaxEndingHere, or just A[i], whichever is larger.

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        maxSoFar,maxEndingHere = nums[0],nums[0]
        for i in range(1,len(nums)):
            maxEndingHere = max(maxEndingHere+nums[i],nums[i])
            maxSoFar = max(maxSoFar,maxEndingHere)
        return maxSoFar

```
## [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

>Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

For example,
Given n = 3,

You should return the following matrix:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]


Solution 1: Build it inside-out - 44 ms, 5 lines

Start with the empty matrix, add the numbers in reverse order until we added the number 1. Always rotate the matrix clockwise and add a top row:

    ||  =>  |9|  =>  |8|      |6 7|      |4 5|      |1 2 3|
                     |9|  =>  |9 8|  =>  |9 6|  =>  |8 9 4|
                                         |8 7|      |7 6 5|

```python
class Solution(object):
  def generateMatrix(self, n):
      A, lo = [], n*n+1
      while lo > 1:
          lo, hi = lo - len(A), lo
          A = [range(lo, hi)] + zip(*A[::-1])
      return A
```
## [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
>Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,
Given the following matrix:

[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
You should return [1,2,3,6,9,8,7,4,5].

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        ret = []
        while matrix:
            ret += matrix.pop(0)
            if matrix and matrix[0]:
                for row in matrix:
                    ret.append(row.pop())
            if matrix:
                ret += matrix.pop()[::-1]

            if matrix and matrix[0]:
                for row in matrix[::-1]:
                    ret.append(row.pop(0))
        return ret


```
## [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)
>Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
The total number of unique paths is 2.

Note: m and n will be at most 100.

```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        resGrid = [[0 for i in range(n+1)] for j in range(m+1)]
        resGrid[0][1] = 1

        for i in range(1,m+1):
            for j in range(1,n+1):
                if not obstacleGrid[i-1][j-1]:
                    resGrid[i][j] = resGrid[i-1][j]+resGrid[i][j-1]
        return resGrid[m][n]          

```
## [Unique Paths](https://leetcode.com/problems/unique-paths/)
>A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?


Above is a 3 x 7 grid. How many possible unique paths are there?

```python
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        aux = [[1 for x in range(n)] for x in range(m)]
        for i in range(1,m):
            for j in range(1,n):
                aux[i][j] = aux[i-1][j]+aux[i][j-1]
        return aux[m-1][n-1]


```
## [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
>Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

```python
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        for i in range(1,n):
            grid[0][i] += grid[0][i-1]
        for j in range(1,m):
            grid[j][0] += grid[j-1][0]
        for i in range(1,m):
            for j in range(1,n):
                grid[i][j] += min(grid[i-1][j],grid[i][j-1])
        return grid[-1][-1]
```
## [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
>Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

```python
class Solution(object):
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        col0 = 1
        rows = len(matrix)
        cols = len(matrix[0])
        for i in range(rows):
            if matrix[i][0] == 0:
                col0 = 0
            for j in range(1,cols):
                if matrix[i][j] == 0:
                    matrix[0][j],matrix[i][0] = 0,0

        for i in range(rows-1,-1,-1):
            for j in range(cols-1,0,-1):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
            if col0 == 0:
                matrix[i][0] = 0
```
## [Sort Colors](https://leetcode.com/problems/sort-colors/)
>Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:
You are not suppose to use the library's sort function for this problem.

click to show follow up.

Subscribe to see which companies asked this question

Show Tags
Show Similar Problems

```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        i,j = 0,0
        for k in range(len(nums)):
            v = nums[k]
            nums[k] = 2
            if v < 2:
                nums[j] = 1
                j += 1
            if v == 0:
                nums[i] = 0
                i += 1

```
## [Subsets](https://leetcode.com/problems/subsets/)
>Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:

[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

```python
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if not nums:
            return []
        out = [[]]
        for num in nums:
            n = len(out)
            for i in range(n):
                out.append(out[i]+[num])
        return out

```
## [ Word Search](https://leetcode.com/problems/word-search/)
>Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given board =

[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.

```python
def exist(self, board, word):
    """
    :type board: List[List[str]]
    :type word: str
    :rtype: bool
    """
    if not board:
        return False
    for i in xrange(len(board)):
        for j in xrange(len(board[0])):
            if self.dfs(board, i, j, word):
                return True
    return False

# check whether can find word, start at (i,j) position    
class Solution(object):
  def dfs(self, board, i, j, word):
      if len(word) == 0: # all the characters are checked
          return True
      if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or word[0]!=board[i][j]:
          return False
      tmp = board[i][j]  # first character is found, check the remaining part
      board[i][j] = "#"  # avoid visit agian
      # check whether can find "word" along one direction
      res = self.dfs(board, i+1, j, word[1:]) or self.dfs(board, i-1, j, word[1:]) \
      or self.dfs(board, i, j+1, word[1:]) or self.dfs(board, i, j-1, word[1:])
      board[i][j] = tmp
      return res
```
## [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
>Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i = 0
        for n in nums:
            if i < 2 or n > nums[i-2]:#已经排好序，所以只需要比较n和nums[i-2]的大小
                nums[i] = n
                i+=1
        return i
```
## [Subsets II](https://leetcode.com/problems/subsets-ii/)
>Given a collection of integers that might contain duplicates, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,2], a solution is:

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = [[]]
        nums.sort()
        for i in range(len(nums)):
            if i == 0 or nums[i] != nums[i-1]:
                l = len(res)
            for j in range(len(res)-l,len(res)):
                res.append(res[j]+[nums[i]])
        return res
```


## [Triangle](https://leetcode.com/problems/triangle/)
>Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

Note:
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

```python
class Solution(object):
    def minimumTotal(self, triangle):
        """
        :type triangle: List[List[int]]
        :rtype: int
        """
        if not triangle:
                return
        for i in xrange(len(triangle)-2, -1, -1):
            for j in xrange(len(triangle[i])):
                triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1])#自底向上进行一个循环
        return triangle[0][0]
```
## [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

>Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.

```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        imax,imin,r = nums[0],nums[0],nums[0]
        for i in range(1,n):
            if nums[i]<0:
                swap = imax
                imax = imin
                imin = swap
            imax = max(nums[i],imax*nums[i]) #如果0的话，将之前的记录全部清空，但是r中保持了之前的最大值
            imin = min(nums[i],imin*nums[i])

            r = max(r,imax)
        return r
```
## [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

>Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.
```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        r = len(nums)-1
        l = 0
        mid = (r+l)//2
        if r == 0:
            return nums[0]
        while mid >l:

            if nums[mid]>nums[mid+1]:
                return nums[mid+1]
            elif nums[mid]>nums[l]:
                l = mid
            else:
                r = mid
            mid = (r+l)//2
        return min(nums[0],nums[1])

```
## [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
>Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array [2,3,1,2,4,3] and s = 7,
the subarray [4,3] has the minimal length under the problem constraint.

```python
class Solution(object):
    def minSubArrayLen(self, s, nums):
        """
        :type s: int
        :type nums: List[int]
        :rtype: int
        """
        count,l,r,res,n= 0,0,0,float('inf'),len(nums)
        for r in range(n):
            count += nums[r]
            while l <= r and count>= s:
                res = min(res,r-l+1)
                count -= nums[l]
                l += 1
        return res if res < float('inf') else 0
```
## [ Find Peak Element](https://leetcode.com/problems/find-peak-element/)
>A peak element is an element that is greater than its neighbors.

>Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.

>The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

>You may imagine that num[-1] = num[n] = -∞.

```python
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n ==1 :
            return 0
        if n ==0 :
            return

        mid,l,h= 0,0,n-1
        while l<h:
            mid = (l+h)//2
            if nums[mid] > nums[mid+1]:
                h = mid
            elif nums[mid] < nums[mid+1]:
                l = mid + 1
        return l
```

For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.
## [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)
>Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Example 1:

Input: k = 3, n = 7

Output:

[[1,2,4]]

Example 2:

Input: k = 3, n = 9

Output:

[[1,2,6], [1,3,5], [2,3,4]]

```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """
        stack = [(0,1,[])] # total,start,comb
        ans = []
        while stack:
            total,start,comb = stack.pop()
            if total == n and len(comb) == k:
                ans.append(comb)
                continue
            for i in range(start,10):
                tmp_total = total + i
                if tmp_total>n:
                    break
                stack.append((tmp_total,i+1,comb+[i]))
        return ans
```
## [Summary Ranges](https://leetcode.com/problems/summary-ranges/)
>Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"].

```python
def summaryRanges(self, nums):
    ranges = []
    for n in nums:
        if not ranges or n > ranges[-1][-1] + 1:
            ranges += [],
        ranges[-1][1:] = n,
    return ['->'.join(map(str, r)) for r in ranges]
```

## [Majority Element II](https://leetcode.com/problems/majority-element-ii/)
>Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times. The algorithm should run in linear time and in O(1) space.

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        count1,count2,candidate1,candidate2 = 0,0,0,1
        for n in nums:
            if n == candidate1 :
                count1 += 1
            elif n == candidate2 :
                count2 +=1
            elif count1 == 0:
                candidate1, count1 = n , 1
            elif count2 == 0:
                candidate2, count2 = n , 1
            else:
                count1,count2 = count1 - 1,count2 -1
        return [n for n in (candidate1,candidate2) if nums.count(n)>len(nums)//3]
```

## [Game of Life](https://leetcode.com/problems/game-of-life/)
>According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

>Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

>ny live cell with fewer than two live neighbors dies, as if caused by under-population.
Any live cell with two or three live neighbors lives on to the next generation.
Any live cell with more than three live neighbors dies, as if by over-population..
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state.

>Follow up:
Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

```python
class Solution(object):
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: void Do not return anything, modify board in-place instead.

        2  dead -> live
        3  live -> dead
        """

        m = len(board)
        n = len(board[0])

        if m ==0 or n == 0:
            return
        for iM in range(m):
            for iN in range(n):
                numNeighbor = sum([board[i][j]%2 for i in range(iM-1,iM+2) for j in range(iN-1,iN+2) if 0 <= i < m and 0<= j < n] )-board[iM][iN]
                if board[iM][iN] == 0 and numNeighbor == 3:
                    board[iM][iN] = 2
                if board[iM][iN] == 1 and ( numNeighbor < 2 or numNeighbor >  3):
                    board[iM][iN] = 3
        for iM in range(m):
            for iN in range(n):
                if board[iM][iN] == 2:
                    board[iM][iN] = 1
                if board[iM][iN] == 3:
                    board[iM][iN] = 0
```

## [Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
>Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

>Find all the elements that appear twice in this array.

>Could you do it without extra space and in O(n) runtime?

Example:
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]

```python
class Solution(object):
    def findDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        res = []
        for x in nums:
            if nums[abs(x)-1] < 0:
                res.append(abs(x))
            else:
                nums[abs(x)-1] *= -1

        return res
```
## [ Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
>Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Solve it without division and in O(n).


For example, given [1,2,3,4], return [24,12,8,6].

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        p = 1
        n = len(nums)

        output = []
        for i in range(0,n):
            output.append(p)
            p = p*nums[i]

        p = 1
        for i in range(n-1,-1,-1):
            output[i] = output[i]*p
            p = p*nums[i]
        return output

```

## [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
>Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution.

Example:
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

```python
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        n = len(numbers)
        indice = [0,0]
        if(numbers == [] or n<2):
            return indice
        left = 0
        right = n-1
        while left < right:
            v = numbers[left]+numbers[right]
            if v == target:
                indice[0] = left+1
                indice[1] = right + 1
                break
            elif (v > target):
                right -= 1
            elif (v < target):
                left += 1
        return indice
```
## [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
>Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.
```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        j = len(height) - 1
        i,mx =0,0
        while i<j:
            mx = max(mx,((j-i)*min(height[i],height[j])))
            if height[i]<height[j]:
                i+=1
            else:
                j-=1

        return mx

```
## []()
