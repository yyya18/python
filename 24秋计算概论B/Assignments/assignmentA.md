# Assignment #A: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：居然还有我能5min写出来的题目（樂



代码：

```python
n=int(input())
dp=[0]*(n+1)
dp[0]=1
dp[1]=1
for i in range(1,n):
    dp[i+1]=dp[i]+dp[i-1]
print(dp[n])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241128171048614](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241128171048614.png)



### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/

思路：找规律



代码：

```python
n=int(input())
print(2**(n-1))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241128172749197](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241128172749197.png)

![image-20241129171433878](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241129171433878.png)

### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D

思路：看出来是数楼梯的变形之后因为dp格式不规范导致一直re

还有就是学到了要用前缀来优化 



代码：

```python
mod=1000000007
maxn=100001
dp=[0]*maxn
pre_sum=[0]*maxn

def count(k):
    dp[0]=1
    for i in range(1,k):
        dp[i]=1
        pre_sum[i]=(pre_sum[i-1]+dp[i])%mod
    for i in range(k,maxn):
        dp[i]=(dp[i-1]+dp[i-k])%mod
        pre_sum[i]=(pre_sum[i-1]+dp[i])%mod

def sum_up(m):
    return pre_sum[m]

t,k=map(int,input().split())
count(k)
for _ in range(t):
    a,b=map(int,input().split())
    print((sum_up(b)-sum_up(a-1))%mod)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241130090832276](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241130090832276.png)



### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：



代码：

```python
class Solution:
    def longestPalindrome(self,s:str)->str:
        n=len(s)
        dp=[[False]*n for _ in range(n)]
        max_len=0
        left,right=0,0
        for i in range(n-1,-1,-1):
            for j in range(i,n):
                if s[i]==s[j]:
                    if j-i<=1 or dp[i+1][j-1]==True:
                        dp[i][j]=True
                if dp[i][j]==True and j-i+1>max_len:
                    max_len=j-i+1
                    left=i
                    right=j
        return s[left:right+1]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241130095329244](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241130095329244.png)





### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：看题解也有点费劲 模板题的代码是真长啊... 思路不清晰的话真能把自己绕晕



代码：

```python
import sys

direction = [(1, 0), (0, 1), (-1, 0), (0, -1)]


def dfs(matrix, x, y, target_x, target_y):
    m, n = len(matrix), len(matrix[0])
    h = matrix[x][y]
    stack = [(x, y)]
    visited = [[False for _ in range(n)] for _ in range(m)]

    while stack:
        x, y = stack.pop()
        if x == target_x and y == target_y:
            return True
        visited[x][y] = True
        for dx, dy in direction:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n: 
                if not visited[nx][ny] and matrix[nx][ny] < h:  
                    stack.append((nx, ny))

    return False

data = sys.stdin.read().split()
k = int(data[0])
id = 1
ans = []

for _ in range(k):
    m, n = int(data[id]), int(data[id + 1])
    id += 2
    matrix = [list(map(int, data[id + i * n:id + (i + 1) * n])) for i in range(m)]
    id += m * n
    a, b = int(data[id]) - 1, int(data[id + 1]) - 1
    id += 2
    p = int(data[id])
    id += 1
    pos = [(int(data[id + i * 2]) - 1, int(data[id + i * 2 + 1]) - 1) for i in range(p)]
    id += p * 2

    result = any(dfs(matrix, x, y, a, b) for x, y in pos)
    ans.append("Yes" if result else "No")

sys.stdout.write("\n".join(ans) + "\n")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241201225823020](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241201225823020.png)



### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/

思路：（没有题解我可怎么活啊x



代码：

```python
import heapq
num1=1
while True:
    w,h=map(int,input().split())
    if w==0 and h==0:
        break
    print(f"Board #{num1}:")
    martix=[[" "]*(w+2)]+[[" "]+list(input())+[" "] for _ in range(h)]+[[" "]*(w+2)]
    dir=[(0,1),(0,-1),(1,0),(-1,0)]
    num2=1
    while True:
        x1,y1,x2,y2=map(int,input().split())
        if x1==0 and x2==0 and y1==0 and y2==0:
            break
        queue,flag=[],False
        vis=set()
        heapq.heappush(queue,(0,x1,y1,-1))
        martix[y2][x2]=" "
        vis.add((-1,x1,y1))
        while queue:
            step,x,y,dirs=heapq.heappop(queue)
            if x==x2 and y==y2:
                flag=True
                break
            for i,(dx,dy) in enumerate(dir):
                px,py=x+dx,y+dy
                if 0<=px<=w+1 and 0<=py<=h+1 and (i,px,py) not in vis and martix[py][px]!="X":
                    vis.add((i,px,py))
                    heapq.heappush(queue,(step+(dirs!=i),px,py,i))
        if flag:
            print(f"Pair {num2}: {step} segments.")
        else:
            print(f"Pair {num2}: impossible.")
        martix[y2][x2]="X"
        num2+=1
    print()
    num1+=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241201230646881](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241201230646881.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

好难..好难..难题连看懂题解都费劲 感觉还是得回头先把各个模块的例题弄熟练了再练点配套的 请善待我TT



