# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024

2024 fall, Complied by Hongfei Yan==胡婧瑶 工学院==



**说明：**

1）Oct⽉考： AC4 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



思路：考试的时候在(ord(i)-ord('a')-k)%26+ord('a')上卡了，感觉数学脑袋还不太行。（但是为什么输出一直跟输入一样啊，明明挺简单的题卡了好久。发发牢骚bhys)



代码

```python
def dect(k,s):
    der=""
    for i in s:
        if 'a'<=i<='z':
            der+=chr((ord(i)-ord('a')-k)%26+ord('a'))
        else:
            der += chr((ord(i) - ord('A') - k) % 26 + ord('A'))
    return der

k=int(input())
s=input()
print(dect(k,s))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241011124216243](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241011124216243.png)



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：好几次一直RE，后来发现用map函数就快了。



代码

```python
s1,s2=map(str,input().split())
a=int(s1[:2])
b=int(s2[:2])
print(a+b)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241011124948113](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241011124948113.png)



### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：创建列表实现对应相乘。



代码

```python
def check(id):
    con=[7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
    com=['1','0','X','9','8','7','6','5','4','3','2']
    sums=sum(int(id[i])*con[i] for i in range(17))
    left=com[sums%11]
    return left==id[-1]
a=int(input())
for i in range(a):
    s=str(input())
    if check(s)==True:
        print('YES')
    else:
        print('NO')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241011125114679](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241011125114679.png)



### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：循环结构的使用



代码

```python
n=int(input())
if n==1:
    print('End')
else:
    while n!=1:
        if n%2==0:
            print('%d/2=%d'% (n,n/2))
            n=n/2
        else:
            print('%d*3+1=%d'% (n,n*3+1))
            n=n*3+1
    print('End')

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241011125239762](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241011125239762.png)



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



思路：考试的时候没想出来函数怎么定义，想用本办法直接写但是没来得及。感觉还是需要数学思维。但是回头自己做的时候一直RE，感觉和题解提供的代码真的大差不差了，不知道哪里有问题。



##### 代码

```python
roman_to_int_map = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}

int_to_roman_map = [(1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'),(100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'),(10, 'X'), (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')]

def roman_to_int(s):
    total = 0
    prev_value = 0
    for char in s:
        value = roman_to_int_map[char]
        if value > prev_value:
            total += value - 2 * prev_value
        else:
            total += value
        prev_value = value
    return total

def int_to_roman(num):
    result = []
    for value, symbol in int_to_roman_map:
        while num >= value:
            result.append(symbol)
            num -= value
    return ''.join(result)

def main():
    input_data = input().strip()
    if input_data.isdigit():
        num = int(input_data)
        print(int_to_roman(num))
    else:
        print(roman_to_int(input_data))

if __name__ == "__main__":
    main()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20241012224207004](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20241012224207004.png)



### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/



思路：（对我来说难度还是太高了，努力钻研一下题解，争取一段时间后可以有思路自己尝试）



代码

```python


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

目前还在努力做之前欠下的每日选做。感觉大家都好厉害啊，月考平均都能AC5吧，感觉自己投入时间还是不够，于是就产生了压力和愧疚感。目标是行动起来，跟上大部队，跟着AI学到很多知识的同时也要提醒自己避免因为科技力量助长了浅层思想的坏习惯。还有看到同学说的平时做题有意识地模拟一下考试，计个时觉得非常赞同。









