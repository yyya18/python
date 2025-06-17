# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）⽉考： AC1。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：



代码：

```python
a=list(map(int,input().split()))
n=len(a)
b=list((a[i+1]-a[i]) for i in range(n-1))
pre=[0]*(n-1)
pre[0]=b[0]
for i in range(n-2):
    pre[i+1]=pre[i]+b[i+1]
def max_pre(pre):
    maxi=float('-inf')
    mini=pre[0]
    for i in range(n-1):
        if pre[i]-mini>maxi:
            maxi=pre[i]-mini
        if pre[i]<mini:
            mini=pre[i]
    return maxi
c=max_pre(pre)

if c<=0:
    print(0)
else:
    print(c)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241208214745320](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241208214745320.png)



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：贪心策略需要一些灵感来想 但是严格证明太困难了 考试的时候真的想得到吗。。



代码：

```python
n,k=map(int,input().split())
t=list(map(int,input().split()))
t.sort()
s=sum(t)
while True:
    if t[-1]>s/k:
        s-=t.pop()
        k-=1
    else:
        print(f'{s/k:.3f}')
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241209225940948](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241209225940948.png)



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：开两个dp数组简洁很多 考试的时候直接三重循环 但是递推的逻辑还是要想清楚的 dp1和dp2分开写结果就不同了 学了一下kadane算法计算最大子数组和

代码：

```python
a=list(map(int,input().split(',')))
n=len(a)
dp1=a[0]
dp2=a[0]
ans=0
for i in range(1,n):
    dp1,dp2=max(dp1+a[i],a[i]),max(dp1,dp2+a[i])
    ans=max(ans,dp2)
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210163647475](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241210163647475.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：



代码：

```python
result=float('inf')
n,m=map(int,input().split())
store_prices=[input().split() for _ in range(n)]
cheap=[input().split() for _ in range(m)]
l=[0]*m
def dfs(i,sum1):
    global result
    if i==n:
        jian=0
        for j in range(m):
            store_j=0
            for k in cheap[j]:
                a,b=map(int,k.split('-'))
                if l[j]>=a:
                    store_j=max(store_j,b)
            jian+=store_j
        result=min(result,sum1-(sum1//300)*50-jian)
        return
    for k in store_prices[i]:
        idx,p=map(int,k.split(':'))
        l[idx-1]+=p
        dfs(i+1,sum1+p)
        l[idx-1]-=p
dfs(0,0)
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210200341517](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241210200341517.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：卡时间 不会优化（）要学会利用题目里有且只有两个孤岛的条件 在找到一个1后马上开始扩展



代码：

```python
from collections import deque

def dfs(x, y, grid, n, queue, directions):
    grid[x][y] = 2
    queue.append((x, y))
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] == 1:
            dfs(nx, ny, grid, n, queue, directions)

def bfs(grid, n, queue, directions):
    distance = 0
    while queue:
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n:
                    if grid[nx][ny] == 1:
                        return distance
                    elif grid[nx][ny] == 0:
                        grid[nx][ny] = 2 
                        queue.append((nx, ny))
        distance += 1
    return distance

def main():
    n = int(input())
    grid = [list(map(int, input())) for _ in range(n)]
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    queue = deque()

    for i in range(n):
        for j in range(n):
            if grid[i][j] == 1:
                dfs(i, j, grid, n, queue, directions)
                return bfs(grid, n, queue, directions)

if __name__ == "__main__":
    print(main())
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210172110751](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241210172110751.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：感觉主要是贪心策略 以及最大值不一定是在最后一个取到



代码：

```python
n=int(input())
a0,b0=map(int,input().split())
numbers=[]
for _ in range(n):
    a,b=map(int,input().split())
    numbers.append((a,b))
numbers.sort(key=lambda x:(x[0]*x[1]))
result=0
for i in range(n):
    result=max(result,a0//numbers[i][1])
    a0*=numbers[i][0]
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210202807875](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241210202807875.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

月考很难 虽然大多数人ac得都不多 但我ac一题耗时还是比别人多得多 期末还是很慌  



