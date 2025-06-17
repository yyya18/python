# Assignment #B: 图为主

Updated 2223 GMT+8 Apr 29, 2025

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

### E07218:献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

思路：



代码：

```python
from collections import deque

def solve_maze():
    T = int(input())
    for _ in range(T):
        R, C = map(int, input().split())
        maze = [list(input().strip()) for _ in range(R)]
        for i in range(R):
            for j in range(C):
                if maze[i][j] == 'S':
                    start = (i, j)
                if maze[i][j] == 'E':
                    end = (i, j)
        queue = deque()
        visited = [[False] * C for _ in range(R)]
        queue.append((start[0], start[1], 0))  # (row, col, distance)
        visited[start[0]][start[1]] = True
        found = False
        while queue:
            x, y, dist = queue.popleft()
            if (x, y) == end:
                print(dist)
                found = True
                break
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < R and 0 <= ny < C:
                    if not visited[nx][ny] and maze[nx][ny] != '#':
                        visited[nx][ny] = True
                        queue.append((nx, ny, dist + 1))
        if not found:
            print("oop!")
solve_maze()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506214907037](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506214907037.png)



### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

思路：



代码：

```python
class Solution:
    def pathExistenceQueries(self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]) -> List[bool]:
        id = [0] * n 
        for i in range(1, n):
            id[i] = id[i - 1]
            if nums[i] - nums[i - 1] > maxDiff:
                id[i] += 1

        return [id[u] == id[v] for u, v in queries]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506174640387](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506174640387.png)



### M22528:厚道的调分方法

binary search, http://cs101.openjudge.cn/practice/22528/

思路：



代码：

```python
def is_ok(b, scores):
    a = b / 1e9
    cnt = 0
    threshold = 0.6 * len(scores)
    for x in scores:
        new_score = a * x + 1.1 ** (a * x)
        if new_score >= 85:
            cnt += 1
    return cnt >= threshold
def main():
    scores = [float(x) for x in input().split()]
    l, r = 1, 10 ** 9 + 1
    ans = -1
    while l < r:
        mid = (l + r) // 2
        if is_ok(mid, scores):
            ans = mid
            r = mid
        else:
            l = mid + 1
    print(ans)
if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506215049938](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506215049938.png)



### Msy382: 有向图判环 

dfs, https://sunnywhy.com/sfbj/10/3/382

思路：



代码：

```python
def has_cycle(n, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)

    color = [0] * n

    def dfs(node):
        if color[node] == 1:
            return True
        if color[node] == 2:
            return False

        color[node] = 1
        for neighbor in graph[node]:
            if dfs(neighbor):
                return True
        color[node] = 2
        return False

    for i in range(n):
        if dfs(i):
            return "Yes"
    return "No"

n, m = map(int, input().split())
edges = []
for _ in range(m):
    u, v = map(int, input().split())
    edges.append((u, v))

print(has_cycle(n, edges))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506210814470](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506210814470.png)



### M05443:兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

思路：



代码：

```python
import heapq
from collections import defaultdict

p = int(input())
points = [input().strip() for _ in range(p)]
maps = defaultdict(list)
for _ in range(int(input())):
    a, b, d = input().split()
    d = int(d)
    maps[a].append((b, d))
    maps[b].append((a, d))

def dijkstra(src, dst):
    INF = float('inf')
    dist = {point: INF for point in points}
    path = {point: "" for point in points}
    dist[src] = 0
    path[src] = src
    pq = [(0, src)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        if u == dst:
            break
        for v, w in maps[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                path[v] = path[u] + f"->({w})->" + v
                heapq.heappush(pq, (nd, v))
    return path[dst]

for _ in range(int(input())):
    s, t = input().split()
    print(dijkstra(s, t))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506223918082](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506223918082.png)



### T28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：



代码：

```python
import sys

def is_valid_move(x, y, board, n):
    return 0 <= x < n and 0 <= y < n and board[x][y] == -1

def get_degree(x, y, board, n, moves):
    count = 0
    for dx, dy in moves:
        if is_valid_move(x + dx, y + dy, board, n):
            count += 1
    return count

def knights_tour_warnsdorff(n, sr, sc):
    moves = [(2, 1), (1, 2), (-1, 2), (-2, 1),
             (-2, -1), (-1, -2), (1, -2), (2, -1)]
    board = [[-1 for _ in range(n)] for _ in range(n)]
    board[sr][sc] = 0
    
    def backtrack(x, y, move_count):
        if move_count == n * n:
            return True
        
        next_moves = []
        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if is_valid_move(nx, ny, board, n):
                degree = get_degree(nx, ny, board, n, moves)
                next_moves.append((degree, nx, ny))
        
        next_moves.sort()
        
        for _, nx, ny in next_moves:
            board[nx][ny] = move_count
            if backtrack(nx, ny, move_count + 1):
                return True
            board[nx][ny] = -1
        
        return False
    
    if backtrack(sr, sc, 1):
        print("success")
    else:
        print("fail")

if __name__ == "__main__":
    n = int(sys.stdin.readline().strip())
    sr, sc = map(int, sys.stdin.readline().strip().split())
    knights_tour_warnsdorff(n, sr, sc)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250506224046209](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250506224046209.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

做题有点吃力，在赶了在赶了。









