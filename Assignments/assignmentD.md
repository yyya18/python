# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：对每个字母一一枚举，看是否符合轻或重的条件



代码：

```python
for _ in range(int(input())):
    L=[[],[],[]]
    for i in range(3):
        L[i]=input().split()
    for a in 'ABCDEFGHIJKL':
        if all((a in i[0] and i[2]=='up') or (a in i[1] and i[2]=='down') or (a not in i[0]+i[1] and i[2]=='even') for i in L):
            print('{} is the counterfeit coin and it is heavy.'.format(a))
            break
        if all((a in i[0] and i[2]=='down') or (a in i[1] and i[2]=='up') or (a not in i[0]+i[1] and i[2]=='even') for i in L):
            print('{} is the counterfeit coin and it is light.'.format(a))
            break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241219154611730](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241219154611730.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：[map(int,input().split())]不是一个包含整数的列表，里面还是迭代器，用list()或者[int(i) for i in input().split()...... debug又浪费好多时间

看到有同学用辅助栈 但是为什么递归比辅助栈时间短呀 是跟数据有关系吗

代码：

```python
r,c=map(int,input().split())
h=[]
h.append([10001 for _ in range(c+2)])
for _ in range(r):
    h.append([10001]+list(map(int,input().split()))+[10001])
h.append([10001 for _ in range(c+2)])
dp=[[0]*(c+2) for _ in range(r+2)]
dx=[-1,0,1,0]
dy=[0,1,0,-1]
def dfs(i,j):
    if dp[i][j]>0:
        return dp[i][j]
    for k in range(4):
        if h[i+dx[k]][j+dy[k]]<h[i][j]:
            dp[i][j]=max(dp[i][j],dfs(i+dx[k],j+dy[k])+1)
    return dp[i][j]
ans=0
for i in range(1,r+1):
    for j in range(1,c+1):
        ans=max(ans,dfs(i,j))
print(ans+1)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241219164654712](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241219164654712.png)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：学完题解自己手敲了一遍都要十分钟......人与人的差距、、

还是专注于一个点 但是在一些范围等细节上不能忘了判断另一个点

（模板又生了怎么回事

代码：

```python
from collections import deque
n=int(input())
maze=[list(map(int,input().split())) for _ in range(n)]
start=[]
for i in range(n):
    for j in range(n):
        if maze[i][j]==5:
            start.append((i,j))
delta_x=start[1][0]-start[0][0]
delta_y=start[1][1]-start[0][1]
visited=set()
visited.add((start[0][0],start[0][1]))
def is_valid(x,y):
    if 0<=x<n and 0<=y<n and (x,y) not in visited and maze[x][y]!=1 and 0<=x+delta_x<n and 0<=y+delta_y<n and maze[x+delta_x][y+delta_y]!=1:
        return 1
    else:
        return 0
dx=[0,0,1,-1]
dy=[1,-1,0,0]
q=deque()
q.append((start[0][0],start[0][1]))
while q:
    front_x,front_y=q.popleft()
    if maze[front_x][front_y]==9 or maze[front_x+delta_x][front_y+delta_y]==9:
        print('yes')
        exit()
    for i in range(4):
        nx=front_x+dx[i]
        ny=front_y+dy[i]
        if is_valid(nx,ny):
            visited.add((nx,ny))
            q.append((nx,ny))
print('no')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241220202311400](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241220202311400.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：好难



代码：

```python
def f(string):
    if string=='':
        return 0
    else:
        return int(string)
m=int(input())
n=int(input())
l=input().split()
for i in range(n):
    for j in range(n-1-i):
        if l[j]+l[j+1]>l[j+1]+l[j]:
            l[j],l[j+1]=l[j+1],l[j]
weight=[]
for i in l:
    weight.append(len(i))
dp=[['']*(m+1) for _ in range(n+1)]
for k in range(m+1):
    dp[0][k]=''
for q in range(n+1):
    dp[q][0]=''
for i in range(1,n+1):
    for j in range(1,m+1):
        if weight[i-1]>j:
            dp[i][j]=dp[i-1][j]
        else:
            dp[i][j]=str(max(f(dp[i-1][j]),int(l[i-1]+dp[i-1][j-weight[i-1]])))
print(dp[n][m])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241220214258352](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241220214258352.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：水平大概是努力看懂题解 要理解题目给的关键提示 对每一个输入的矩阵 按第一行64种按灯情况一一验证直到找到解 用A[1][i]= abs(A[1][i] - 1)来模拟按灯 以及因为要尝试很多次所以要深拷贝X和Y列表



代码：

```python
X = [[0,0,0,0,0,0,0,0]]
Y = [[0,0,0,0,0,0,0,0]]
for _ in range(5):
    X.append([0] + [int(x) for x in input().split()] + [0])
    Y.append([0 for x in range(8)])    
X.append([0,0,0,0,0,0,0,0])
Y.append([0,0,0,0,0,0,0,0])

import copy
for a in range(2):
    Y[1][1] = a
    for b in range(2):
        Y[1][2] = b
        for c in range(2):
            Y[1][3] = c
            for d in range(2):
                Y[1][4] = d
                for e in range(2):
                    Y[1][5] = e
                    for f in range(2):
                        Y[1][6] = f
                        
                        A = copy.deepcopy(X)
                        B = copy.deepcopy(Y)
                        for i in range(1, 7):
                            if B[1][i] == 1:
                                A[1][i] = abs(A[1][i] - 1)
                                A[1][i-1] = abs(A[1][i-1] - 1)
                                A[1][i+1] = abs(A[1][i+1] - 1)
                                A[2][i] = abs(A[2][i] - 1)
                        for i in range(2, 6):
                            for j in range(1, 7):
                                if A[i-1][j] == 1:
                                    B[i][j] = 1
                                    A[i][j] = abs(A[i][j] - 1)
                                    A[i-1][j] = abs(A[i-1][j] - 1)
                                    A[i+1][j] = abs(A[i+1][j] - 1)
                                    A[i][j-1] = abs(A[i][j-1] - 1)
                                    A[i][j+1] = abs(A[i][j+1] - 1)
                        if A[5][1]==0 and A[5][2]==0 and A[5][3]==0 and A[5][4]==0 and A[5][5]==0 and A[5][6]==0:
                            for i in range(1, 6):
                                print(" ".join(repr(y) for y in [B[i][1],B[i][2],B[i][3],B[i][4],B[i][5],B[i][6] ]))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241221153034964](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241221153034964.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：灵活的二分查找



代码：

```python
L,N,M=map(int,input().split())
stones=[0]
for _ in range(N):
    stones.append(int(input()))
stones.append(L)
left,right=0,L
while left<right:
    mid=(left+right)//2
    cnt,pos=0,0
    for i in range(1,len(stones)):
        if stones[i]-stones[pos]<mid:
            cnt+=1
        else:
            pos=i
    if cnt>M:
        right=mid
    else:
        left=mid+1
print(left-1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241221162303287](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241221162303287.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

感觉这次作业还挺难的 自己做感觉做不了几题TT 尤其是最大整数和熄灯问题 用了好久看懂题解 （看懂之后想考场上还是做不出的为什么不去刷点简单题、、



