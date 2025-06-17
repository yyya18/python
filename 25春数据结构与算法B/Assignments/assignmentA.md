# Assignment #A: Graph starts

Updated 1830 GMT+8 Apr 22, 2025

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

### M19943:图的拉普拉斯矩阵

OOP, implementation, http://cs101.openjudge.cn/practice/19943/

要求创建Graph, Vertex两个类，建图实现。

思路：



代码：

```python
n, m = map(int, input().split())
ans = [[0 for i in range(n)] for j in range(n)]
for i in range(m):       
    knot1, knot2 = map(int, input().split())       
    ans[knot1][knot1] += 1       
    ans[knot2][knot2] += 1       
    ans[knot1][knot2] -= 1       
    ans[knot2][knot1] -= 1
for j in range(n):       
    print(' '.join(map(str, ans[j])))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250426112504066](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250426112504066.png)



### LC78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：



代码：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        ans = []
        for mask in range(1 << n):
            t = []
            for i in range(n):
                if mask & (1 << i):
                    t.append(nums[i])
            ans.append(t)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425220307724](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250425220307724.png)



### LC17.电话号码的字母组合

hash table, backtracking, https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

思路：



代码：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return list()
        
        phoneMap = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        def backtrack(index: int):
            if index == len(digits):
                combinations.append("".join(combination))
            else:
                digit = digits[index]
                for letter in phoneMap[digit]:
                    combination.append(letter)
                    backtrack(index + 1)
                    combination.pop()

        combination = list()
        combinations = list()
        backtrack(0)
        return combinations
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425220112316](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250425220112316.png)



### M04089:电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：



代码：

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_number = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, number):
        node = self.root
        for digit in number:
            if digit not in node.children:
                node.children[digit] = TrieNode()
            node = node.children[digit]
            if node.is_end_of_number:
                return False
        node.is_end_of_number = True
        return len(node.children) == 0
    
    def is_consistent(self, numbers):
        numbers.sort(key=len)
        for number in numbers:
            if not self.insert(number):
                return False
        return True

def main():
    import sys
    input = sys.stdin.read
    data = input().splitlines()
    
    t = int(data[0])
    index = 1
    results = []
    
    for _ in range(t):
        n = int(data[index])
        index += 1
        numbers = data[index:index + n]
        index += n
        
        trie = Trie()
        if trie.is_consistent(numbers):
            results.append("YES")
        else:
            results.append("NO")
    
    print("\n".join(results))

if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250426112910704](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250426112910704.png)



### T28046:词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：



代码：

```python
from collections import defaultdict
dic=defaultdict(list)
n,lis=int(input()),[]
for i in range(n):
    lis.append(input())
for word in lis:
    for i in range(len(word)):
        bucket=word[:i]+'_'+word[i+1:]
        dic[bucket].append(word)
def bfs(start,end,dic):
    queue=[(start,[start])]
    visited=[start]
    while queue:
        currentword,currentpath=queue.pop(0)
        if currentword==end:
            return ' '.join(currentpath)
        for i in range(len(currentword)):
            bucket=currentword[:i]+'_'+currentword[i+1:]
            for nbr in dic[bucket]:
                if nbr not in visited:
                    visited.append(nbr)
                    newpath=currentpath+[nbr]
                    queue.append((nbr,newpath))
    return 'NO'
start,end=map(str,input().split())    
print(bfs(start,end,dic))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250426113109986](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250426113109986.png)



### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

思路：



代码：

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        def generateBoard():
            board = list()
            for i in range(n):
                row[queens[i]] = "Q"
                board.append("".join(row))
                row[queens[i]] = "."
            return board

        def backtrack(row: int):
            if row == n:
                board = generateBoard()
                solutions.append(board)
            else:
                for i in range(n):
                    if i in columns or row - i in diagonal1 or row + i in diagonal2:
                        continue
                    queens[row] = i
                    columns.add(i)
                    diagonal1.add(row - i)
                    diagonal2.add(row + i)
                    backtrack(row + 1)
                    columns.remove(i)
                    diagonal1.remove(row - i)
                    diagonal2.remove(row + i)
                    
        solutions = list()
        queens = [-1] * n
        columns = set()
        diagonal1 = set()
        diagonal2 = set()
        row = ["."] * n
        backtrack(0)
        return solutions
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425220750739](C:\Users\33168\AppData\Roaming\Typora\typora-user-images\image-20250425220750739.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>











