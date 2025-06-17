# Assignment #4: 位操作、栈、链表、堆和NN

Updated 1203 GMT+8 Mar 10, 2025

2025 spring, Complied by <mark>胡婧瑶 工学院</mark>



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### 136.只出现一次的数字

bit manipulation, https://leetcode.cn/problems/single-number/



<mark>请用位操作来实现，并且只使用常量额外空间。</mark>



代码：

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        result=0
        for num in nums:
            result^=num
        return result
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250311193323759](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250311193323759.png)



### 20140:今日化学论文

stack, http://cs101.openjudge.cn/practice/20140/



思路：利用栈后进先出的性质 以及数字可能不是一位数 需要用字符串来储存



代码：

```python
s=input()
stack=[]
for i in range(len(s)):
    stack.append(s[i])
    if stack[-1]==']':
        stack.pop()
        helpstack=[]
        while stack[-1]!='[':
            helpstack.append(stack.pop())
        stack.pop()
        num=''
        while helpstack[-1] in ('0123456789'):
            num+=str(helpstack.pop())
        helpstack*=int(num)
        while helpstack!=[]:
            stack.append(helpstack.pop())
print(*stack,sep='')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250316104243078](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250316104243078.png)



### 160.相交链表

linked list, https://leetcode.cn/problems/intersection-of-two-linked-lists/



思路：双指针 当指针到达链表末尾后就指向另一个链表的开头，确保不会出现无限循环。



代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if not headA or not headB:
            return None
        pA,pB=headA,headB
        while pA!=pB:
            pA=pA.next if pA else headB
            pB=pB.next if pB else headA
        return pA
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250311205121015](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250311205121015.png)



### 206.反转链表

linked list, https://leetcode.cn/problems/reverse-linked-list/



思路：用pre current next_node三个指针实现反转链表



代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre=None
        current=head
        while current:
            next_node=current.next
            current.next=pre
            pre=current
            current=next_node
        return pre
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250311210142737](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250311210142737.png)



### 3478.选出和最大的K个元素

heap, https://leetcode.cn/problems/choose-k-elements-with-maximum-sum/



思路：学习了zip() 可以将多个可迭代对象按元素配对



代码：

```python
class Solution:
    def findMaxSum(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        combined=defaultdict(list)
        for num1,num2 in zip(nums1,nums2):
            combined[num1].append(num2)
        heap,cur,res=[],0,{}
        for num1 in sorted(combined.keys()):
            res[num1]=cur
            for num2 in combined[num1]:
                heapq.heappush(heap,num2)
                cur+=num2
                if len(heap)>k:
                    cur-=heapq.heappop(heap)
        return list(map(lambda x:res[x],nums1))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250316101104509](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250316101104509.png)



### Q6.交互可视化neural network

https://developers.google.com/machine-learning/crash-course/neural-networks/interactive-exercises

**Your task:** configure a neural network that can separate the orange dots from the blue dots in the diagram, achieving a loss of less than 0.2 on both the training and test data.

**Instructions:**

In the interactive widget:

1. Modify the neural network hyperparameters by experimenting with some of the following config settings:
   - Add or remove hidden layers by clicking the **+** and **-** buttons to the left of the **HIDDEN LAYERS** heading in the network diagram.
   - Add or remove neurons from a hidden layer by clicking the **+** and **-** buttons above a hidden-layer column.
   - Change the learning rate by choosing a new value from the **Learning rate** drop-down above the diagram.
   - Change the activation function by choosing a new value from the **Activation** drop-down above the diagram.
2. Click the Play button above the diagram to train the neural network model using the specified parameters.
3. Observe the visualization of the model fitting the data as training progresses, as well as the **Test loss** and **Training loss** values in the **Output** section.
4. If the model does not achieve loss below 0.2 on the test and training data, click reset, and repeat steps 1–3 with a different set of configuration settings. Repeat this process until you achieve the preferred results.

给出满足约束条件的<mark>截图</mark>，并说明学习到的概念和原理。

![image-20250316113034661](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250316113034661.png)

不同的函数结果差别挺大的 对神经网络的理解有一点入门的感觉

## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

努力跟进吧 这学期硬课不是特别多但是感觉事情还是好多呀







