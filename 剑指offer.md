# 剑指offer
## 矩阵中的路径
tag ： 回溯

>设计一个函数来判断一个矩阵中是否存在一条包含字符串所有字符的路径，路径不能回走（走过的地方不能再走）

```
s = [
  'a','b','c','d',         'bcce'存在
  's','f','c','s',         ‘afce’不存在
  'a','d','e','e'
]
```

### Solution：

```python
def hasPath(matrix,rows,cols,string):
    if not matrix or rows<1 or cols <1 or not string :
        return False
    visited=[False for i in range(rows*cols)]

    pathLength = 0
    for row in range(rows):
        for col in range(cols):
            if hasPathCore(matrix,rows,cols,row,col,visited,string,pathLength):
                return True
    return False    

def hasPathCore(matrix,rows,cols,row,col,visited,string,pathLength):
    if pathLength == len(string):
        return True
    hasPath = False
    if row >= 0 and row<rows and col>=0 and col<cols and matrix[row*cols + col] == string[pathLength] and not visited[row*cols + col]:
        pathLength += 1
        visited[row*cols + col] == True
        hasPath = hasPathCore(matrix,rows,cols,row+1,col,visited,string,pathLength) or hasPathCore(matrix,rows,cols,row-1,col,visited,string,pathLength) or hasPathCore(matrix,rows,cols,row,col+1,visited,string,pathLength) or hasPathCore(matrix,rows,cols,row,col-1,visited,string,pathLength)
        if not hasPath:
            pathLength -= 1
            visited[row*cols + col] == False
    return hasPath

```

## 机器人的运动范围

tag：回溯

>地上有m行m列的方格，一个机器人从坐标（0，0）的格子开始运动，它每次可以向上，向下，向左，或者向右移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。比如（35，36）的列数之和为3+5+3+6=17

### Solution：
```python
def movingCount(threshould,rows,cols):
    visited = [False for i in range(rows*cols)]
    count = movingCountCore(threshould,rows,cols,0,0,visited)
    return count

def movingCountCore(threshould,rows,cols,row,col,visited):
    count = 0
    if check(threshould,rows,cols,row,col,visited):
        visited[row*cols+col] = True
        count = 1 + movingCountCore(threshould,rows,cols,row+1,col,visited) + movingCountCore(threshould,rows,cols,row-1,col,visited) + movingCountCore(threshould,rows,cols,row,col+1,visited) + movingCountCore(threshould,rows,cols,row,col-1,visited)
    return count

def check(threshould,rows,cols,row,col,visited):
#检查改点是否合法
    if row>=0 and row<rows and col>=0 and col<cols and getDigitSum(row)+getDigitSum(col)<=threshould and not visited[row*cols+col]:
        return True
def getDigitSum(number):
#获取一个数字的位数和
    sum = 0
    while number>0:
        sum += number % 10
        number  =  number//10
    return sum
```
