# Assignment #C: 20250514 Mock Exam

Updated 1518 GMT+8 May 14, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



> **说明：**
>
> 1. **⽉考**：未参加 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
> 2. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 4. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E06364: 牛的选举

http://cs101.openjudge.cn/practice/06364/

思路：很基础了



代码：

```python
n, k = map(int, input().split())
cows = []
for i in range(n):
    a, b = map(int, input().split())
    cows.append((a, b, i + 1))
first_round = sorted(cows, key=lambda x: x[0], reverse=True)
top_k = first_round[:k]
second_round = sorted(top_k, key=lambda x: x[1], reverse=True)
print(second_round[0][2])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518150001428](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518150001428.png)



### M040677: 出栈序列统计

http://cs101.openjudge.cn/practice/04077/

思路：



代码：

```python
def count_sequences(n):
    def dfs(push_num,stack,popped):
        nonlocal count
        if popped==n:
            count+=1
            return
        if push_num<=n:
            stack.append(push_num)
            dfs(push_num+1,stack,popped)
            stack.pop()
        if stack:
            top=stack.pop()
            dfs(push_num,stack,popped+1)
            stack.append(top)
    count=0
    dfs(1,[],0)
    return count
n=int(input())
print(count_sequences(n))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518162822315](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518162822315.png)



### M05343:用队列对扑克牌排序

http://cs101.openjudge.cn/practice/05343/

思路：



代码：

```python
from collections import deque
n=int(input())
queues=[deque() for _ in range(9)]
cards=deque(list(input().split()))
while cards:
    card=cards.popleft()
    queues[int(card[1])-1].append(card)
qs={'A':deque(),'B':deque(),'C':deque(),'D':deque()}
for i in range(9):
    tmp=[]
    while queues[i]:
        card=queues[i].popleft()
        qs[card[0]].append(card)
        tmp.append(card)
    print(f'Queue{i+1}:'+' '.join(tmp))
result=[]
for char in qs.keys():
    tmp=[]
    while qs[char]:
        card=qs[char].popleft()
        result.append(card)
        tmp.append(card)
    print(f'Queue{char}:'+' '.join(tmp))
print(*result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518203208702](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518203208702.png)



### M04084: 拓扑排序

http://cs101.openjudge.cn/practice/04084/

思路：



代码：

```python
v,a=map(int,input().split())
node=['v'+str(i) for i in range(v+1)]
dic1={i:0 for i in node}
dic2={i:[] for i in node}
for _ in range(a):
    f,t=map(int,input().split())
    dic1[node[t]]+=1
    dic2[node[f]].append(node[t])
vis=set()
cnt=0
ans=[]
while cnt<v:
    for i in range(1,v+1):
        if dic1[node[i]]==0 and node[i] not in vis:
            vis.add(node[i])
            ans.append(node[i])
            cnt+=1
            for nodes in dic2[node[i]]:
                dic1[nodes]-=1
            break
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518214512246](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518214512246.png)



### M07735:道路

Dijkstra, http://cs101.openjudge.cn/practice/07735/

思路：



代码：

```python
import heapq
k,n,r=int(input()),int(input()),int(input())
graph={i:[] for i in range(1,n+1)}
for _ in range(r):
    s,d,dl,dt=map(int,input().split())
    graph[s].append((dl,dt,d))
que=[(0,0,1)]
fee=[10000]*101
def dijkstra(g):
    while que:
        l,t,d=heapq.heappop(que)
        if d==n:
            return l
        if t>fee[d]:
            continue
        fee[d]=t
        for dl,dt,next_d in g[d]:
            if t+dt<=k:
                heapq.heappush(que,(l+dl,t+dt,next_d))
    return -1
print(dijkstra(graph))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518220632090](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518220632090.png)



### T24637:宝藏二叉树

dp, http://cs101.openjudge.cn/practice/24637/

思路：



代码：

```python
import sys
sys.setrecursionlimit(10**7)

def max_treasure(N, vals):
    # vals: 1-based list of length N+1
    # f[i][0], f[i][1]
    f0 = [0] * (N + 1)
    f1 = [0] * (N + 1)
    for i in range(N, 0, -1):
        l, r = 2*i, 2*i+1
        include = vals[i]
        if l <= N:
            include += f0[l]
        if r <= N:
            include += f0[r]
        f1[i] = include
        not_include = 0
        if l <= N:
            not_include += max(f0[l], f1[l])
        if r <= N:
            not_include += max(f0[r], f1[r])
        f0[i] = not_include
    
    return max(f0[1], f1[1])

if __name__ == "__main__":
    import sys
    data = sys.stdin.read().split()
    N = int(data[0])
    vals = [0] + list(map(int, data[1:]))
    print(max_treasure(N, vals))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518221827679](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250518221827679.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

ddl不紧不慢前仆后继× 但是无论如何要开始恶补数算了 感觉把一些算法的基本原理弄清楚做题时思路确实会清晰一些 有时候看到名字还得反应一下基本模板是啥）别偷懒！机考好运！









