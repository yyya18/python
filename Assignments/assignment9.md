# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by <mark>胡婧瑶  工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：感觉是很明显的dfs 但是还是不太会debug 耗了很久很久



代码：

```python
def dfs(matrix, row, col, visited):
    if row < 0 or row >= len(matrix) or col < 0 or col >= len(matrix[0]) \
            or matrix[row][col] != 'W' or visited[row][col]:
        return 0

    visited[row][col] = True
    size = 1

    for dr in [-1, 0, 1]:
        for dc in [-1, 0, 1]:
            size += dfs(matrix, row + dr, col + dc, visited)

    return size


def max_connected_area(matrix):
    max_area = 0
    visited = [[False for _ in range(len(matrix[0]))] for _ in range(len(matrix))]

    for row in range(len(matrix)):
        for col in range(len(matrix[0])):
            if matrix[row][col] == 'W' and not visited[row][col]:
                area = dfs(matrix, row, col, visited)
                max_area = max(max_area, area)

    return max_area


def main():
    T = int(input())
    for _ in range(T):
        N, M = map(int, input().split())
        matrix = [input().strip() for _ in range(N)]
        print(max_connected_area(matrix))


if __name__ == "__main__":
    main()
```

![image-20241126143243934](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126143243934.png

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126143108661](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126143108661.png)

![image-20241126143305047](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126143305047.png)

### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：



代码：

```python
import heapq
def bfs(x,y):
    d=[[-1,0],[1,0],[0,1],[0,-1]]
    queue=[]
    heapq.heappush(queue,[0,x,y])
    check=set()
    check.add((x,y))
    while queue:
        step,x,y=map(int,heapq.heappop(queue))
        if martix[x][y]==1:
            return step
        for i in range(4):
            dx,dy=x+d[i][0],y+d[i][1]
            if martix[dx][dy]!=2 and (dx,dy) not in check:
                heapq.heappush(queue,[step+1,dx,dy])
                check.add((dx,dy))
    return "NO"
            
m,n=map(int,input().split())
martix=[[2]*(n+2)]+[[2]+list(map(int,input().split()))+[2] for i in range(m)]+[[2]*(n+2)]
print(bfs(1,1))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241126143505067](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126143505067.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：



代码：

```python
maxn = 10;
sx = [-2,-1,1,2, 2, 1,-1,-2]
sy = [ 1, 2,2,1,-1,-2,-2,-1]

ans = 0;
 
def Dfs(dep: int, x: int, y: int):
    if n*m == dep:
        global ans
        ans += 1
        return
    
   
    for r in range(8):
        s = x + sx[r]
        t = y + sy[r]
        if chess[s][t]==False and 0<=s<n and 0<=t<m :
            chess[s][t]=True
            Dfs(dep+1, s, t)
            chess[s][t] = False
 

for _ in range(int(input())):
    n,m,x,y = map(int, input().split())
    chess = [[False]*maxn for _ in range(maxn)] 
    ans = 0
    chess[x][y] = True
    Dfs(1, x, y)
    print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126143744264](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126143744264.png)



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：



代码：

```python
n, m = map(int, input().split())
maze = [list(map(int, input().split())) for _ in range(n)]

directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
visited = [[False] * m for _ in range(n)] 
max_path = []
max_sum = -float('inf')  

def dfs(x, y, current_path, current_sum):
    global max_path, max_sum

    if (x, y) == (n - 1, m - 1):
        if current_sum > max_sum:
            max_sum = current_sum
            max_path = current_path[:]
        return

    for dx, dy in directions:
        nx, ny = x + dx, y + dy

        if 0 <= nx < n and 0 <= ny < m and not visited[nx][ny]:
            visited[nx][ny] = True
            current_path.append((nx, ny))

            dfs(nx, ny, current_path, current_sum + maze[nx][ny])

            current_path.pop()
            visited[nx][ny] = False

visited[0][0] = True
dfs(0, 0, [(0, 0)], maze[0][0])

for x, y in max_path:
    print(x + 1, y + 1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126144501634](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126144501634.png)





### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：偷懒（）dp在学



代码：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        import math
        a=m+n-2
        b=m-1
        c=math.comb(a,b)
        return c
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126163435683](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126163435683.png)



### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：感觉对递归的理解还不是很到位 要跟着pythontutor才能搞懂TT 但是递归好简洁



代码：

```python
import math
a=input()
n=len(a)
def dfs(x):
    global a,n
    if x==n:
        return True
    for y in range(x+1,n+1):
        b=int(a[x:y])
        if b>0 and b==int(math.sqrt(b))**2:
            if dfs(y):
                return True
    return False
if dfs(0):
    print('Yes')
else:
    print('No')

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241126160416340](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241126160416340.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

还是好忙啊啊啊 老老实实看完讲义花了不少时间 感觉模板已经熟悉了 做题还是磕磕绊绊 



