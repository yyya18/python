# Assignment #3: 惊蛰 Mock Exam

Updated 1641 GMT+8 Mar 5, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



> **说明：**
>
> 1. **惊蛰⽉考**：AC0（之前有点事但是半个小时第一题一直RE） 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E04015: 邮箱验证

strings, http://cs101.openjudge.cn/practice/04015



思路：'@'不能和'.'直接相连 这句话总是想当然地以为是'@.'不能出现，忽视了'.@'的情况。

另外因为index()在找不到字符串是会抛出异常，没有处理异常导致总是RE，用find()可以解决问题。

还有就是找'.'要从'@'后面找，所以有一个b+1。

小细节总是处理不好（叹气）



代码：

```python
try:
    while True:
        a=str(input())
        if a.count('@')!=1:
            print('NO')
        elif a[0]=='.' or a[0]=='@' or a[-1]=='.' or a[-1]=='@':
            print('NO')
        elif a.count('@.')!=0 or a.count('.@')!=0:
            print('NO')
        else:
            b=a.find('@')
            c=a.find('.',b+1)
            if c==-1:
                print('NO')
            else:
                print('YES')
except EOFError:
    pass
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309181213874](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309181213874.png)



### M02039: 反反复复

implementation, http://cs101.openjudge.cn/practice/02039/



思路：题目给出明文到密文的过程 答案就是构建矩阵倒过来实现



代码：

```python
c=int(input())
secret=input()
r=len(secret)//c
matrix=[['' for _ in range(c)] for _ in range(r)]
index=0
for row in range(r):
    if row%2==0:
        for column in range(c):
            matrix[row][column]=secret[index]
            index+=1
    else:
        for column in range(c-1,-1,-1):
            matrix[row][column]=secret[index]
            index+=1
original=''
for column in range(c):
    for row in range(r):
        original+=matrix[row][column]
print(original)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309190443384](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309190443384.png)



### M02092: Grandpa is Famous

implementation, http://cs101.openjudge.cn/practice/02092/



思路：用defaultdict或者创建[0]*10001来计数 学习了Counter()和dict.values()、dict.keys()、dict.items()



代码：

```python
while True:
    n,m=map(int,input().split())
    if n==0 and m==0:
        break
    count=[0]*10001
    for _ in range(n):
        for num in map(int,input().split()):
            count[num]+=1
    max_count=max(count)
    second=max(x for x in count if x!=max_count)
    for num,c in enumerate(count):
        if c==second:
            print(num,end=' ')
    print()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309195406378](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309195406378.png)



### M04133: 垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/



思路：闫老师强调了好多遍的range里面加max、min，再看一遍还是很惊艳。



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

![image-20250309200426429](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309200426429.png)



### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/



思路：既有模板又有变化的dfs



代码：

```python
move = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]
def dfs(x, y, step, p, q, visited, ans):
    if step == p * q:
        return True
    for i in range(8):
        dx, dy = x + move[i][0], y + move[i][1]
        if 1 <= dx <= q and 1 <= dy <= p and not visited[dx][dy]:
            visited[dx][dy] = True
            ans[step] = chr(dx + 64) + str(dy)
            if dfs(dx, dy, step + 1, p, q, visited, ans):
                return True
            visited[dx][dy] = False
    return False
n = int(input())
for m in range(1, n + 1):
    p, q = map(int, input().split())
    ans = ["" for _ in range(p * q)]
    visited = [[False] * (p + 1) for _ in range(q + 1)]
    visited[1][1] = True
    ans[0] = "A1"
    if dfs(1, 1, 1, p, q, visited, ans):
        result = "".join(ans)
    else:
        result = "impossible"
    print(f"Scenario #{m}:")
    print(result)
    print()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309202452065](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309202452065.png)



### T06648: Sequence

heap, http://cs101.openjudge.cn/practice/06648/



思路：用heapq降低时间复杂度 还有heapq.nsmallest()这种好东西 还是学的太少了 用时方恨少（）



代码：

```python
import heapq
t=int(input())
for _ in range(t):
    m,n=map(int,input().split())
    seq1=sorted(map(int,input().split()))
    for _ in range(m-1):
        seq2=sorted(map(int,input().split()))
        min_heap=[(seq1[i]+seq2[0],i,0) for i in range(n)]
        heapq.heapify(min_heap)
        result=[]
        for _ in range(n):
            current_sum,i,j=heapq.heappop(min_heap)
            result.append(current_sum)
            if j+1<len(seq2):
                heapq.heappush(min_heap,(seq1[i]+seq2[j+1],i,j+1))
        seq1=result
    print(*seq1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250309204125664](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250309204125664.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

这周花的时间不多，不能再这样了，不然又要完蛋了！









