# Assignment #8: 树为主

Updated 1704 GMT+8 Apr 8, 2025

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

### LC108.将有序数组转换为二叉树

dfs, https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

思路：由有序数组想到中序遍历，选择中间位置的数作为二叉树的根节点



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def helper(left,right):
            if left>right:
                return None
            mid=(left+right)//2
            root=TreeNode(nums[mid])
            root.left=helper(left,mid-1)
            root.right=helper(mid+1,right)
            return root
        return helper(0,len(nums)-1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250410162423937](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250410162423937.png)



### M27928:遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

思路：找到根节点 递归排序输出



代码：

```python
from collections import defaultdict

n=int(input())
tree=defaultdict(list)
all_nodes=set()
child_nodes=set()
for _ in range(n):
    parts=list(map(int,input().split()))
    parent,*children=parts
    tree[parent].extend(children)
    all_nodes.add(parent)
    all_nodes.update(children)
    child_nodes.update(children)
    root=(all_nodes-child_nodes).pop()
def traverse(u):
    group=tree[u]+[u]
    group.sort()
    for x in group:
        if x==u:
            print(u)
        else:
            traverse(x)
traverse(root)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250413145949383](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250413145949383.png)



### LC129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

思路：由路径组成的数字要想到每往前一位就相当于×10再相加，在此基础上进行递归



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(root:TreeNode,prevTotal:int)->int:
            if not root:
                return 0
            total=prevTotal*10+root.val
            if not root.left and not root.right:
                return total
            else:
                return dfs(root.left,total)+dfs(root.right,total)
        return dfs(root,0)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250413152756346](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250413152756346.png)



### M22158:根据二叉树前中序序列建树

tree, http://cs101.openjudge.cn/practice/22158/

思路：根节点是前序的第一个字母，也是在后序中分隔左右子树的分界，由根节点在后序中的索引来递归找到根节点的左右节点，新的前序序列是根据中序里子树的节点个数来确定的。



代码：

```python
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None
def build_tree(preorder,inorder):
    if not preorder and not inorder:
        return None
    root_value=preorder[0]
    root=TreeNode(root_value)
    root_index_inorder=inorder.index(root_value)
    root.left=build_tree(preorder[1:1+root_index_inorder],inorder[:root_index_inorder])
    root.right=build_tree(preorder[1+root_index_inorder:],inorder[root_index_inorder+1:])
    return root
def postorder(root):
    if root is None:
        return ''
    return postorder(root.left)+postorder(root.right)+root.value
while True:
    try:
        preorder=input().strip()
        inorder=input().strip()
        root=build_tree(preorder,inorder)
        print(postorder(root))
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250413160214320](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250413160214320.png)



### M24729:括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

思路：对我来说确实是T的题 建树有困难



代码：

```python
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.children=[]
def parse_tree(s):
    stack=[]
    node=None
    for char in s:
        if char.isalpha():
            node=TreeNode(char)
            if stack:
                stack[-1].children.append(node)
        elif char=='(':
            if node:
                stack.append(node)
                node=None
        elif char==')':
            if stack:
                node=stack.pop()
    return node
def preorder(node):
    output=[node.value]
    for child in node.children:
        output.extend(preorder(child))
    return ''.join(output)
def postorder(node):
    output=[]
    for child in node.children:
        output.extend(postorder(child))
    output.append(node.value)
    return ''.join(output)
s=input().strip()
s=''.join(s.split())
root=parse_tree(s)
print(preorder(root))
print(postorder(root))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250413163045646](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250413163045646.png)



### LC3510.移除最小数对使数组有序II

doubly-linked list + heap, https://leetcode.cn/problems/minimum-pair-removal-to-sort-array-ii/

思路：只有看懂题解的水平



代码：

```python
import heapq
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        n = len(nums)
        pairs = [(nums[i], nums[i + 1]) for i in range(n - 1)]
        h = []
        dec = 0
        for i, (x, y) in enumerate(pairs):
            if x > y:
                dec += 1
            h.append((x + y, i))

        heapq.heapify(h)
        left = list(range(-1, n))
        right = list(range(1, n + 1))

        ans = 0
        while dec:
            ans += 1
            while right[h[0][1]] >= n or nums[h[0][1]] + nums[right[h[0][1]]] != h[0][0]:
                heapq.heappop(h)

            s, i = heapq.heappop(h)
            nxt = right[i]
            pre = left[i]
            nxt2 = right[nxt]

            if nums[nxt] < nums[i]:
                dec -= 1

            if pre >= 0:
                if nums[pre] > s:
                    dec += 1
                if nums[pre] > nums[i]:
                    dec -= 1
                heapq.heappush(h, (nums[pre] + s, pre))

            if nxt2 < n:
                if nums[nxt] > nums[nxt2]:
                    dec -= 1
                if nums[nxt2] < s:
                    dec += 1
                heapq.heappush(h, (s + nums[nxt2], i))

            nums[i] = s
            right[i] = nxt2
            left[nxt2] = i
            right[nxt] = n

        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250413171216158](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250413171216158.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

期中结束在数算上的时间比之前多了 目前还是以讲义和作业为主 之后估计得保持大量时间投入去做题 感觉还有很多掌握得不够的点 紧迫感很强









