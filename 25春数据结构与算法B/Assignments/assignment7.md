# Assignment #7: 20250402 Mock Exam

Updated 1624 GMT+8 Apr 2, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



> **说明：**
>
> 1. **⽉考**：未参加 自己做在ac3-4 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E05344:最后的最后

http://cs101.openjudge.cn/practice/05344/



思路：约瑟夫问题 assignment5做过 当时是队列 这次用链表做了一下



代码：

```python
class Node:
    def __init__(self,value):
        self.value=value
        self.next=None
def j(n,k):
    head=Node(1)
    current=head
    for i in range(2,n+1):
        current.next=Node(i)
        current=current.next
    current.next=head

    result=[]
    current=head
    while current.next!=current:
        for _ in range(k-2):
            current=current.next
        result.append(current.next.value)
        current.next=current.next.next
        current=current.next
    return result

n,k=map(int,input().split())
print(' '.join(map(str,j(n,k))))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250405102226355](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405102226355.png)



### M02774: 木材加工

binary search, http://cs101.openjudge.cn/practice/02774/



思路：二分查找



代码：

```python
n,k=map(int,input().split())
length=[int(input()) for _ in range(n)]
if sum(length)<k:
    print(0)
    exit()
def cut(length,k):
    left, right = 1, sum(length) // k
    best=0
    while left<=right:
        mid = (left + right) // 2
        a = sum(i // mid for i in length)
        if a>=k:
            best=mid
            left=mid+1
        else:
            right=mid-1
    return best
print(cut(length,k))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250405105611149](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405105611149.png)



### M07161:森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/



思路：树的题目还是有点难以下手 感受到程序比较长但是各个部分很清晰



代码：

```python
from collections import deque
class Node:
    def __init__(self):
        self.value=None
        self.degree=0
        self.childs=[]
def build():
    node=Node()
    node.value=l.pop(0)
    node.degree=int(l.pop(0))
    return node
def Tree():
    root=build()
    q=deque([root])
    while q:
        node=q.popleft()
        for i in range(node.degree):
            child=build()
            node.childs.append(child)
            q.append(child)
    return root
def lastorder(tree):
    for child in tree.childs:
        lastorder(child)
    print(tree.value,end=' ')
n=int(input())
for _ in range(n):
    l=list(input().split())
    tree=Tree()
    lastorder(tree)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250405112107041](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405112107041.png)



### M18156:寻找离目标数最近的两数之和

two pointers, http://cs101.openjudge.cn/practice/18156/



思路：双指针



代码：

```python
t=int(input())
nums=list(map(int,input().split()))
nums.sort()
closest=float('inf')
left,right=0,len(nums)-1
while left<right:
    r=nums[left]+nums[right]
    if r==t:
        print(t)
        exit()
    if abs(r-t)<abs(closest-t):
        closest=r
    elif abs(r-t)==abs(closest-t):
        closest=min(closest,r)
    if r<t:
        left+=1
    else:
        right-=1
print(closest)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250405125300759](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405125300759.png)



### M18159:个位为 1 的质数个数

sieve, http://cs101.openjudge.cn/practice/18159/



思路：复习筛法



代码：

```python
def sieve(n):
    isprime=[True]*(n+1)
    primes=[]
    for i in range(2,n+1):
        if isprime[i]:
            primes.append(str(i))
        for p in primes:
            if i*int(p)>n:
                break
            isprime[i*int(p)]=False
            if i%int(p)==0:
                break
    return primes
t=int(input())
prime=sieve(10001)
for i in range(t):
    n=int(input())
    primes=[x for x in prime if int(x)<n and x[-1]=='1']
    print(f'Case{i+1}:')
    print(' '.join(primes) if primes else 'NULL')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250405133923435](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405133923435.png)



### M28127:北大夺冠

hash table, http://cs101.openjudge.cn/practice/28127/



思路：字典 有点麻烦 自己写不出来



代码：

```python
import sys
from collections import defaultdict
def process_icpc_results(m, submissions):
    teams = defaultdict(lambda: {'solved': set(), 'attempts': defaultdict(int), 'total_attempts': 0})
    for record in submissions:
        team_name, problem, result = record.split(',')
        team_name = team_name.strip()
        problem = problem.strip()
        result = result.strip()
        teams[team_name]['attempts'][problem] += 1
        teams[team_name]['total_attempts'] += 1
        if result == 'yes':
            teams[team_name]['solved'].add(problem)
    ranking = sorted(teams.items(),key=lambda item: (-len(item[1]['solved']), item[1]['total_attempts'], item[0]))
    for rank, (team_name, data) in enumerate(ranking[:12], start=1):
        print(rank, team_name, len(data['solved']), data['total_attempts'])
if __name__ == "__main__":
    m = int(sys.stdin.readline().strip())
    submissions = [sys.stdin.readline().strip() for _ in range(m)]
    process_icpc_results(m, submissions)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250405135237370](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250405135237370.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

月考题有不少模板题，但是状态好大概也只能ac4，有些经典题不常做还是忘了，对树也还不是很熟练。

还是先把讲义学完了，期中之后要多花时间了。









