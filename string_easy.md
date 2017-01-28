# String_easy

##[Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
>Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        roman = {'M':1000,'D':500,'C':100,'L':50,'X':10,'V':5,'I':1}
        z = 0
        for i in range(0,len(s) - 1):
            if roman[s[i]] < roman[s[i+1]]:
                z -= roman[s[i]]
            else:
                z += roman[s[i]]
        return z + roman[s[-1]]
      
```

##[Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

>Write a function to find the longest common prefix string amongst an array of strings.

```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        s,ret = zip(*strs),""
        for c in s:
            if len(set(c)) > 1: break
            ret += str(c[0])
        return ret

```
## [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
>Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        dict = { "]":"[","}":"{",")":"("}
        for char in s:
            if char in dict.values():
                stack.append(char)
            elif char in dict.keys():
                if stack == [] or dict[char] != stack.pop():
                    return False
            else:
                return False
        return stack == []

```


## [Count and Say](https://leetcode.com/problems/count-and-say/)
>The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

```python
class Solution(object):
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        s = '1'
        for i in range(n-1):
            next = ''
            for num in [list(g) for k,g in itertools.groupby(s)]: #k表示序号，g表示每一类的一个迭代
                next += str(len(num))+num[0]
            s = next
        return s


```

## [Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

>Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example,
Given s = "Hello World",
return 5.

```python
class Solution(object):
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        if not s:
            return 0
        flg = 0
        length = 0
        for i in range(len(s)-1,-1,-1):
            if s[i] != ' ':
                length += 1
                flg = 1
            elif flg ==1:
                return length
        if flg == 0:
            return 0
        else:
            return length

```
## [Add Binary](https://leetcode.com/problems/add-binary/)
>Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100".

```python
class Solution(object):
    def addBinary(self, a, b):
        """
        :type a: str
        :type b: str
        :rtype: str
        """
        if len(a) == 0 : return b
        if len(b) == 0 : return a
        if a[-1] == '1' and b[-1] == '1':
            return self.addBinary(self.addBinary(a[0:-1],b[0:-1]),'1')+'0'
        if a[-1] == '0' and b[-1] == '0':
            return self.addBinary(a[0:-1],b[0:-1])+'0'
        else:
            return self.addBinary(a[0:-1],b[0:-1])+'1'

```

## [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
>Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if not s:
            return True
        i,j = 0,len(s)-1
        while i < j:
            while i<j and not s[i].isalnum():
                i += 1
            while i<j and not s[j].isalnum():
                j -= 1
            if s[i].lower() != s[j].lower():
                return False
            i+=1;j-=1
        return True
```
## [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)
>Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
Given s = "hello", return "holle".

Example 2:
Given s = "leetcode", return "leotcede".

Note:
The vowels does not include the letter "y".

```python
class Solution(object):
    def reverseVowels(self, s):
        """
        :type s: str
        :rtype: str
        """
        vowels = 'AEIOUaeiou'
        s = list(s)
        i,j = 0,len(s)-1
        while i < j:
            while s[i] not in vowels and i<j:
                i += 1
            while s[j] not in vowels and i<j:
                j -= 1
            s[i],s[j] = s[j],s[i]
            i+=1
            j-=1
        return ''.join(s)
```

## [Ransom Note](https://leetcode.com/problems/ransom-note/)
>Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

Note:
You may assume that both strings contain only lowercase letters.
```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

```python
class Solution(object):
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        for i in set(ransomNote):
            if ransomNote.count(i) > magazine.count(i):
                return False
        return True

```


## [ Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)

>Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

Example 1:
Input: "abab"

Output: True

Explanation: It's the substring "ab" twice.
Example 2:
Input: "aba"

Output: False
Example 3:
Input: "abcabcabcabc"

Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)

```python
class Solution(object):
    def repeatedSubstringPattern(self, str):
        """
        :type str: str
        :rtype: bool
        """
        n = len(str)
        for i in range(n//2,0,-1):
            if n%i==0:
                m = n/i
                subS = str[0:i]
                sb = subS*m
                if sb == str:
                    return True
        return False
```
