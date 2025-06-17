# Assignment #1: 自主学习

Updated 0110 GMT+8 Sep 10, 2024

2024 fall, Complied by ==胡婧瑶 工学院==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/



思路：按照题目提示先考虑特殊情况，之后普通年份就按是否是四的倍数来讨论。



##### 代码

```python
 a = int(input())
if (a%3200 == 0) or ((a%100 == 0) and (a%400!= 0)):
    print('N')
elif a%4 == 0:
    print('Y')
else:
    print('N')

```



代码运行截图 ==（至少包含有"Accepted"）==



![image-20241005095813883](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005095813883.png)

5min

### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/



思路：至少就按全是兔来算，至多就按全是鸡来算。若不是4的倍数就另外讨论，若是奇数就不可能有。



##### 代码

```python
a = int(input())
if a%4 == 0:
    print(int(a/4),int(a/2))
elif a%2 == 0:
    print(int((a+2)/4),int(a/2))
else:
    print(0,0)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241005100621395](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005100621395.png)

5min

### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A



思路：因为牌是两格的，所以其实只要是偶数的就能完全塞下，如果是奇数的就空出一格。



##### 代码

```python
M,N =[int(x) for x in input().split()]
print(int(M*N/2))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==



![image-20241005101416316](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005101416316.png)

5min

### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A



思路：运用ceil()函数向上取整。



##### 代码

```python
n,m,a=map(int,input().split())
import math
s1=n/a
s2=m/a
s3=math.ceil(s1)
s4=math.ceil(s2)
print(int(s3*s4))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005102030634](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005102030634.png)

8min(在学ceil函数)

### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A



思路：先全部转化成小写字母，然后直接字符串比大小。



##### 代码

```python
s1=input().lower()
s2=input().lower()
if s1>s2:
    print("1")
elif s1<s2:
    print("-1")
else:
    print("0")

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005102652645](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005102652645.png)

5min

### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A



思路：依次输入，数1的个数即可。



##### 代码

```python
n=int(input())
count=0
for i in range(n):
    s=input()
    if s.count('1')>1:
        count+=1
print(count)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241005103141862](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241005103141862.png)

10min

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

完全零基础，对操作电脑也一无所知。其实说实话最开始选上闫老师的课是很高兴的，但是对计概本身好像一直有点抗拒，或许是本性对挑战未知走出舒适区不是那么勇敢吧(笑)。当然非常非常幸福的是有这么多同学在大群里互帮互助，上机课鼓起勇气问了一个自认为很基础的问题，发现助教真的是在很温柔很耐心地讲解。刚开始做的几个题很吃力，面对一些挺容易的数学思维脑子好像也转不起来，会带着挫败感去答案里学习。再后来借助了通义补充了很多语法函数的知识（感觉边做边学真的比强迫自己死学语法快乐好多~），第一次自己写代码一遍过AC也是收获了成就感哈哈。虽然现在还在努力赶每日选做，还说不上有很大的进步，但是希望努力了就会有回报吧。加油！



