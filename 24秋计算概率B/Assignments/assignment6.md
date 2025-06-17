# Assignment #6: Recursion and DP

Updated 2201 GMT+8 Oct 29, 2024

2024 fall, Complied by <mark>胡婧瑶 工学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  

思路：主要是想到递归的方法吧 n个圆盘的时候就是把上面的n-1个看成整体 单独一个移动 最后只要换一下柱子就可以了。



代码：

```python
def move_one(num,a,c):
    print(f'{a}->{c}')
def move(nums:int,a,b,c):
    if nums==1:
        move_one(1,a,c)
    else:
        move(nums-1,a,c,b)
        move_one(nums,a,c)
        move(nums-1,b,a,c)
def count(n):
    if n==1:
        return n
    else:
        return count(n-1)*2+1

n=int(input())
print(count(n))
move(n,'A','B','C')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241102114110799](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241102114110799.png)



### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132

思路：刚开始能想出一个递归的思路 就是n个数的时候用原先排过的n-1数 再把去除的第n个数放到每个序列前 但是用代码实现还是失败了 遂看题解 发现思路不太一样 但也学到了很多 比如浅拷贝的重要性 创建True和False的列表实现状态变化记录 列表元素逐行输出等 很多感觉挺基础的东西到自己手上可能就会出问题。。（当然也试了一下偷懒的permutations）



代码：

```python
def dfs(idx,n,used,temp,result):
    if idx==n+1:
        result.append(temp[:])
        return
    for i in range(1,n+1):
        if not used[i]:
            temp.append(i)
            used[i]=True
            dfs(idx+1,n,used,temp,result)
            used[i]=False
            temp.pop()
n=int(input())
result=[]
used=[False]*(n+1)
temp=[]
dfs(1,n,used,temp,result)
for i in result:
    print(' '.join(map(str,i)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241103090010661](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241103090010661.png)

![image-20241103085902644](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241103085902644.png)



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945

思路：自己做不出来 从群里学到了Dilworth定理 但应用还是好难 看懂题解也花了一个多小时 真是巧妙啊



代码：

```python
def max_intercepted(k,heights):
    dp=[1]*k
    for i in range(1,k):
        for j in range(i):
            if heights[i]<=heights[j]:
                dp[i]=max(dp[i],dp[j]+1)
    return max(dp)
k = int(input())
heights = list(map(int, input().split()))
print(max_intercepted(k, heights))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241103160207324](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241103160207324.png)



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421

思路：题解好多方法都努力看了 挣扎了好久 借助了ai也算是看懂了 但是对每种方法用的递归还是迭代还是dp会搞不太懂 可能自己写的时候就不会了（）



代码：

```python
n,b=map(int,input().split())
price=[0]+[int(x) for x in input().split()]
weight=[0]+[int(x) for x in input().split()]
dp=[[0]*(b+1) for i in range(n+1)]
for i in range(1,n+1):
    for j in range(1,b+1):
        if weight[i]<=j:
            dp[i][j]=max(price[i]+dp[i-1][j-weight[i]],dp[i-1][j])
        else:
            dp[i][j]=dp[i-1][j]
print(dp[-1][-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241102120817212](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241102120817212.png)



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754

思路：参考了全排列代码 独立思考实现AC！（真呀真高兴~）诶呀好吧其实真的很像的两道题 读完题就想到了多出来的对角线可以用序号和差来处理 再扔两个列表就好了 虽然还是经历了半小时的debug和一次wa（对索引和换行输入不够敏感） 但是能不看题解不看群聊自己敲出来还是挺开心的 蒽 菜菜的快乐就是这样 任重而道远 但还有希望



代码：

```python
def dfs(idx,used,temp,result,added,minus):
    if idx==9:
        result.append(temp[:])
        return
    for i in range(1,9):
        if not used[i]:
            if len(temp)+1+i not in added and len(temp)+1-i not in minus:
                used[i]=True
                temp.append(i)
                added.append(len(temp)+i)
                minus.append(len(temp)-i)
                dfs(idx+1,used,temp,result,added,minus)
                used[i]=False
                temp.pop()
                added.pop()
                minus.pop()

result=[]
used=[False]*9
temp=[]
added=[]
minus=[]
dfs(1,used,temp,result,added,minus)
result.sort()
n=int(input())
b=[int(input()) for _ in range(n)]
for i in b:
    print(''.join(map(str,result[i-1])))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241103231951670](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241103231951670.png)



### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A

思路：（来不及了先搁着 有种执念要今天交上作业 明天一定会做的



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

依然被dp dfs 递归弄得焦头烂额 但又好像有点头绪 有的时候灵光一现 但多数时候还是被难倒了 真的耗时 也真的能学到很多 不过我的脑袋什么时候能灵活点哇 经常需要把代码扔给pythontutor可视化就是说 还有好多小错误 第一遍写出来的总是不够严谨 平时大多数能改出来 不知道考试会怎么样 月考怕怕的（）



