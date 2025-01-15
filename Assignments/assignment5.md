# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：没有多思考，直接让s一个一个试过来

​        看了题解显然第一种更清晰更好理解，就是想法简单地一个一个数去尝试。第二种用周期跳跃地找数，其中将p,e,i重置为d后第一个周期内的数似乎有更简洁的表达式：p=(p-d)%23+d，其实大差不差，主要是题解的写法我理解起来没这么快（笑

代码：

```python
num=1
while True:
    p,e,i,d=map(int,input().split())
    if p==-1:
        break
    p%=23
    e%=28
    i%=33
    s=d+1
    while (s-p)%23!=0 or (s-e)%28!=0 or (s-i)%33!=0:
        s+=1
    s-=d
    print('Case %d: the next triple peak occurs in %d days.'%(num,s))
    num+=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241023104856131](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241023104856131.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：感受到了双指针的便捷，买便宜的，卖贵的，还有就是对最后一件买不买的处理。



代码：

```python
p=int(input())
c=sorted(list(map(int,input().split())))
i=0
j=len(c)-1
cnt=0
while i<j:
    if c[i]<=p:
        p-=c[i]
        i += 1
        cnt+=1
    elif cnt:
        p+=c[j]
        j -= 1
        cnt-=1
    else:
        break
print(cnt+(c[i]<=p))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241026152956150](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241026152956150.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：计算最短平均时间比较简单，将列表元素升序排列求反序和即可。输出顺序主要是用sorted给索引排序，然后再加一。第一次RE了，发现是sorted每一次都要算range(len(a))，把它单独令成一个变量就好了。



代码：

```python
n=int(input())
a=list(map(int,input().split()))
b=sorted(a)
g=range(len(a))
d=sorted(g,key=lambda i:a[i])
e=[str(i+1) for i in d]
f=' '.join(e)
ans=0
for i in range(len(b)):
    c=n-1-i
    ans+=b[i]*c
print(f)
print('{:.2f}'.format(ans/n))
```

![image-20241026163141968](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241026163141968.png)

代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：整体感觉比前几题好想，但是确实有几个坑点是会让一次性AC有点困难。



代码：

```python
Haab = {"pop": 0, "no": 1, "zip": 2, "zotz": 3, "tzec": 4, "xul": 5, "yoxkin": 6, "mol": 7, "chen": 8, "yax": 9, "zac": 10, "ceh": 11, "mac": 12, "kankin": 13, "muan": 14, "pax": 15, "koyab": 16, "cumhu": 17, "uayet": 18}
Tzolkin = {0: "imix", 1: "ik", 2: "akbal", 3: "kan", 4: "chicchan", 5: "cimi", 6: "manik", 7: "lamat", 8: "muluk", 9: "ok", 10: "chuen", 11: "eb", 12: "ben", 13: "ix", 14: "mem", 15: "cib", 16: "caban", 17: "eznab", 18: "canac", 19: "ahau"}
n = int(input())
print(n)
for _ in range(n):
    day, month, year = input().split()
    day = int(day.strip("."))
    total = int(year) * 365 + Haab[month] * 20 + day
    m=total%13+1
    d=Tzolkin[total%20]
    y=total//260
    print(m,d,y)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241026192837824](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241026192837824.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：第一步还是得把题目理解对啊 读完开始做的时候一直以为要考虑一棵树砍倒后覆盖其他树的情况（这是真的当成点在做了...）

所以本来很简单的问题硬是控了我好久...后面还因为缩进错了一次 感觉小问题好多 就会对AC造成致命伤害。

代码：

```python
n=int(input())
a=[[int(x) for x in map(int,input().split())] for i in range(n)]
if n==1:
    print(1)
else:
    s=2
    for i in range(1,n-1):
        if a[i][0]-a[i-1][0]>a[i][1]:
            s+=1
        elif a[i+1][0]-a[i][0]>a[i][1]:
            s+=1
            a[i][0]+=a[i][1]
    print(s)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241026220836689](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241026220836689.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：难难的。自己敲了几段碎片以后就是一个看题解学习的过程。（看完觉得一段AC的代码像一个好伟大的工程，真的是投入好多的（对我这种菜菜来说



啊啊啊卧槽终于懂了啊啊啊啊中间最难理解的有数学思维的那段啊啊啊啊

真的是要思考的就是说 真的是要在草稿纸上画画的啊啊啊

首先有一个贪心的基本思路是尽可能地让雷达往后安装来满足更多后面的需求！

首先是你已经sort()了就可以保证后面的start一定在前面的之后 然后就去考虑它的end 最难理解的那个min就是因为如果end<last_covered就相当于是后面一个区间包含在前面一个里 那就必须让雷达往前去满足区间较小的！



代码：

```python
import math

def solve(n, d, islands):
    if d < 0:
        return -1

    ranges = []
    for x, y in islands:
        if y > d:
            return -1
        delta = math.sqrt(d * d - y * y)
        ranges.append((x - delta, x + delta))

    ranges.sort(key=lambda r: r[0])

    cnt = 0
    last_covered = float('-inf')
    for start, end in ranges:
        if start > last_covered:
            cnt += 1
            last_covered = end
        else:
            last_covered = min(last_covered, end)

    return cnt

case_number = 1
while True:
    n, d = map(int, input().split())
    if n == 0 and d == 0:
        break

    islands = []
    for _ in range(n):
        islands.append(tuple(map(int, input().split())))

    result = solve(n, d, islands)
    print(f'Case {case_number}: {result}')
    case_number += 1
    input()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241026230448343](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241026230448343.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

没有如果，作业题一点都不简单（哭）。好吧是自己前面摆了，每日选做没跟上，作业有几题还是挺吃力的。期中周附近真的是几门硬课互相抢时间（头痛ing），但是这周花在计概上的时间比之前已经明显多了，再挤挤吧时间总是有的。继续研究老师的讲义，知识点好多，但是感觉光看效果不佳，还是得配套做题会加深印象，啃讲义有点累，但也是从之前连几种算法的区别都分不清楚到能够大概理清各自的基本套路了。该会的还没完全掌握，又要迎接新的挑战了，硬着头皮上了，但愿勤能补拙，做题的时候清醒一点plz.



