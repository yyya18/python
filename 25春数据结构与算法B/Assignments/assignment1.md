# Assignment #1: 虚拟机，Shell & 大语言模型

Updated 2309 GMT+8 Feb 20, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



**作业的各项评分细则及对应的得分**

| 标准                                 | 等级                                                         | 得分 |
| ------------------------------------ | ------------------------------------------------------------ | ---- |
| 按时提交                             | 完全按时提交：1分<br/>提交有请假说明：0.5分<br/>未提交：0分  | 1 分 |
| 源码、耗时（可选）、解题思路（可选） | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于2个：0分 | 1 分 |
| AC代码截图                           | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于：0分 | 1 分 |
| 清晰头像、PDF文件、MD/DOC附件        | 包含清晰的Canvas头像、PDF文件以及MD或DOC格式的附件：1分<br/>缺少上述三项中的任意一项：0.5分<br/>缺失两项或以上：0分 | 1 分 |
| 学习总结和个人收获                   | 提交了学习总结和个人收获：1分<br/>未提交学习总结或内容不详：0分 | 1 分 |
| 总得分： 5                           | 总分满分：5分                                                |      |
>
> 
>
> **说明：**
>
> 1. **解题与记录：**
>       - 对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>    
>2. **课程平台与提交安排：**
> 
>   - 我们的课程网站位于Canvas平台（https://pku.instructure.com ）。该平台将在第2周选课结束后正式启用。在平台启用前，请先完成作业并将作业妥善保存。待Canvas平台激活后，再上传你的作业。
> 
>       - 提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>3. **延迟提交：**
> 
>   - 如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
> 
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/

思路：照着讲义学的OOP

代码：

```python
def gcd(m,n):
    while m%n!=0:
        oldm=m
        oldn=n
        m=oldn
        n=oldm%oldn
    return n
class Fraction:
    def __init__(self,top,bottom):
        self.num=top
        self.den=bottom
    def __str__(self):
        return str(self.num)+"/"+str(self.den)
    def show(self):
        print(self.num,"/",self.den)
    def __add__(self,otherfraction):
        newnum=self.num*otherfraction.den+self.den*otherfraction.num
        newden=self.den*otherfraction.den
        common=gcd(newnum,newden)
        return Fraction(newnum//common,newden//common)
a,b,c,d=map(int,input().split())
x=Fraction(a,b)
y=Fraction(c,d)
print(x+y)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250301191918368](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250301191918368.png)



### 1760.袋子里最少数目的球

 https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/




思路：一开始想的不是二分 思路是正着平均拆分最大数 题解的二分法有一点逆向思维的意思 先选定一个数为最大值的最小值 再用maxOperations来判断指针变化

代码：

```python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        left,right,ans=1,max(nums),0
        while left<=right:
            y=(left+right)//2
            ops=sum((x-1)//y for x in nums)
            if ops<=maxOperations:
                ans=y
                right=y-1
            else:
                left=y+1
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250301202247164](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250301202247164.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135



思路：二分法降低时间复杂度

上学期做的确实有点忘了 计概还得复习

代码：

```python
n,m = map(int, input().split())
expenditure = []
for _ in range(n):
    expenditure.append(int(input()))

def check(x):
    num, s = 1, 0
    for i in range(n):
        if s + expenditure[i] > x:
            s = expenditure[i]
            num += 1
        else:
            s += expenditure[i]
    
    return [False, True][num > m]


lo = max(expenditure)
hi = sum(expenditure) + 1
ans = 1
while lo < hi:
    mid = (lo + hi) // 2
    if check(mid):   
        lo = mid + 1  
    else:
        ans = mid   
        hi = mid
        
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250225171953698](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250225171953698.png)





### 27300: 模型整理

http://cs101.openjudge.cn/practice/27300/



思路：用到了defaultdict  不同单位转化成同一个再比较



代码：

```python
from collections import defaultdict

n = int(input())
d = defaultdict(list)
for _ in range(n):
    name, para = input().split('-')
    if para[-1]=='M':
        d[name].append((para, float(para[:-1])/1000) )
    else:
        d[name].append((para, float(para[:-1])))
sd = sorted(d)
for k in sd:
    paras = sorted(d[k],key=lambda x: x[1])
    value = ', '.join([i[0] for i in paras])
    print(f'{k}: {value}')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250301192214725](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250301192214725.png)



### Q5. 大语言模型（LLM）部署与测试

本任务旨在本地环境或通过云虚拟机（如 https://clab.pku.edu.cn/ 提供的资源）部署大语言模型（LLM）并进行测试。用户界面方面，可以选择使用图形界面工具如 https://lmstudio.ai 或命令行界面如 https://www.ollama.com 来完成部署工作。

测试内容包括选择若干编程题目，确保这些题目能够在所部署的LLM上得到正确解答，并通过所有相关的测试用例（即状态为Accepted）。选题应来源于在线判题平台，例如 OpenJudge、Codeforces、LeetCode 或洛谷等，同时需注意避免与已找到的AI接受题目重复。已有的AI接受题目列表可参考以下链接：
https://github.com/GMyhf/2025spring-cs201/blob/main/AI_accepted_locally.md

请提供你的最新进展情况，包括任何关键步骤的截图以及遇到的问题和解决方案。这将有助于全面了解项目的推进状态，并为进一步的工作提供参考。





### Q6. 阅读《Build a Large Language Model (From Scratch)》第一章

作者：Sebastian Raschka

请整理你的学习笔记。这应该包括但不限于对第一章核心概念的理解、重要术语的解释、你认为特别有趣或具有挑战性的内容，以及任何你可能有的疑问或反思。通过这种方式，不仅能巩固你自己的学习成果，也能帮助他人更好地理解这一部分内容。

看英文书还是有点累 本身兴趣不是特别大 但当作拓展视野了 也会接触到一些有趣的知识



## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

寒假的课件没有全部学完 有空争取再刷一点 以及上学期题目练少了 这学期吸取教训
