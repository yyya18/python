# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by ==胡婧瑶 工学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：借鉴了题解，依次输入寻找1。



##### 代码

```python
for i in range(5):
    s=input().split()
    if "1" in s:
        print(abs(i-2)+abs(s.index("1")-2))
        break

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241005150009613](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005150009613.png)

10min

### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



思路：把能整除的单独讨论，题目需要一起输出的话是不是需要一个列表来装一下。



##### 代码

```python
s=int(input())
inputs=[]
for i in range(s):
    a,b=map(int,input().split())
    inputs.append((a,b))
outputs=[]
for a,b in inputs:
    if a%b==0:
        outputs.append("0")
    else:
        outputs.append(str(b-a%b))
print("\n".join(outputs))
 

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241005162720692](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005162720692.png)

15min

### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



思路：关键是创建两个变量来储存现有警察人数和untreated的数量，然后分类讨论即可。



##### 代码

```python
n=int(input())
a=list(map(int,input().split()))
cr=0
pl=0
for i in a:
    if i==-1:
        if pl==0:
            cr+=1
        else:
            pl-=1
    elif i>0:
        pl+=i
print(cr)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005171009097](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005171009097.png)

15min

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：构造列表，把取到的树标记0，数剩下的树。



##### 代码

```python
L,m=map(int,input().split())
a=[1]*(L+1)
for i in range(m):
    s,e=map(int,input().split())
    for j in range(s,e+1):
        a[j]=0
print(a.count(1))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005151200032](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005151200032.png)

10min

### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：遍历即可。



##### 代码

```python
a,b=map(int,input().split())
numbers=[]
for num in range(a,b+1):
    nums=str(num)
    sums=0
    for s in nums:
        sums+=int(s)**3
    if sums==num:
        numbers.append(num)
if numbers:
    print(" ".join(map(str,numbers)))
else:
    print("NO")


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005215824146](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005215824146.png)

10min

### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：（题目看完一下子没太懂，学习了题解的做法）



##### 代码

```python
import math
while True:
    n = int(input())
    if n == 0:
        break
    max_time = float("inf")
    for _ in range(n):
        speed, time = map(int, input().split())
        if time < 0:
            continue
        arrival_time = math.ceil((4500 / speed) * 3.6 + time)
        max_time = min(max_time, arrival_time)

    print(max_time)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005223154146](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005223154146.png)

15min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

感觉随着题目做得稍稍多了起来，了解的知识点也越来越多了，一些特别常见的操作程序也逐渐熟练起来，好像慢慢地变成肌肉记忆，不用再花很多时间去想每一步的作用。但是还有很多题目有很大的挑战性，还有很多重要的技巧没有掌握，所以希望能再多花点时间，缩小和大佬们之间的差距吧。

