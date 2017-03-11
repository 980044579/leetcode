# Array_easy

## [Majority Element](https://leetcode.com/problems/majority-element/)
> Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

>You may assume that the array is non-empty and the majority element always exist in the array.

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        count,candidate = 0,0
        for n in nums :
            if n == candidate:
                count += 1
            elif count == 0 :
                candidate , count = n , 1
            else :
                count -= 1
        return candidate
```


## [Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)
>Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

Note:
Could you optimize your algorithm to use only O(k) extra space?

```python
class Solution(object):
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        row = [1]
        for _ in range(rowIndex):
            row = [x + y for x, y in zip([0]+row, row+[0])]
        return row
```

## [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
>Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

>Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: void Do not return anything, modify nums1 in-place instead.
        """

        i,j = m-1,n-1
        while i != -1 and j != -1:
            if nums1[i]>nums2[j]:
                nums1[i+j+1] = nums1[i]
                i -= 1
            else:
                nums1[i+j+1] = nums2[j]
                j -= 1
        if i == -1:
            nums1[:j+1] = nums2[:j+1]
```


## [Remove Element](https://leetcode.com/problems/remove-element/)

>Given an array and a value, remove all instances of that value in place and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:
Given input array nums = [3,2,2,3], val = 3

Your function should return length = 2, with the first two elements of nums being 2.


```python
class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        i = 0
        for num in nums:
            if num != val:
                nums[i] = num
                i += 1
        return i

```

## [Two Sum](https://leetcode.com/problems/two-sum/)
>Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        dic = {}
        for i in range(len(nums)):
            if (target-nums[i]) in dic:
                return [dic[target-nums[i]],i]
            else:
                dic[nums[i]] = i

```

## [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
>Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example 1:
```
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
```
Example 2:
```
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.
```

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if len(prices) == 0:
            return 0

        min = prices[0]
        maxfit = 0
        for i in range(1,len(prices)):
            if prices[i] < min:
                min = prices[i]
            else:
                maxfit = max(maxfit,prices[i]-min)
        return maxfit

```

## [Rotate Array](https://leetcode.com/problems/rotate-array/)
>Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4]

```python
class Solution(object):
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        if(nums == [] or n < 2):
            return
        k = k%n
        nums[:] = nums[n-k:] + nums[:n-k]

```


## [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)
>Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.



```python
def containsNearbyDuplicate(self, nums, k):
    dic = {}
    for i, v in enumerate(nums):
        if v in dic and i - dic[v] <= k:
            return True
        dic[v] = i
    return False
```


## [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
>Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if nums ==[]:
            return
        insertPos = 0
        for num in nums:
            if num !=0:
                nums[insertPos] = num
                insertPos +=1

        while insertPos < len(nums):
            nums[insertPos] = 0
            insertPos +=1            
```

## [Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)
>Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

Example 1:
```
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```
Example 2:
```
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```
Example 3:
```
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
```

```python
class Solution(object):
    def thirdMax(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        nums = set(nums)
        if len(nums)<3:
            return max(nums)
        nums.remove(max(nums))
        nums.remove(max(nums))
        return max(nums)



```


## [Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
>Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

```python
class Solution(object):
    def findDisappearedNumbers(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        out = []
        for i in range(len(nums)):
            a = abs(nums[i]) - 1
            if nums[a]>0:
                nums[a] = -nums[a]
        for i in range(len(nums)):
            if nums[i]>0:
                out.append(i+1)
        return out
```






## [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
>Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.

Example:
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.
```python
class Solution(object):
    def removeDuplicates(self, nums):

        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n == 0:
            return 0
        i,j = 1,1
        while j < n:
            if nums[i-1] != nums[j]:
                nums[i] = nums[j]
                i+=1
            j+=1
        return i
```
