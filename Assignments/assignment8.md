# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周长

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：30min

计算1的数量和相邻边的数量，前者*4-后者即为答案，用到保护圈。

代码：

```python
n,m=map(int,input().split())
s=[[0]*(m+2) for i in range(n+2)]
for i in range(1,n+1):
    t=list(map(int,input().split()))
    t=[0]+t+[0]
    for j in range(1,m+1):
        s[i][j]+=t[j]
y=0
near=0
for i in range(n+2):
    for j in range(m+2):
        if s[i][j]==1:
            y+=1
dire=[[-1,0],[1,0],[0,-1],[0,-1]]
for i in range(4):
    for p in range(n+2):
        for q in range(m+2):
            if s[p][q]==1 and s[p+dire[i][0]][q+dire[i][1]]==1:
                near+=1
print(y*4-near)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241117210356011](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241117210356011.png)



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：directions的顺序必须是右下左上顺时针，对于改变方向的设置要小心。



代码：

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]:
            return list()
        
        rows, columns = len(matrix), len(matrix[0])
        visited = [[False] * columns for _ in range(rows)]
        total = rows * columns
        order = [0] * total

        directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]
        row, column = 0, 0
        directionIndex = 0
        for i in range(total):
            order[i] = matrix[row][column]
            visited[row][column] = True
            nextRow, nextColumn = row + directions[directionIndex][0], column + directions[directionIndex][1]
            if not (0 <= nextRow < rows and 0 <= nextColumn < columns and not visited[nextRow][nextColumn]):
                directionIndex = (directionIndex + 1) % 4
            row += directions[directionIndex][0]
            column += directions[directionIndex][1]
        return order
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241118111216559](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241118111216559.png)



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/

思路：学习题解 按照垃圾来枚举确实更快

字典避免keyerror可以用setdefault函数，获取目前指定点位的总数值，如果还没有就插入0之后加上相应数量。



代码：

```python
d=int(input())
n=int(input())
c={}
for _ in range(n):
    x,y,i=map(int,input().split())
    for p in range(max(0,x-d),min(1024,x+d)+1):
        for q in range(max(0,y-d),min(1024,y+d)+1):
            c[(p,q)]=c.setdefault((p,q),0)+i
a=list(c.values())
s=max(a)
print(a.count(s),s)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241117210844273](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241117210844273.png)



### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：原来是可以一路往后考虑的 感觉是题目做得不够多 对自己有的思路也没信心写下去

用符号函数来判断正负还是挺好的 差初始化为0也要考虑周全

代码：

```python
def sgn(x):
    if x==0:
        return 0
    elif x>0:
        return 1
    else:
        return -1
n=int(input())
a=list(map(int,input().split()))
d=[sgn(a[i+1]-a[i]) for i in range(n-1)]
result=1
pre=0
for i in range(n-1):
    if d[i]*pre<0 or (pre==0 and d[i]!=0):
        result+=1
        pre=d[i]
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118214802152](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241118214802152.png)



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：知道要用dp做 能写状态转移方程

学到了对于不确定的长度可以用一个较大的数去包括所有情况

代码：

```python
m=int(1e5)
a=[0]*(m+1)
n=int(input())
for i in map(int,input().split()):
    a[i]+=1
dp=[[0,0]for _ in range(m+1)]
for i in range(1,m+1):
    dp[i][0]=max(dp[i-1][0],dp[i-1][1])
    dp[i][1]=dp[i-1][0]+a[i]*i
print(max(dp[m][0],dp[m][1]))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118225601725](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241118225601725.png)



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：还没深入研究过 经典题目题解真是丰富（） 打算明天学习一下不同的实现方法



代码：

```python
while True:
    n = int(input())
    if n == 0:
        break
    a = sorted([int(x) for x in input().split()], reverse=True)
    b = sorted([int(x) for x in input().split()], reverse=True)
    c = [[0]*(n+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for j in range(1, n+1):
            if a[i-1] > b[j-1]:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1]+2)
            elif a[i-1] == b[j-1]:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1]+1)
            else:
                c[i][j] = max(c[i-1][j], c[i][j-1], c[i-1][j-1])
    print((c[n][n]-n)*200)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118230859703](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241118230859703.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

还是没有额外做什么（）定了新的计划 无底洞还是能补一题是一题 现在还是困难的 但是作业能做两题ww



