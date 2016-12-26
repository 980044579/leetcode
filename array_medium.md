# Array_medium
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
