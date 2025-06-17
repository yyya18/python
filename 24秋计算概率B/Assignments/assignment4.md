# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：除了m的限制外，要付钱了就不要了，即查到正数就停，先判断是否大于0，所以break写在前面。



代码

```python
n,m=map(int,input().split())
a = list(map(int, input().split()))
a.sort()
ans = 0
for i in range(m):
    if a[i] > 0:
        break
    ans += a[i]
print(-ans)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241021210059458](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241021210059458.png)



### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：降序排列，注意一下break的位置（跟上一道有点点不一样），这题是算完了total才去比较的。



代码

```python
n=int(input())
a=list(map(int,input().split()))
a.sort(reverse=True)
tal=0
ans=0
s=sum(a)
for i in range(n):
    tal += a[i]
    ans += 1
    if tal>s/2:
        break
print(ans)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241021212027832](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241021212027832.png)



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：学习了题解中*a,的用法，需要多次访问时可以将a转化为列表。



代码

```python
t = int(input())
for _ in range(t):
    n = int(input())
    *a, = map(int, input().split())
    *b, = map(int, input().split())

    min_a = min(a)
    min_b = min(b)

    ans1 = sum([min_a + i for i in b])
    ans2 = sum([min_b + i for i in a])
    print(min(ans1, ans2))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241021215518884](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241021215518884.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：自己用一堆if else写了一遍但是wa了。感觉不是很复杂但没明白问题出在哪。基本功还是不行ww。

题解让人不得不佩服，大概就是数学思维的魅力吧（虽然我理解了好一会儿）什么时候自己也能想出这样简洁的代码呢（）

（哎 ac的还是答案）



代码

```python
input()
a, b, c, d = map(input().count, ('1', '2', '3', '4'))
print(d + c + (b * 2 + max(0, a - c) + 3) // 4)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241021231140245](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241021231140245.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：之前跟着ai和群里的大佬们学了一下埃氏筛法和欧拉筛，还能记得大概，不过不是自己的研究成果，就不copy了。



代码

```python


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：真的没时间）等学得多了来还。



代码

```python


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

虽然肯定有自己的原因，但还是想吐槽：感觉最近几周真的是一直很忙又不知道在忙什么的状态。可能被一些学习之外的工作活动耗掉太多时间精力了吧，每日选做也没怎么跟进，靠着有限的做题机会顺便学习一些语法和算法，老师的讲义还是认真研究了一下。不想承认又不得不承认，计概显然是还没起飞，速速处理完手上的杂物，不想再拖欠内耗下去了，希望多多做题可以慢慢收获一些成就感。

