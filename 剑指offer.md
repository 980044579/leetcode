# 剑指offer
## P_283序列化二叉树
>请实现两个函数，分别来序列化和反序列化二叉树

tag：树

### Soluthon：

```python
def Serialize(root,output):
    if root == None:
        out+'$,'
        return
    out+str(root.val)+','
    Serialize(root.left,output)
    Serialize(roo.right,output)


def Deserialize(root,inpt):
    inp = inpt.split(',')
    DeserializeCore(root,inp)
def DeserializeCore(root,inp):
    inp = put.split(',')
    value = inp.pop(0)
    if value.isdigit():
        root = TreeNode()
        root.val = int(value)
        root.left = None
        root.right = None

        Deserialize(root.left,inp)
        Deserialize(root.right,inp)

```

## P_285二叉搜索树的第k个结点
>给定一颗二叉搜索树，请找出其中的第k大的节点。

tag：树
### Soluthon

```python
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def KthNode(root,k):
    if root == None or k == 0:
        return None
    return KthNodeCore(root,k)

def KthNodeCore(root,k):
    target = None

    if  root.left:
        target = KthNodeCore(root.left,k)
    if targrt == None:
        if k == 1:
            target = root
        k-=1
    if target == None and root.right != None:
        targrt = KthNodeCore(root.right,k)
    return target

```
## P_186数据流中的中位数
>如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值

###Soluthon：

```python
暂留

```
## P_290输出输出滑窗中的最大值

tag ：队列，栈

>输入一个列表，和滑窗大小，依次输出每个滑窗中的最大值。例如：[2,3,1,4,5,3,7] 和 划窗大小3则输出[2,4,5,5,7]

### Solution：

```python
def FindTheMax(matrix,k):
    if not matrix or k <= 0:
        return []
    if k>len(matrix):
        print('非法输入')
        return
    ans,maxIdex = [],[]#ans存最后结果，maxInde存的是可能的最大值的下标
    for i in range(k):
        while len(maxIdex) != 0 and matrix[i] >= matrix[maxIdex[-1]]:
            #print(maxIdex)
            maxIdex.pop()
        maxIdex.append(i)
    for j in range(k,len(matrix)):
        ans.append(matrix[maxIdex[0]])
        while len(maxIdex) != 0 and matrix[j] >= matrix[maxIdex[-1]]:
            maxIdex.pop()
        if len(maxIdex) != 0 and maxIdex[0] <= j-k:
            maxIdex.pop(0)
        maxIdex.append(j)
    ans.append(matrix[maxIdex[0]])
    return ans
```

## P_294矩阵中的路径
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

## P_296机器人的运动范围

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
