# CheatSheet

Compiled by HUJingyao

Wednesday 加油！ 加油加油坚持一下！

### Tips

1.注意数据范围！不要被样例骗了：O and B数字是r[4:]，而不是r[4]。

2.不要怕WA，冷静分析，有没有没想周全的地方，想想边界条件，特殊情况（某个量是0）。

3.利用完全二叉树的性质（子节点下标2*i+1 和2*i+2），可以直接操作数组，不用大费周章建树。

# 数据结构

### 并查集(DSU)（无向图连通性 合并 查找）

##### ！单个量自成一个连通分量 函数内都已经计算过了 不要再加孤立点的值了！直接输出各个连通分量的值就行！

```python
parent=[]
rank=[]
def init_dsu(n): # 注意编号 这个模板是0到n-1
    global parent,rank
    parent=list(range(n)) # 初始化每个节点的父节点为自己
    rank=[0]*n # 初始化秩（树高度）
def find(x): # 路径压缩
    if parent[x]!=x:
        parent[x]=find(parent[x]) # 递归压缩路径
    return parent[x]
def union(x,y): # 按秩合并
    x_root=find(x)
    y_root=find(y)
    if x_root==y_root:
        return
    if rank[x_root]<rank[y_root]:
        parent[x_root]=y_root
    else:
        parent[y_root]=x_root
        if rank[x_root]==rank[y_root]:
            rank[x_root]+=1
            
def is_connected(n,edges): # 判断连通性
    init_dsu(n)
    for u,v in edges:
        union(u,v)
    num_components=len(set(find(i) for i in range(n))) # 统计连通分量数量
    root=find(0)
    return all(find(i)==root for i in range(n))
```

#### 判断等价关系是否成立

```python
def equationsPossible(equations):
    def find(x):
        if x!=uf[x]:
            uf[x]=find(uf[x])
        return uf[x]
    uf={chr(i):chr(i) for i in range(97,123)} # 初始化：uf={'a':'a','b':'b'...}
    for eq in equations:
        if eq[1]=='=':
            a,b=eq[0],eq[3]
            uf[find(a)]=find(b) # 合并
    for eq in equations:
        if eq[1]=='!':
            a,b=eq[0],eq[3]
            if find(a)==find(b):
                return False
    return True
n=int(input())
equations=[input().strip() for _ in range(n)]
result=equationsPossible(equations)
print('True' if result else 'False')
```

#### 宗教信仰

```python
#前面全是模板 最后返回的是连通分量数量
test_case=1
while True:
    edges=[]
    n,m=map(int,input().split())
    if n==0 and m==0:
        break
    for _ in range(m):
        a,b=map(int,input().split())
        edges.append((a-1,b-1)) # 注意学生编号
    num=num_connected(n,edges)
    print(f'Case {test_case}: {num}') # 注意单独的学生也是一个连通分量了
    test_case+=1
```

#### 谣言

```python
#前面就是模板 init_dsu find union函数
n,m=map(int,input().split())
coins=list(map(int,input().split()))
pair=[(i,coin) for i,coin in enumerate(coins)]
graph=[[] for _ in range(n)]
for _ in range(m):
    u,v=map(int,input().split())
    graph[u-1].append(v-1)
    graph[v-1].append(u-1)
init_dsu(n)
for i in range(len(graph)): # 注意二维列表的写法
    for j in graph[i]:
        union(i,j)
total=0
groups=defaultdict(list)
for i,coin in pair:
    groups[find(i)].append(coin)
for money in groups.values():
    if money:
        total+=int(min(money))
print(total)
```

#### 针对图的路径存在性查询

```python
def find(u):#路径压缩
    while parent[u]!=u:
        parent[u]=parent[parent[u]]
        u=parent[u]
    return u
left=0
for right in range(n):
    while nums[right]-nums[left]>maxDiff:
        left+=1
    root_r=find(right)
    for i in range(left,right):
        root_i=find(i)
        if root_i!=root_r:
            parent[root_i]=root_r
ans=[]
for u,v in queries:
    ans.append('true' if find(u)==find(v) else 'false')
print('\n'.join(ans))
```

### 字典树(Trie)

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # 字符到子节点的映射
        self.is_end = False  # 标记是否为单词结尾
class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end = True
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end  # 必须是一个完整单词
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True  # 前缀存在即可
```

#### 电话号码

```python
class TrieNode:
    def __init__(self):
        self.child={}
class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, nums):
        curnode = self.root
        for x in nums:
            if x not in curnode.child:
                curnode.child[x] = TrieNode()
            curnode=curnode.child[x]
    def search(self, num):
        curnode = self.root
        for x in num:
            if x not in curnode.child:
                return 0
            curnode = curnode.child[x]
        return 1
t = int(input())
p = []
for _ in range(t):
    n = int(input())
    nums = []
    for _ in range(n):
        nums.append(str(input()))
    nums.sort(reverse=True)
    s = 0
    trie = Trie()
    for num in nums:
        s += trie.search(num)
        trie.insert(num)
    if s > 0:
        print('NO')
    else:
        print('YES')
```

# 算法

### 树的遍历     前序：根→左→右  中序：左→根→右  后序：左→右→根

#### 递归实现（以根→右→左为例）

```python
def root_right_left(node):
    if node:
        print(node.data)       # 访问根节点
        root_right_left(node.right)  # 遍历右子树
        root_right_left(node.left)   # 遍历左子树
```

#### 迭代实现（以根→右→左为例）

```python
def root_right_left_iterative(node):
    if not node:
        return
    stack = [node]
    while stack:
        current = stack.pop()
        print(current.data)  # 访问当前节点
        if current.left: # 注意入栈顺序：先左后右，因为栈是LIFO
            stack.append(current.left)
        if current.right:
            stack.append(current.right)
```

#### 通过中序和后序转层序

```python
from collections import deque
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None
def build_tree(inorder,postorder):
    if inorder:
        root=Node(postorder.pop())
        root_index=inorder.index(root.data)
         # 先构建右子树，再构建左子树（因为后序是左右根，pop顺序是根->右->左）
        root.right=build_tree(inorder[root_index+1:],postorder)
        root.left=build_tree(inorder[:root_index],postorder)
        return root
def level_order_traversal(root):
    if root is None:
        return []
    result=[]
    queue=deque([root])
    while queue:
        node=queue.popleft()
        result.append(node.data)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return result
n=int(input())
for _ in range(n):
    inorder=list(input().strip())
    postorder=list(input().strip())
    root=build_tree(inorder,postorder)
    print(''.join(level_order_traversal(root)))
```

### Huffman

```python
import heapq
class Node:
    def __init__(self, weight, char=None):
        self.weight = weight
        self.char = char
        self.left = None
        self.right = None
    def __lt__(self, other):
        if self.weight == other.weight:
            return self.char < other.char
        return self.weight < other.weight
def huffman_encoding(char_freq):
    heap = [Node(freq, char) for char, freq in char_freq.items()]
    heapq.heapify(heap)
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(left.weight + right.weight, min(left.char, right.char))
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
    return heap[0]
def external_path_length(node, depth=0):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return depth * node.weight
    return (external_path_length(node.left, depth + 1) +external_path_length(node.right, depth + 1))
```

### 二分查找

#### 厚道的调分方法

```python
def is_ok(b,scores):
    a=b/1e9
    cnt=0
    threshold=0.6*len(scores)
    for x in scores:
        new_score=a*x+1.1**(a*x)
        if new_score>=85:
            cnt+=1
    return cnt>=threshold
scores=[float(x) for x in input().split()]
l,r=1,10**9+1
while l<r:
    mid=(l+r)//2
    if is_ok(mid,scores):
        r=mid
    else:
        l=mid+1
print(r)
```

### 回溯

#### 电话号码的字母组合

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return list()
        phoneMap={'2':'abc','3':'def','4':'ghi','5':'jkl','6':'mno','7':'pqrs','8':'tuv','9':'wxyz'}
        def backtrack(index):
            if index==len(digits):
                combinations.append('',join(combination))
            else:
                digit=digits[index]
                for letter in phoneMap[digit]:
                    combination.append(letter)
                    backtrack(index+1)
                    combination.pop()
        combination=list()
        combinations=list()
        backtrack(0)
        return combinations
```

#### N皇后

```python
n=int(input())
def generateBoard():
    board=[]
    for i in range(n):
        row[queens[i]]='Q'
        board.append(''.join(row))
        row[queens[i]]='.'
    return board
def backtrack(row):
    if row==n:
        board=generateBoard()
        solutions.append(board)
    else:
        for i in range(n):
            if i in columns or row-i in diagonal1 or row+i in diagonal2:
                continue
            queens[row]=i
            columns.add(i)
            diagonal1.add(row-i)
            diagonal2.add(row+i)
            backtrack(row+1)
            columns.remove(i)
            diagonal1.remove(row-i)
            diagonal2.remove(row+i)
solutions=[]
queens=[-1]*n
columns=set()
diagonal1=set()
diagonal2=set()
row=['.']*n
backtrack(0)
print(solutions)
```

#### 马走日

```python
sx = [-2,-1,1,2, 2, 1,-1,-2]
sy = [ 1, 2,2,1,-1,-2,-2,-1]
ans=0
def dfs(dep,x,y):
    if dep==n*m:
        global ans
        ans+=1
        return
    for r in range(8):
        s=x+sx[r]
        t=y+sy[r]
        if 0<=s<n and 0<=t<m and chess[s][t]==False:
            chess[s][t]=True
            dfs(dep+1,s,t)
            chess[s][t]=False
for _ in range(int(input())):
    n,m,x,y=map(int,input().split())
    chess=[[False]*m for i in range(n)]
    ans=0
    chess[x][y]=True
    dfs(1,x,y)
    print(ans)
```

### 树形DP

```python
from typing import List, Optional
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
def tree_dp(root: Optional[TreeNode]) -> int: # 返回值通常为题目所需的计算结果（如最大路径和、节点数等）
    def dfs(node: Optional[TreeNode]) -> int:
        if not node:
            return 0  # 根据题意返回基准值（如空节点返回0或-inf）
        left = dfs(node.left)
        right = dfs(node.right)
        # 例1：求二叉树最大路径和（路径可跨根节点）
        nonlocal max_sum
        max_sum = max(max_sum, left + right + node.val)
        # 例2：返回当前子树的最大贡献值（只能选左或右分支）
        return max(left, right) + node.val
    max_sum = float('-inf')  # 初始化全局变量（根据题目调整）
    dfs(root)
    return max_sum
```

## 排序

### Merge Sort

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])  # 递归排序左半部分
    right = merge_sort(arr[mid:]) # 递归排序右半部分
    merged = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            merged.append(left[i])
            i += 1
        else:
            merged.append(right[j])
            j += 1
    merged.extend(left[i:])
    merged.extend(right[j:])
    return merged
```

### Quick Sort

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[-1]  # 选择最后一个元素作为基准
    left = [x for x in arr[:-1] if x <= pivot]  # 小于等于基准的放左边
    right = [x for x in arr[:-1] if x > pivot]  # 大于基准的放右边
    return quick_sort(left) + [pivot] + quick_sort(right)  # 递归拼接
```

```python
def quick_sort_inplace(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)  # 获取分区点
        quick_sort_inplace(arr, low, pi - 1)  # 递归排序左半部分
        quick_sort_inplace(arr, pi + 1, high)  # 递归排序右半部分
def partition(arr, low, high):
    pivot = arr[high]  # 选择最后一个元素作为基准
    i = low - 1  # 指向小于基准的区域的末尾  
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]  # 交换   
    arr[i + 1], arr[high] = arr[high], arr[i + 1]  # 将基准放到正确位置
    return i + 1  # 返回基准的索引
quick_sort_inplace(arr, 0, len(arr) - 1)
```

## 字符串算法

### KMP

```python
def compute_lps(pattern): # pattern每个子串的最长公共前后缀长度
    m=len(pattern)
    lps=[0]*m
    length=0
    for i in range(1,m):
        while length>0 and pattern[i]!=pattern[length]:
            length=pattern[length-1]
        if pattern[i]==pattern[length]:
            length+=1
        lps[i]=length
    return lps
def kmp_search(text,pattern): # 利用LPS数组在text中高效查找所有pattern的出现位置。
    n=len(text)
    m=len(pattern)
    if m==0:
        return 0
    lps=compute_lps(pattern)
    matches=[]
    j=0
    for i in range(n):
        while j>0 and text[i]!=pattern[j]:
            j=lps[j-1]
        if text[i]==pattern[j]:
            j+=1
        if j==m:
            matches.append(i-j+1)
            j=lps[j-1]
    return matches
```

#### 字符串乘方

```python
import sys
while True:
    s=sys.stdin.readline().strip()
    if s=='.':
        break
    n=len(s)
    next=[0]*n
    j=0
    for i in range(1,n):
        while j>0 and s[i]!=s[j]:
            j=next[j-1]
        if s[i]==s[j]:
            j+=1
        next[i]=j
    p=len(s)-next[-1]
    if n%p==0:
        print(n//p)
    else:
        print(1)
```

#### 前缀中的周期

```python
def kmp_next(s):
    next = [0] * len(s)
    j = 0
    for i in range(1, len(s)):
        while s[i] != s[j] and j > 0:
            j = next[j - 1]
        if s[i] == s[j]:
            j += 1
        next[i] = j
    return next
case = 0
while True:
    n = int(input().strip())
    if n == 0:
        break
    s = input().strip()
    case += 1
    print("Test case #{}".format(case))
    next = kmp_next(s)
    for i in range(2, len(s) + 1):
        k = i - next[i - 1]		# 可能的重复子串的长度
        if (i % k == 0) and i // k > 1:
            print(i, i // k)
    print()
```

## 图遍历

### DFS

#### 堆路径（输出路径部分）

```python
def dfs(arr,a,path,paths,n):
    if a>=n: # 安全边界 优先访问
        return # 退出递归
    path.append(arr[a])
    if 2*a+1>=n: # 递归到叶子节点 加入路径
        paths.append(' '.join(map(str,path)))
    else:
        if 2*a+2<n:
            dfs(arr,2*a+2,path.copy(),paths,n)
        if 2*a+1<n:
            dfs(arr,2*a+1,path.copy(),paths,n)
```

#### 判断无向图是否连通&成环

```python
n,m=map(int,input().split())
edge=[[] for _ in range(n)]
for _ in range(m):
    a,b=map(int,input().split())
    edge[a].append(b)
    edge[b].append(a)
visited=[False]*n
has_cycle=False
def dfs(x,y): # (子节点，父节点)
    global has_cycle
    visited[x]=True
    for i in edge[x]:
        if not visited[i]:
            dfs(i,x)
        elif y!=i: # 已经遍历过并且不是父亲节点
            has_cycle=True
dfs(0,-1) # 0的父亲节点-1（不存在）
is_connected=all(visited) # 是否连通
print('connected:'+('yes' if is_connected else 'no'))
print('loop:'+('yes' if has_cycle else 'no'))
```

#### 有向图判环

```python
def has_cycle(n,edges):
    graph=[[] for _ in range(n)]
    for u,v in edges:
        graph[u].append(v)
    visited=[False]*n
    rec_stack=[False]*n
    def dfs(node):
        if rec_stack[node]:
            return True
        if visited[node]:
            return False
        visited[node]=True
        rec_stack[node]=True
        for nei in graph[node]:
            if dfs(nei):
                return True
        rec_stack[node]=False
    for i in range(n):
        if not visited[i] and dfs(i):
            return 'Yes'
    return 'No'
n,m=map(int,input().split())
edges=[]
for _ in range(m):
    u,v=map(int,input().split())
    edges.append((u,v))
print(has_cycle(n,edges))
```

#### 骑士周游

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
n = int(sys.stdin.readline().strip())
sr, sc = map(int, sys.stdin.readline().strip().split())
knights_tour_warnsdorff(n, sr, sc)
```

#### 省份数量

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(i):
            for j in range(cities):
                if isConnected[i][j]==1 and j not in visited:
                    visited.add(j)
                    dfs(j)
        cities=len(isConnected)
        visited=set()
        provinces=0
        for i in range(cities):
            if i not in visited:
                dfs(i)
                provinces+=1
        return provinces
```

### BFS

#### 省份数量

```python
from collections import deque
n=int(input())
isConnected=[]
for _ in range(n):
    row=list(map(int,input().split()))
    isConnected.append(row)
visited=[False]*n
provinces=0
for i in range(n):
    if not visited[i]:
        queue=deque([i])
        visited[i]=True
        provinces+=1
        while queue:
            cur=queue.popleft()
            for nei in range(n):
                if isConnected[cur][nei]==1 and not visited[nei]:
                    visited[nei]=True
                    queue.append(nei)
print(provinces)
```

#### 词梯

```python
from collections import defaultdict
dic=defaultdict(list)
n,lis=int(input()),[]
for i in range(n):
    lis.append(input())
for word in lis:
    for i in range(len(word)):
        bucket=word[:i]+'_'+word[i+1:]
        dic[bucket].append(word)
def bfs(start,end,dic):
    queue=[(start,[start])]
    visited=[start]
    while queue:
        curword,curpath=queue.pop(0)
        if curword==end:
            return ' '.join(curpath)
        for i in range(len(curword)):
            bucket=curword[:i]+'_'+curword[i+1:]
            for nei in dic[bucket]:
                if nei not in visited:
                    visited.append(nei)
                    newpath=curpath+[nei]
                    queue.append((nei,newpath))
    return 'NO'
start,end=map(str,input().split())
print(bfs(start,end,dict))
```

#### 献给阿尔吉侬的花束

```python
from collections import deque
t=int(input())
for _ in range(t):
    r,c=map(int,input().split())
    maze=[list(input().strip()) for _ in range(r)]
    for i in range(r):
        for j in range(c):
            if maze[i][j]=='S':
                start=(i,j)
            if maze[i][j]=='E':
                end=(i,j)
    queue=deque()
    visited=[[False]*c for _ in range(r)]
    queue.append((start[0],start[1],0))
    visited[start[0]][start[1]]=True
    found=False
    while queue:
        x,y,dist=queue.popleft()
        if (x,y)==end:
            print(dist)
            found=True
            break
        for dx,dy in [(-1,0),(1,0),(0,-1),(0,1)]:
            nx,ny=x+dx,y+dy
            if 0<=nx<r and 0<=ny<c and maze[nx][ny]!='#' and not visited[nx][ny]:
                visited[nx][ny]=True
                queue.append((nx,ny,dist+1))
    if not found:
        print('oop!')
```

#### 网格传送门旅游

```python
from collections import defaultdict,deque
class Solution:
    def minMoves(self,matrix):
        if matrix[-1][-1]=='#':
            return -1
        m,n=len(matrix),len(matrix[0])
        pos=defaultdict(list)
        for i,row in enumerate(matrix):
            for j,c in enumerate(row):
                if c.isupper():
                    pos[c].append((i,j))
        dirs=[(0,-1),(0,1),(-1,0),(1,0)]
        dis=[[float('inf')]*n for _ in range(m)]
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
                        q.append((px,py))
                del pos[c]
            for dx,dy in dirs:
                nx,ny=x+dx,y+dy
                if 0<=nx<m and 0<=ny<n and matrix[nx][ny]!='#' and d+1<dis[nx][ny]:
                    dis[nx][ny]=d+1
                    q.append((nx,ny))
        return -1
```

## 最短路径

### Dijkstra（贪心）

#### 两种模板

##### heap+visited

```python
import heapq
def dijkstra_visited(n, edges, start):
    graph = [[] for _ in range(n)]
    for u, v, w in edges:
        graph[u].append((v, w))
    heap = [(0, start)]
    visited = set()
    distance = [float('inf')] * n
    distance[start] = 0
    while heap:
        dist, u = heapq.heappop(heap)
        if u in visited:
            continue
        visited.add(u)
        for v, w in graph[u]:
            if v not in visited and distance[v] > dist + w:
                distance[v] = dist + w
                heapq.heappush(heap, (distance[v], v))
    return distance
```

##### heap+distance

```python
import heapq
def dijkstra_distance(n, edges, start):
    graph = [[] for _ in range(n)]
    for u, v, w in edges:
        graph[u].append((v, w)) 
    heap = [(0, start)]
    distance = [float('inf')] * n
    distance[start] = 0
    while heap:
        dist, u = heapq.heappop(heap)
        if dist > distance[u]:  # 关键：跳过旧数据
            continue
        for v, w in graph[u]:
            if distance[v] > dist + w:
                distance[v] = dist + w
                heapq.heappush(heap, (distance[v], v))
    return distance
```

#### Candies

```python
import heapq
def dijkstra(N,G,start):
    INF=float('inf')
    dist=[INF]*(N+1)
    dist[start]=0
    pq=[(0,start)]
    while pq:
        d,node=heapq.heappop(pq)
        if d>dist[node]:
            continue
        for neighbor,weight in G[node]:
            new_dist=dist[node]+weight
            if new_dist<dist[neighbor]:
                dist[neighbor]=new_dist
                heapq.heappush(pq,(new_dist,neighbor))
    return dist
N,M=map(int,input().split())
G=[[] for _ in range(N+1)]
for _ in range(M):
    u,v,w=map(int,input().split())
    G[u].append((v,w))
start_node=1
s=dijkstra(N,G,start_node)
print(s[-1])
```

#### 兔子与樱花🌸

```python
import heapq
from collections import defaultdict
p=int(input())
points=[input().strip() for _ in range(p)]
maps=defaultdict(list)
for _ in range(int(input())):
    a,b,d=input().split()
    d=int(d)
    maps[a].append((b,d))
    maps[b].append((a,d))
def dijkstra(src,dst):
    INF=float('inf')
    dist={point:INF for point in points}
    path={point:'' for point in points}
    dist[src]=0
    path[src]=src
    pq=[(0,src)]
    while pq:
        d,u=heapq.heappop(pq)
        if d>dist[u]:
            continue
        if u==dst:
            break
        for v,w in maps[u]:
            nd=d+w
            if nd<dist[v]:
                dist[v]=nd
                path[v]=path[u]+f'->({w})->'+v
                heapq.heappush(pq,(nd,v))
    return path[dst]
for _ in range(int(input())):
    s,t=input().split()
    print(dijkstra(s,t))
```

#### 走山路

```python
import heapq
m,n,p=map(int,input().split())
info=[]
for _ in range(m):
    info.append(list(input().split()))
directions=[(-1,0),(1,0),(0,1),(0,-1)]
def dijkstra(sr,sc,er,ec):
    pos=[]
    dist=[[float('inf')]*n for _ in range(m)]
    if info[sr][sc]=='#':
        return 'NO'
    dist[sr][sc]=0
    heapq.heappush(pos,(0,sr,sc))
    while pos:
        d,r,c=heapq.heappop(pos)
        if r==er and c==ec:
            return d
        if d>dist[r][c]:
            continue
        h=int(info[r][c])
        for dr,dc in directions:
            nr,nc=r+dr,c+dc
            if 0<=nr<m and 0<=nc<n and info[nr][nc]!='#':
                if dist[nr][nc]>d+abs(int(info[nr][nc])-h):
                    dist[nr][nc]=d+abs(int(info[nr][nc])-h)
                    heapq.heappush(pos,(dist[nr][nc],nr,nc))
    return 'NO'
for _ in range(p):
    x,y,z,w=map(int,input().split())
    print(dijkstra(x,y,z,w))
```

### Floyd-Warshall（多源）(dp)

```python
def floyd_warshall(graph):
    n = len(graph)
    dist = [[float('inf')] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i == j:
                dist[i][j] = 0
            elif j in graph[i]:
                dist[i][j] = graph[i][j]
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist
```

### *Bellman-Ford（负权）(dp)

```python
def bellman_ford(n, edges, src):
    dist = [float('inf')] * n
    dist[src] = 0
    # Step 1: Relax all edges V-1 times
    for _ in range(n - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                updated = True
        if not updated:  # Early stop if no update
            break
    # Step 2: Check for negative cycles
    has_negative_cycle = False
    for u, v, w in edges:
        if dist[u] + w < dist[v]:
            has_negative_cycle = True
            break
    return dist, has_negative_cycle
```

### *SPFA（Bellman-Ford优化）

```python
from collections import deque
def spfa(n, edges, src):
    graph = [[] for _ in range(n)]
    for u, v, w in edges:
        graph[u].append((v, w))
    dist = [float('inf')] * n
    dist[src] = 0
    queue = deque([src])
    in_queue = [False] * n
    in_queue[src] = True
    while queue:
        u = queue.popleft()
        in_queue[u] = False
        for v, w in graph[u]:
            if dist[v] > dist[u] + w:
                dist[v] = dist[u] + w
                if not in_queue[v]:
                    queue.append(v)
                    in_queue[v] = True
    return dist
```

## Topological Sorting(有向无环图)

```python
from collections import deque
def topological_sort(graph):
    in_degree = {u: 0 for u in graph}  # 初始化入度
    for u in graph:
        for v in graph[u]:
            in_degree[v] += 1  # 统计入度
    queue = deque([u for u in graph if in_degree[u] == 0])  # 入度为0的节点
    topo_order = []
    while queue:
        u = queue.popleft()
        topo_order.append(u)
        for v in graph[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                queue.append(v)
    if len(topo_order) != len(graph):  # 存在环
        return None
    return topo_order
```

### Kahn（基于入度）

#### 舰队海域出击

```python
from collections import deque,defaultdict
def topo_sort(graph):
    in_degree={u:0 for u in range(1,n+1)}
    for u in graph:
        for v in graph[u]:
            in_degree[v]+=1
    q=deque([u for u in in_degree if in_degree[u]==0])
    topo_order=[]
    while q:
        u=q.popleft()
        topo_order.append(u)
        for v in graph[u]:
            in_degree[v]-=1
            if in_degree[v]==0:
                q.append(v)
    if len(topo_order)!=len(graph):
        return 'Yes'
    return 'No'
for _ in range(int(input())):
    n,m=map(int,input().split())
    graph=defaultdict(list)
    for _ in range(m):
        u,v=map(int,input().split())
        graph[u].append(v)
    print(topo_sort(graph))
```

#### 最小奖金方案

```python
import sys
from collections import defaultdict,deque
def min_bonus(n,m,matches):
    graph=defaultdict(list)
    indegree=[0]*n
    for a,b in matches:
        graph[b].append(a)
        indegree[a]+=1
    bonus=[100]*n
    queue=deque([i for i in range(n) if indegree[i]==0])
    while queue:
        curr=queue.popleft()
        for neighbor in graph[curr]:
            if bonus[neighbor]<=bonus[curr]:
                bonus[neighbor]=bonus[curr]+1
            indegree[neighbor]-=1
            if indegree[neighbor]==0:
                queue.append(neighbor)
    return sum(bonus)
input=sys.stdin.read
data=input().split()
n=int(data[0])
m=int(data[1])
matches=[]
idx=2
for _ in range(m):
    a=int(data[idx])
    b=int(data[idx+1])
    matches.append((a,b))
    idx+=2
print(min_bonus(n,m,matches))
```

### DFS（三色法）

#### 舰队海域出击

```python
from collections import defaultdict
def dfs(node, color):
    color[node] = 1
    for neighbour in graph[node]:
        if color[neighbour] == 1:
            return True
        if color[neighbour] == 0 and dfs(neighbour, color):
            return True
    color[node] = 2
    return False
T = int(input())
for _ in range(T):
    N, M = map(int, input().split())
    graph = defaultdict(list)
    for _ in range(M):
        x, y = map(int, input().split())
        graph[x].append(y)
    color = [0] * (N + 1)
    is_cyclic = False
    for node in range(1, N + 1):
        if color[node] == 0:
            if dfs(node, color):
                is_cyclic = True
                break
    print("Yes" if is_cyclic else "No")
```

## 最小生成树（MST）

### Kruskal（边列表输入）

#### 兔子与星空

```python
class DisjSet:
    def __init__(self, n):
        self.parent = [i for i in range(n)]
        self.rank = [0]*n  
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]    
    def union(self, x, y):
        xset, yset = self.find(x), self.find(y)
        if self.rank[xset] > self.rank[yset]:
            self.parent[yset] = xset
        else:
            self.parent[xset] = yset
            if self.rank[xset] == self.rank[yset]:
                self.rank[yset] += 1
def kruskal(n, edges):
    dset = DisjSet(n)
    edges.sort(key = lambda x:x[2])
    sol = 0
    for u, v, w in edges:
        u, v = ord(u)-65, ord(v)-65
        if dset.find(u) != dset.find(v):
            dset.union(u, v)
            sol += w
    if len(set(dset.find(i) for i in range(n))) > 1:
        return -1
    return sol
n = int(input())
edges = []
for _ in range(n-1):
    arr = input().split()
    root, m = arr[0], int(arr[1])
    for i in range(m):
        edges.append((root, arr[2+2*i], int(arr[3+2*i])))
print(kruskal(n, edges))
```

### Prim（堆优化）（邻接表输入）

#### 兔子与星空

```python
import heapq
def prim(graph, start):
    mst = []
    used = set([start])
    edges = [(cost, start, to) for to, cost in graph[start].items()]
    heapq.heapify(edges)
    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in used:
            used.add(to)
            mst.append((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in used:
                    heapq.heappush(edges, (cost2, to, to_next))
    return mst
n = int(input())
graph = {chr(i+65): {} for i in range(n)}
for i in range(n-1):
    data = input().split()
    star = data[0]
    m = int(data[1])
    for j in range(m):
        to_star = data[2+j*2]
        cost = int(data[3+j*2])
        graph[star][to_star] = cost
        graph[to_star][star] = cost
    mst = prim(graph, 'A')
    print(sum(x[2] for x in mst))
```

# 其他

1. 检查字符串是否以指定的前缀开头：str.startswith(prefix[, start[, end]])  e.g. text = "Hello, World!"   text.startswith("Hello")

