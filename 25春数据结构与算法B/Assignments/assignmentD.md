# Assignment #D: 图 & 散列表

Updated 2042 GMT+8 May 20, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### M17975: 用二次探查法建立散列表

http://cs101.openjudge.cn/practice/17975/

<mark>需要用这样接收数据。因为输入数据可能分行了，不是题面描述的形式。OJ上面有的题目是给C++设计的，细节考虑不周全。</mark>

```python
import sys
input = sys.stdin.read
data = input().split()
index = 0
n = int(data[index])
index += 1
m = int(data[index])
index += 1
num_list = [int(i) for i in data[index:index+n]]
```



思路：先看了线性探查法，基本结构一样，在解决冲突时处理好增量的设置，正负号用一个新的变量来标记



代码：

```python
import sys
input=sys.stdin.read
data=input().split()
index=0
n=int(data[index])
index+=1
m=int(data[index])
index+=1
num_list=map(int,data[index:index+n])
table=[0.5]*m
result=[]
def insert_hash_table():
    for num in num_list:
        pos=num%m
        current=table[pos]
        if current==0.5 or current==num:
            table[pos]=num
            result.append(pos)
            continue
        else:
            sign=1
            cnt=1
            while True:
                now=pos+sign*(cnt**2)
                current=table[now%m]
                if current==0.5 or current==num:
                    table[now%m]=num
                    result.append(now%m)
                    break
                sign*=-1
                if sign==1:
                    cnt+=1
    return result
ans=insert_hash_table()
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250525152914688](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250525152914688.png)



### M01258: Agri-Net

MST, http://cs101.openjudge.cn/practice/01258/

思路：Prim



代码：

```python
from heapq import heappop,heappush
while True:
    try:
        n=int(input())
    except:
        break
    matrix=[]
    for i in range(n):
        matrix.append(list(map(int,input().split())))
    d,v,q,cnt=[100000 for i in range(n)],set(),[],0
    d[0]=0
    heappush(q,(d[0],0))
    while q:
        x,y=heappop(q)
        if y in v:
            continue
        v.add(y)
        cnt+=d[y]
        for i in range(n):
            if d[i]>matrix[y][i]:
                d[i]=matrix[y][i]
                heappush(q,(d[i],i))
    print(cnt)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250525163217635](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250525163217635.png)



### M3552.网络传送门旅游

bfs, https://leetcode.cn/problems/grid-teleportation-traversal/

思路：根据题目把字母之间看成边权为0，点之间看成边权为1.



代码：

```python
class Solution:
    def minMoves(self, matrix: List[str]) -> int:
        if matrix[-1][-1]=='#':
            return -1
        m,n=len(matrix),len(matrix[0])
        pos=defaultdict(list)
        for i,row in enumerate(matrix):
            for j,c in enumerate(row):
                if c.isupper():
                    pos[c].append((i,j))
        dirs=[(0,-1),(0,1),(-1,0),(1,0)]
        dis=[[inf]*n for _ in range(m)]
        dis[0][0]=0
        q=deque([(0,0)])
        while q:
            x,y=q.popleft()
            d=dis[x][y]
            if x==m-1 and y==n-1:
                return d
            c=matrix[x][y]
            if c in pos:
                for px,py in pos[c]:
                    if d<dis[px][py]:
                        dis[px][py]=d
                        q.appendleft((px,py))
                del pos[c]
            for dx,dy in dirs:
                nx,ny=x+dx,y+dy
                if 0<=nx<m and 0<=ny<n and matrix[nx][ny]!='#' and d+1<dis[nx][ny]:
                    dis[nx][ny]=d+1
                    q.append((nx,ny))
        return -1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250525170423350](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250525170423350.png)



### M787.K站中转内最便宜的航班

Bellman Ford, https://leetcode.cn/problems/cheapest-flights-within-k-stops/

思路：进行k+1次松弛 每次对dist进行备份满足最多k+1条边的限制



代码：

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        INF=float('inf')
        dist=[INF]*n
        dist[src]=0
        for _ in range(k+1):
            clone=dist.copy()
            for x,y,w in flights:
                if dist[y]>clone[x]+w:
                    dist[y]=clone[x]+w
        return dist[dst] if dist[dst]!=INF else -1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250525203159851](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250525203159851.png)



### M03424: Candies

Dijkstra, http://cs101.openjudge.cn/practice/03424/

思路：



代码：

```python
import heapq

def dijkstra(N, G, start):
    INF = float('inf')
    dist = [INF] * (N + 1)
    dist[start] = 0 
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:
            continue
        for neighbor, weight in G[node]: 
            new_dist = dist[node] + weight 
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    return dist
N, M = map(int, input().split())
G = [[] for _ in range(N + 1)]
for _ in range(M):
    s, e, w = map(int, input().split())
    G[s].append((e, w))
start_node = 1
shortest_distances = dijkstra(N, G, start_node)
print(shortest_distances[-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527210004910](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250527210004910.png)



### M22508:最小奖金方案

topological order, http://cs101.openjudge.cn/practice/22508/

思路：



代码：

```python
import sys
from collections import defaultdict, deque

def min_bonus(n, m, matches):
    graph = defaultdict(list)
    indegree = [0] * n
    
    for a, b in matches:
        graph[b].append(a)
        indegree[a] += 1
    bonus = [100] * n
    queue = deque([i for i in range(n) if indegree[i] == 0])

    while queue:
        curr = queue.popleft()
        for neighbor in graph[curr]:
            if bonus[neighbor] <= bonus[curr]:
                bonus[neighbor] = bonus[curr] + 1
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)

    return sum(bonus)

if __name__ == "__main__":
    input = sys.stdin.read
    data = input().split()
    
    n = int(data[0])
    m = int(data[1])
    
    matches = []
    idx = 2
    for _ in range(m):
        a = int(data[idx])
        b = int(data[idx+1])
        matches.append((a, b))
        idx += 2

    result = min_bonus(n, m, matches)
    print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527210309091](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250527210309091.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

感觉数算知识点也挺多的 还有好多内容 抓紧时间吧







