# stack_queue
## [132 Pattern](https://leetcode.com/problems/132-pattern/#/description)
>Given a sequence of n integers a1, a2, ..., an, a 132 pattern is a subsequence ai, aj, ak such that i < j < k and ai < ak < aj. Design an algorithm that takes a list of n numbers as input and checks whether there is a 132 pattern in the list.

Note: n will be less than 15,000.

Example 1:
Input: [1, 2, 3, 4]

Output: False

Explanation: There is no 132 pattern in the sequence.
Example 2:
Input: [3, 1, 4, 2]

Output: True

Explanation: There is a 132 pattern in the sequence: [1, 4, 2].
Example 3:
Input: [-1, 3, 2, 0]

Output: True

Explanation: There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

```python
#Scan nums only once. The stack maintains a list of disjoint intervals (except the end point) where the lower and upper bound of each interval denote the minimum and maximum in the '132' pattern respectively. Thus any subsequent number which is strictly contained in any intervals in the stack will form a '132' pattern.
# The order of intervals in the stack are maintained in a way such that if the right end of interval A is less than or equal to the left end of interval B, then A is above B in the stack. The time complexity is O(N), since each number will at most be pushed and popped once.
class Solution(object):
    def find132pattern(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        minx = float('inf')
        stack = []
        for num in nums:
            while stack and stack[-1][1] <= num:
                stack.pop()
            if stack and stack[-1][0] < num:
                return True
            minx = min(minx,num)
            stack.append([minx,num])
        return False

```
## [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/#/description)
>Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.
```
Example 1:
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number;
The second 1's next greater number needs to search circularly, which is also 2.
```

```python
class Solution(object):
    def nextGreaterElements(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        ans = [0]*len(nums)
        stack = []
        for j in range(2):
            for i in range(len(nums)-1,-1,-1):
                item = (i,nums[i])
                while stack and stack[-1][1]<=nums[i]:
                    stack.pop()
                if not stack:
                    ans[i] = -1
                    stack.append(item)
                else:
                    ans[i] = stack[-1][1]
                    stack.append(item)
        return ans
```
## [Min Stack](https://leetcode.com/problems/min-stack/#/description)
>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
Example:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        min = self.getMin()
        if min > x or min == None:
            min = x
        self.stack.append((x,min))




    def pop(self):
        """
        :rtype: void
        """
        self.stack.pop()


    def top(self):
        """
        :rtype: int
        """
        if len(self.stack) == 0:
            return None
        else:
            return self.stack[-1][0]



    def getMin(self):
        """
        :rtype: int
        """
        if not self.stack:
            return None
        else:
            return self.stack[-1][1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

## [ Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/#/description)

>You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

Example 1:
```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```    
Example 2:
```
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
```    
Note:
All elements in nums1 and nums2 are unique.
The length of both nums1 and nums2 would not exceed 1000.

```python
class Solution(object):
    def nextGreaterElement(self, findNums, nums):
        """
        :type findNums: List[int]
        :type nums: List[int]
        :rtype: List[int]
        """
        stack = []
        map = {}
        for num in nums:
            while stack and num > stack[-1]:
                map[stack.pop()] = num
            stack.append(num)
        while stack:
            map[stack.pop()] = -1
        return [map[num] for num in findNums]
```
