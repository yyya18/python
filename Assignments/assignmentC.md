# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：设a是较大数，如果a=b或者a>=2b，这时取的人就赢了，如果b<a<2b，取的情况固定，不能保证赢。

计数器i判断第二种情况经过的次数，第一种情况第一次出现时自动停止计数，也就是第一种情况会出现在先手还是后手。



代码：

```python
while True:
    a,b=map(int,input().split())
    if a==0 and b==0:
        break
    i=1
    a,b=max(a,b),min(a,b)
    while a//b<2 and a!=b:
        a,b=b,a-b
        i+=1
    if i%2==1:
        print('win')
    else:
        print('lose')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241212144523054](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241212144523054.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：自己写的时候正着写特别复杂而且还是错的 看了题解发现换个方向思考 不是确定层数去找元素而是确定元素扔进对应的层数就非常简洁明了 有点时候还是要学会逆向思考啊啊



代码：

```python
n = int(input())
matrix = [list(map(int, input().split())) for _ in range(n)]
ans = [0] * ((n + 1) // 2)
for i in range(n):
    for j in range(n):
        ans[min(i, j, n - 1 - i, n - 1 - j)] += matrix[i][j]
print(max(ans))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241212164343854](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241212164343854.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：后悔策略实现贪心算法 最小堆优化时间



代码：

```python
import heapq
def max_potions(n,potions):
    health=0
    consumed=[]
    for potion in potions:
        health+=potion
        heapq.heappush(consumed,potion)
        if health<0:
            if consumed:
                health-=consumed[0]
                heapq.heappop(consumed)
    return len(consumed)
n=int(input())
potions=list(map(int,input().split()))
print(max_potions(n,potions))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241216093027477](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241216093027477.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：学习了栈 在末端操作可以不用deque 直接用列表的append pop也是O(1)



代码：

```python
a=[]
m=[]
while True:
    try:
        s=input().split()
        if s[0]=='pop':
            if a:
                a.pop()
                if m:
                    m.pop()
        elif s[0]=='min':
            if m:
                print(m[-1])
        else:
            h=int(s[1])
            a.append(h)
            if not m:
                m.append(h)
            else:
                k=m[-1]
                m.append(min(k,h))
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241212170835405](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241212170835405.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：学习题解 问了ai好久有点明白Dijkstra和bfs的细微差别了 以及新开一个记录distance的矩阵可以简化visited的步骤 而且省时间



代码：

```python
from heapq import heappop, heappush

def bfs(x1, y1):
    q = [(0, x1, y1)]
    visited = set()
    while q:
        t, x, y = heappop(q)
        if (x, y) in visited:
          continue
        visited.add((x, y))
        if x == x2 and y == y2:
            return t  
        for dx, dy in dir:
            nx, ny = x+dx, y+dy
            if 0 <= nx < m and 0 <= ny < n and ma[nx][ny] != '#' and (nx, ny) not in visited:
                nt = t+abs(int(ma[nx][ny])-int(ma[x][y]))
                heappush(q, (nt, nx, ny))
    return 'NO'


m, n, p = map(int, input().split())
ma = [list(input().split()) for _ in range(m)]
dir = [(1, 0), (-1, 0), (0, 1), (0, -1)]
for _ in range(p):
    x1, y1, x2, y2 = map(int, input().split())
    if ma[x1][y1] == '#' or ma[x2][y2] == '#':  
        print('NO')
        continue
    print(bfs(x1, y1))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241214233253531](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241214233253531.png)



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：第一次碰到三维的 学习



代码：

```python
from collections import deque

def bfs(x, y):
    visited = {(0, x, y)}
    dx = [0, 0, 1, -1]
    dy = [1, -1, 0, 0]
    queue = deque([(0, x, y)])
    while queue:
        time, x, y = queue.popleft()
        for i in range(4):
            nx, ny = x + dx[i], y + dy[i]
            temp = (time + 1) % k
            if 0 <= nx < r and 0 <= ny < c and (temp, nx, ny) not in visited:
                cur = maze[nx][ny]
                if cur == 'E':
                    return time + 1
                elif cur != '#' or temp == 0:
                    queue.append((time + 1, nx, ny))
                    visited.add((temp, nx, ny))
    return 'Oop!'


t = int(input())
for _ in range(t):
    r, c, k = map(int, input().split())
    maze = [list(input()) for _ in range(r)]
    for i in range(r):
        for j in range(c):
            if maze[i][j] == 'S':
                print(bfs(i, j))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241215231004341](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241215231004341.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

努力补天ing 感觉搜索模板化的内容还是基本能掌握的 但是思维的部分（包括其他算法）还是需要很多时间 机考不知道会多难 求求了对我好一点吧



