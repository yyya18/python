# Assignment #5: 链表、栈、队列和归并排序

Updated 1348 GMT+8 Mar 17, 2025

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

### LC21.合并两个有序链表

linked list, https://leetcode.cn/problems/merge-two-sorted-lists/

思路：复习了归并排序

代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val<l2.val:
            l1.next=self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next=self.mergeTwoLists(l1,l2.next)
            return l2
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323104101839](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323104101839.png)



### LC234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现。</mark>



代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None:
            return True
        first_half_end=self.end_of_first_half(head)
        second_half_start=self.reverse_list(first_half_end.next)
        result=True
        first_position=head
        second_position=second_half_start
        while result and second_position is not None:
            if first_position.val!=second_position.val:
                result=False
            first_position=first_position.next
            second_position=second_position.next
        first_half_end.next=self.reverse_list(second_half_start)
        return result
    def end_of_first_half(self,head):
        fast=head
        slow=head
        while fast.next is not None and fast.next.next is not None:
            fast=fast.next.next
            slow=slow.next
        return slow
    def reverse_list(self,head):
        previous=None
        current=head
        while current is not None:
            next_node=current.next
            current.next=previous
            previous=current
            current=next_node
        return previous
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323111415886](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323111415886.png)



### LC1472.设计浏览器历史记录

doubly-lined list, https://leetcode.cn/problems/design-browser-history/

<mark>请用双链表实现。</mark>



代码：

```python
class ListNode:
    def __init__(self,val='',prev=None,next=None):
        self.val=val
        self.prev=prev
        self.next=next

class BrowserHistory:

    def __init__(self, homepage: str):
        self.curr=ListNode(homepage)

    def visit(self, url: str) -> None:
        new_node=ListNode(url,self.curr)
        self.curr.next=new_node
        self.curr=new_node

    def back(self, steps: int) -> str:
        while steps>0 and self.curr.prev:
            self.curr=self.curr.prev
            steps-=1
        return self.curr.val

    def forward(self, steps: int) -> str:
        while steps>0 and self.curr.next:
            self.curr=self.curr.next
            steps-=1
        return self.curr.val
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323142004175](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323142004175.png)

### 24591: 中序表达式转后序表达式

stack, http://cs101.openjudge.cn/practice/24591/

思路：学习了调度场算法



代码：

```python
def infix_to_postfix(expression):
    precedence={'+':1,'-':1,'*':2,'/':2}
    stack=[]
    postfix=[]
    number=''
    for char in expression:
        if char.isnumeric() or char=='.':
            number+=char
        else:
            if number:
                num=float(number)
                postfix.append(int(num) if num.is_integer() else num)
                number=''
            if char in '+-*/':
                while stack and stack[-1] in '+-*/' and precedence[char]<=precedence[stack[-1]]:
                    postfix.append(stack.pop())
                stack.append(char)
            elif char=='(':
                stack.append(char)
            elif char==')':
                while stack and stack[-1]!='(':
                    postfix.append(stack.pop())
                stack.pop()
    if number:
        num=float(number)
        postfix.append(int(num) if num.is_integer() else num)
    while stack:
        postfix.append(stack.pop())
    return ' '.join(str(x) for x in postfix)
n=int(input())
for _ in range(n):
    expression=input()
    print(infix_to_postfix(expression))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323152020696](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323152020696.png)



### 03253: 约瑟夫问题No.2

queue, http://cs101.openjudge.cn/practice/03253/

<mark>请用队列实现。</mark>



代码：

```python
from collections import deque
while True:
    n,p,m=map(int,input().split())
    if n==p==m==0:
        break
    queue=deque([i for i in range(p,n+1)]+[i for i in range(1,p)])
    out=[]
    while queue:
        for _ in range(m-1):
            queue.append(queue.popleft())
        out.append(queue.popleft())
    print(*out,sep=',')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323154128897](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323154128897.png)



### 20018: 蚂蚁王国的越野跑

merge sort, http://cs101.openjudge.cn/practice/20018/

思路：用归并排序 记录移动的次数



代码：

```python
def merge_sort(l):
    if len(l) <= 1:
        return l, 0
    mid = len(l) // 2
    left, left_count = merge_sort(l[:mid])
    right, right_count = merge_sort(l[mid:])
    l, merge_count = merge(left, right)
    return l, left_count + right_count + merge_count
def merge(left, right):
    merged = []
    left_index, right_index = 0, 0
    count = 0
    while left_index < len(left) and right_index < len(right):
        if left[left_index] >= right[right_index]:
            merged.append(left[left_index])
            left_index += 1
        else:
            merged.append(right[right_index])
            right_index += 1
            count += len(left) - left_index
    merged += left[left_index:]+right[right_index:]
    return merged, count
n = int(input())
l = []
for i in range(n):
    l.append(int(input()))
l, ans = merge_sort(l)
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250323161458679](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250323161458679.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

复习了几种排序









