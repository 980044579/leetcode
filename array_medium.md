# Array_medium
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
## [ombination Sum III](https://leetcode.com/problems/combination-sum-iii/)
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
