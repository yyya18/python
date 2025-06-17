# Assignment #6: 回溯、树、双向链表和哈希表

Updated 1526 GMT+8 Mar 22, 2025

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

### LC46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：递归+回溯 感觉基于交换的回溯碰到比较少



代码：

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def backtrack(first=0):
            if first==n:
                res.append(nums[:])
            for i in range(first,n):
                nums[first],nums[i]=nums[i],nums[first]
                backtrack(first+1)
                nums[first],nums[i]=nums[i],nums[first]
        n=len(nums)
        res=[]
        backtrack()
        return res
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250328230702419](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250328230702419.png)



### LC79: 单词搜索

backtracking, https://leetcode.cn/problems/word-search/

思路：dfs+回溯



代码：

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        def check(i: int, j: int, k: int) -> bool:
            if board[i][j] != word[k]:
                return False
            if k == len(word) - 1:
                return True
            visited.add((i, j))
            result = False
            for di, dj in directions:
                newi, newj = i + di, j + dj
                if 0 <= newi < len(board) and 0 <= newj < len(board[0]):
                    if (newi, newj) not in visited:
                        if check(newi, newj, k + 1):
                            result = True
                            break
            visited.remove((i, j))
            return result
        h, w = len(board), len(board[0])
        visited = set()
        for i in range(h):
            for j in range(w):
                if check(i, j, 0):
                    return True
        return False
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250329174238925](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250329174238925.png)



### LC94.二叉树的中序遍历

dfs, https://leetcode.cn/problems/binary-tree-inorder-traversal/

思路：递归或迭代



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(node):
            if not node:
                return
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        dfs(root)
        return res
    

class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res=[]
        stack=[]
        current=root
        while current or stack:
            while current:
                stack.append(current)
                current=current.left
            current=stack.pop()
            res.append(current.val)
            current=current.right
        return res
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250329230145259](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250329230145259.png)

![image-20250330100944040](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250330100944040.png)

### LC102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/

思路：bfs



代码：

```python
from collections import deque
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        result=[]
        queue=deque([root])
        while queue:
            level_size=len(queue)
            current_level=[]
            for _ in range(level_size):
                node=queue.popleft()
                current_level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            result.append(current_level)
        return result
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250330104127404](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250330104127404.png)



### LC131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：dfs+回溯+dp预判断回文子串



代码：

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        n=len(s)
        f=[[True]*n for _ in range(n)]
        for i in range(n-1,-1,-1):
            for j in range(i+1,n):
                f[i][j]=(s[i]==s[j] and f[i+1][j-1])
        ret=list()
        ans=list()
        def dfs(i):
            if i==n:
                ret.append(ans[:])
                return 
            for j in range(i,n):
                if f[i][j]:
                    ans.append(s[i:j+1])
                    dfs(j+1)
                    ans.pop()
        dfs(0)
        return ret
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250330105915674](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250330105915674.png)



### LC146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：看着题解学的，哈希表+双端队列，理解了lru的原理



代码：

```python
class DLinkedNode:
    def __init__(self,key=0,value=0):
        self.key=key
        self.value=value
        self.prev=None
        self.next=None

class LRUCache:

    def __init__(self, capacity: int):
        self.cache=dict()
        self.head=DLinkedNode()
        self.tail=DLinkedNode()
        self.head.next=self.tail
        self.tail.prev=self.head
        self.capacity=capacity
        self.size=0

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node=self.cache[key]
        self.moveToHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            node=DLinkedNode(key,value)
            self.cache[key]=node
            self.addToHead(node)
            self.size+=1
            if self.size>self.capacity:
                removed=self.removeTail()
                self.cache.pop(removed.key)
                self.size-=1
        else:
            node=self.cache[key]
            node.value=value
            self.moveToHead(node)
    def addToHead(self,node):
        node.prev=self.head
        node.next=self.head.next
        self.head.next.prev=node
        self.head.next=node
    def removeNode(self,node):
        node.prev.next=node.next
        node.next.prev=node.prev
    def moveToHead(self,node):
        self.removeNode(node)
        self.addToHead(node)
    def removeTail(self):
        node=self.tail.prev
        self.removeNode(node)
        return node
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250330114242964](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250330114242964.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

看了一半树的讲义 希望能多做点题









