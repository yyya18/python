# Assignment #9: Huffman, BST & Heap

Updated 1834 GMT+8 Apr 15, 2025

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

### LC222.完全二叉树的节点个数

dfs, https://leetcode.cn/problems/count-complete-tree-nodes/

思路：根据完全二叉树的性质，从左右子树的高度判断最后一行节点的情况



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        def get_depth(node,go_left=True):
            depth=0
            while node:
                depth+=1
                node=node.left if go_left else node.right
            return depth
        left_depth=get_depth(root,True)
        right_depth=get_depth(root,False)
        if left_depth==right_depth:
            return (2**left_depth-1)
        else:
            return 1+self.countNodes(root.left)+self.countNodes(root.right)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419134303315](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419134303315.png)



### LC103.二叉树的锯齿形层序遍历

bfs, https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/

思路：在二叉树层序遍历的基础上添加left_to_right来标记列表是否需要翻转



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        result=[]
        queue=deque([root])
        left_to_right=True
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
            if not left_to_right:
                current_level=current_level[::-1]
            result.append(current_level)
            left_to_right=not left_to_right
        return result
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419145838676](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419145838676.png)



### M04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

思路：一个数每被合并一次权值就加一，将最小的两个数合并并将和加入队列



代码：

```python
import heapq
def min_weighted_path_length(n,weights):
    heapq.heapify(weights)
    total=0
    while len(weights)>1:
        a=heapq.heappop(weights)
        b=heapq.heappop(weights)
        combined=a+b
        total+=combined
        heapq.heappush(weights,combined)
    return total
n=int(input())
weights=list(map(int,input().split()))
print(min_weighted_path_length(n,weights))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419151127142](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419151127142.png)



### M05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

思路：建树的过程：终止条件是节点为None，也就是找到了对应的位置，然后按照BST的性质让新节点一步步递归下沉



代码：

```python
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None
def insert(node,value):
    if node is None:
        return TreeNode(value)
    if value<node.value:
        node.left=insert(node.left,value)
    elif value>node.value:
        node.right=insert(node.right,value)
    return node
def level_order_traversal(root):
    queue=[root]
    traversal=[]
    while queue:
        node=queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal
numbers=list(map(int,input().strip().split()))
numbers1=[]
for i in numbers:
    if i not in numbers1:
        numbers1.append(i)
root=None
for i in numbers1:
    root=insert(root,i)
traversal=level_order_traversal(root)
print(' '.join(map(str,traversal)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419154244047](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419154244047.png)



### M04078: 实现堆结构

手搓实现，http://cs101.openjudge.cn/practice/04078/

类似的题目是 晴问9.7: 向下调整构建大顶堆，https://sunnywhy.com/sfbj/9/7

思路：



代码：

```python
class BinaryHeap:
    def __init__(self):
        self._heap = []

    def _perc_up(self, i):
        while (i - 1) // 2 >= 0:
            parent_idx = (i - 1) // 2
            if self._heap[i] < self._heap[parent_idx]:
                self._heap[i], self._heap[parent_idx] = (
                    self._heap[parent_idx],
                    self._heap[i],
                )
            i = parent_idx

    def insert(self, item):
        self._heap.append(item)
        self._perc_up(len(self._heap) - 1)

    def _perc_down(self, i):
        while 2 * i + 1 < len(self._heap):
            sm_child = self._get_min_child(i)
            if self._heap[i] > self._heap[sm_child]:
                self._heap[i], self._heap[sm_child] = (
                    self._heap[sm_child],
                    self._heap[i],
                )
            else:
                break
            i = sm_child

    def _get_min_child(self, i):
        if 2 * i + 2 > len(self._heap) - 1:
            return 2 * i + 1
        if self._heap[2 * i + 1] < self._heap[2 * i + 2]:
            return 2 * i + 1
        return 2 * i + 2

    def delete(self):
        self._heap[0], self._heap[-1] = self._heap[-1], self._heap[0]
        result = self._heap.pop()
        self._perc_down(0)
        return result

    def heapify(self, not_a_heap):
        self._heap = not_a_heap[:]
        i = len(self._heap) // 2 - 1
        while i >= 0:
            self._perc_down(i)
            i = i - 1

n = int(input().strip())
bh = BinaryHeap()
for _ in range(n):
    inp = input().strip()
    if inp[0] == '1':
        bh.insert(int(inp.split()[1]))
    else:
        print(bh.delete())
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419161504814](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419161504814.png)



### T22161: 哈夫曼编码树

greedy, http://cs101.openjudge.cn/practice/22161/

思路：



代码：

```python
import heapq
class Node:
    def __init__(self,weight,char=None):
        self.weight=weight
        self.char=char
        self.left=None
        self.right=None
    def __lt__(self,other):
        if self.weight==other.weight:
            return self.char<other.char
        return self.weight<other.weight
def build_huffman_tree(characters):
    heap=[]
    for char,weight in characters.items():
        heapq.heappush(heap,Node(weight,char))
    while len(heap)>1:
        left=heapq.heappop(heap)
        right=heapq.heappop(heap)
        merged=Node(left.weight+right.weight,min(left.char,right.char))
        merged.left=left
        merged.right=right
        heapq.heappush(heap,merged)
    return heap[0]
def encode_huffman_tree(root):
    codes={}
    def traverse(node,code):
        if node.left is None and node.right is None:
            codes[node.char]=code
        else:
            traverse(node.left,code+'0')
            traverse(node.right,code+'1')
    traverse(root,'')
    return codes
def huffman_encoding(codes,string):
    encoded=''
    for char in string:
        encoded+=codes[char]
    return encoded
def huffman_decoding(root,encoded_string):
    decoded=''
    node=root
    for bit in encoded_string:
        if bit=='0':
            node=node.left
        else:
            node=node.right
        if node.left is None and node.right is None:
            decoded+=node.char
            node=root
    return decoded
n=int(input())
characters={}
for _ in range(n):
    char,weight=input().split()
    characters[char]=int(weight)
huffman_tree=build_huffman_tree(characters)
codes=encode_huffman_tree(huffman_tree)
strings=[]
while True:
    try:
        line=input()
        strings.append(line)
    except EOFError:
        break
results=[]
for string in strings:
    if string[0] in('0','1'):
        results.append(huffman_decoding(huffman_tree,string))
    else:
        results.append(huffman_encoding(codes,string))
for result in results:
    print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250419173018030](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250419173018030.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

期中还没考完，先把知识梳理了一遍，实战还有待提高。









