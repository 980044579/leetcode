# String_medium

 

## [Multiply Strings](https://leetcode.com/problems/multiply-strings/)
>Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2.

Note:

The length of both num1 and num2 is < 110.
Both num1 and num2 contains only digits 0-9.
Both num1 and num2 does not contain any leading zero.
You must not use any built-in BigInteger library or convert the inputs to integer directly.



```python
class Solution(object):
    def multiply(self,num1, num2):
        product = [0] * (len(num1) + len(num2))
        pos = len(product)-1

        for n1 in reversed(num1):
            tempPos = pos
            for n2 in reversed(num2):
                product[tempPos] += int(n1) * int(n2)
                product[tempPos-1] += product[tempPos]/10
                product[tempPos] %= 10
                tempPos -= 1
            pos -= 1

        pt = 0
        while pt < len(product)-1 and product[pt] == 0:
            pt += 1

        return ''.join(map(str, product[pt:]))
```

## [Group Anagrams](https://leetcode.com/problems/anagrams/)
>Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"],
Return:

[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]


```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        d = {}
        for i in strs:
            key = tuple(sorted(i))
            d[key] = d.get(key,[])+[i]
        return d.values()
```
## [Simplify Path](https://leetcode.com/problems/simplify-path/)
>Given an absolute path for a file (Unix-style), simplify it.

For example,
path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"
click to show corner cases.

Corner Cases:
Did you consider the case where path = "/../"?
In this case, you should return "/".
Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".
In this case, you should ignore redundant slashes and return "/home/foo".

```python
class Solution(object):
    def simplifyPath(self, path):
        """
        :type path: str
        :rtype: str
        """
        places = [p for p in path.split('/') if p!="." and p!='']
        stack = []
        for p in places:
            if p == "..":
                if len(stack) > 0:
                    stack.pop()
            else:
                stack.append(p)
        return "/" + "/".join(stack)
```
## [Decode Ways](https://leetcode.com/problems/decode-ways/)
>A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.

```python
class Solution(object):
    def numDecodings(self, s):
        '''
        Here we try to compute dp[i] for each i. dp[i] is the number of ways in which the string s[i:len(s)] can be decoded.

        '''
        if not s:   return 0
        i, n = len(s)-1, len(s)
        prev, prevPrev = 1, 1
        while i>=0:
            char = s[i]
            # If current character is 3-9, then dp[i]=dp[i+1] since there is no additional ways that can be induced by these digits
            if char in '3456789':
                curr = prev
            # If current char is 0, then there can be ZERO ways of decoding a string that starts with a 0
            elif char == '0':
                curr = 0
            elif char in '12':
                # If current char is 1 or 2 and next character is 0, then we have no additional ways of decoding.
                # Hence dp[i] = dp[i+2] (Note: dp[i+1] would be 0, since s[i+1] is '0'
                if i<n-1 and s[i+1]=='0':
                    curr = prevPrev
                # If the next char is 1-6 (or 7-9 with curr char as '1'), then dp[i] = dp[i+1] + dp[i+2]
                # dp[i] = dp[i+1] (represents the numWays when we regard 1/2 as A/B) + dp[i+2] (represents the numWays when we regard 1/2 along with next digit
                elif i<n-1 and ((s[i+1] in '123456') or (s[i+1] in '789' and char=='1')):
                    curr = prev + prevPrev
                # If we have 1/2 as the rightmost digit of the string, then we don't have any additional ways to produce
                else:
                    curr = prev
            prev, prevPrev = curr, prev
            i-=1
        return prev
```
## [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)
>Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter)

```python
class Solution(object):
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        res = []
        self.dfs(s,0,'',res)
        return res

    def dfs(self,s,index,path,res):
        if index == 4:
            if not s:
                res.append(path[:-1])
            return
        for i in range(1,4):
            if i<= len(s):
                if i == 1:
                    self.dfs(s[i:],index+1,path+s[:i]+'.',res)
                elif i == 2 and s[0] != '0':
                    self.dfs(s[i:],index+1,path+s[:i]+'.',res)
                elif i == 3 and s[0] != '0' and int(s[:i]) < 256:
                    self.dfs(s[i:],index+1,path+s[:i]+'.',res)

```

## [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)
>Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, * / operators and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid.

Some examples:
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5

```python
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        if not s :
            return 0
        stack,num,sign = [],0,'+'
        for i in range(len(s)):
            if s[i].isdigit():
                num = num*10+ord(s[i])-ord("0")
            if (not s[i].isdigit() and not s[i].isspace()) or i == len(s)-1:
                if sign == '+':
                    stack.append(num)
                elif sign == '-':
                    stack.append(-num)
                elif sign == '*':
                    stack.append(stack.pop()*num)
                else:
                    tmp = stack.pop()
                    if tmp//num < 0 and tmp%num != 0:
                        stack.append(tmp//num+1)
                    else:
                        stack.append(tmp//num)
                sign = s[i]
                num = 0
        return sum(stack)

```
## [Mini Parser](https://leetcode.com/problems/mini-parser/)
>Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Note: You may assume that the string is well-formed:

String is non-empty.
String does not contain white spaces.
String contains only digits 0-9, [, - ,, ].
Example 1:

Given s = "324",

You should return a NestedInteger object which contains a single integer 324.
Example 2:

Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.

```python

# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger(object):
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

class Solution(object):
    def deserialize(self, s):
        """
        :type s: str
        :rtype: NestedInteger
        """
        def nestedInteger(x):
            if isinstance(x,int):
                return NestedInteger(x)
            lst = NestedInteger()
            for y in x:
                lst.add(NestedInteger(y))
            return lst
        return nestedInteger(eval(s))

```

## [Validate IP Address](https://leetcode.com/problems/validate-ip-address/)
>Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.

IPv4 addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots ("."), e.g.,172.16.254.1;

Besides, leading zeros in the IPv4 is invalid. For example, the address 172.16.254.01 is invalid.

IPv6 addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons (":"). For example, the address 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so 2001:db8:85a3:0:0:8A2E:0370:7334 is also a valid IPv6 address(Omit leading zeros and using upper cases).

However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons (::) to pursue simplicity. For example, 2001:0db8:85a3::8A2E:0370:7334 is an invalid IPv6 address.

Besides, extra leading zeros in the IPv6 is also invalid. For example, the address 02001:0db8:85a3:0000:0000:8a2e:0370:7334 is invalid.

Note: You may assume there is no extra space or special characters in the input string.

Example 1:
Input: "172.16.254.1"

Output: "IPv4"

Explanation: This is a valid IPv4 address, return "IPv4".
Example 2:
Input: "2001:0db8:85a3:0:0:8A2E:0370:7334"

Output: "IPv6"

Explanation: This is a valid IPv6 address, return "IPv6".
Example 3:
Input: "256.256.256.256"

Output: "Neither"

Explanation: This is neither a IPv4 address nor a IPv6 address.

```python
class Solution(object):
    def validIPAddress(self, IP):
        """
        :type IP: str
        :rtype: str
        """
        def is_hex(s):
            hex_digits = set("0123456789abcdefABCDEF")
            for char in s:
                if not (char in hex_digits):
                    return False
            return True
        ary = IP.split('.')
        if len(ary) == 4:
            for i in range(4):
                if not ary[i].isdigit() or not 0 <= int(ary[i]) < 256 or (ary[i][0] == '0' and len(ary[i]) > 1):
                    return "Neither"
            return "IPv4"
        ary = IP.split(':')
        if len(ary) == 8:
            for i in range(8):
                tmp = ary[i]
                if len(tmp) == 0 or not len(tmp) <=4 or not is_hex(tmp):
                    return "Neither"
            return "IPv6"
        return "Neither"
```

## [Compare Version Numbers](https://leetcode.com/problems/compare-version-numbers/)
>Compare two version numbers version1 and version2.
If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:

0.1 < 1.1 < 1.2 < 13.37

```python
class Solution(object):
    def compareVersion(self, version1, version2):
        """
        :type version1: str
        :type version2: str
        :rtype: int
        """
        v1 = version1.split(".")
        v2 = version2.split(".")

        m = len(v1)
        n = len(v2)

        l = max(m,n)
        if m > n:
            for j in range(m-n):
                v2.append("0")
        if m < n:
            for j in range(n-m):
                v1.append("0")


        for i in range(l):
            if int(v1[i]) > int(v2[i]):
                return 1
            elif int(v1[i]) < int(v2[i]):
                return -1
        return 0
```
