###  牛客网—剑指offer

```python
import sys
import collections
if __name__ == "__main__":
    # 读取第一行的n
    n = int(sys.stdin.readline().strip())
    ans = 0
    inp=[]
    for i in range(n):
        # 读取每一行
        line = sys.stdin.readline().strip()
        # 把每一行的数字分隔后转化成int列表
        inp.append(line)
    ans=switchstring(inp[0],inp[-1])
    print(ans)
```



### 1 数组

#### 1.1 二维数组中的查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数

```python
def Find(self, target, array):
        row=len(array)
        col=len(array[0])
        r=0
        c=col-1
        while r<row and c>=0:
            value=array[r][c]
            if target>value:
                r +=1
            elif target< value:
                c -=1
            else:
                return True
        return False
 
```

```java

```



#### 1.2 构建乘机数组

给定一个数组*A[0,1,...,n-1],*请构建一个数组*B[0,1,...,n-1]*,其中B中的元素*B[i]=A[0]A[1]...A[i-1]A[i+1]...A[n-1]*。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];） 

```python
from functools import reduce
class Solution:
    def mul(self,a,b):
        return a*b
    def multiply(self, A):
        # write code here
        B=[0 for _ in range(len(A))]
        for i,j in enumerate(A):
            A.pop(i)
            B[i]=reduce(self.mul,A)
            A.insert(i,j)
        return B
```

### 2 链表

#### 2.1 链表中环的入口结点

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

```python
    def EntryNodeOfLoop(self, pHead):
        # write code here
        tempList = []
        p = pHead
        while p:
            if p in tempList:
                return p
            else:
                tempList.append(p)
            p = p.next
```

```java
public Boolean isHasNode(ListNode node){
    if(node==null) return false;
    ListNode slow=node,fast=node;
    while(fast!=null && fast.next!=null)
        slow=slow.next;
        fast=fast.next.next;
        if(slow==fast){
            return true;
        }
    }
    return false;
}
```



#### 2.2 删除链表中重复的结点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

```python
def deleteDuplication(self, pHead):
        # write code here
        if pHead is None or pHead.next is None:
            return pHead
        head1 = pHead.next
        if head1.val != pHead.val:
            pHead.next = self.deleteDuplication(pHead.next)
        else:
            while pHead.val == head1.val and head1.next is not None:
                head1 = head1.next
            if head1.val != pHead.val:
                pHead = self.deleteDuplication(head1)
            else:
                return None
        return pHead
```

#### 2.3 链表中倒数第k个节点

输入一个链表，输出该链表中倒数第k个结点。

```python
class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        slow=head
        i=0
        while head:
            i +=1
            head=head.next
            if i>k:
                slow=slow.next
        if i>=k:
            return slow
        else:
            return None
```

#### 2.4 反转链表

输入一个链表，反转链表后，输出新链表的表头。

```python
    def ReverseList(self, pHead):
        if not pHead or not pHead.next:
            return pHead
        last = None
        while pHead:
            tmp = pHead.next
            pHead.next = last
            last = pHead
            pHead = tmp
        return last
```

#### 2.5  合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

```python
    def Merge(self, pHead1, pHead2):
        # write code here
        head=ListNode(0)
        ans=head
        while pHead1 and pHead2:
            if pHead1.val >pHead2.val:
                ans.next=pHead2
                ans=ans.next
                pHead2=pHead2.next
            else:
                ans.next=pHead1
                ans=ans.next
                pHead1=pHead1.next
        if pHead1:
            ans.next=pHead1
        if pHead2:
            ans.next=pHead2
        return head.next
```

### 3 树

#### 3.1 重建树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

```python
    def reConstructBinaryTree(self, pre, tin):
        if not pre or not tin:
            return None
        root = TreeNode(pre.pop(0))
        index = tin.index(root.val)
        root.left = self.reConstructBinaryTree(pre, tin[:index])
        root.right = self.reConstructBinaryTree(pre, tin[index + 1:])
        return root
```

#### 3.2  二叉树的下一个结点

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

```python
    def GetNext(self, pNode):
        # write code here
        if not pNode:
            return pNode
        if pNode.right:
            left1=pNode.right
            while left1.left:
                   left1=left1.left
            return left1
        p=pNode
        while pNode.next:
            tmp=pNode.next
            if tmp.left==pNode:
                return tmp
            pNode=tmp
```

#### 3.3 对称的二叉树

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

```python
    def isSymmetrical(self, pRoot):
        # write code here
        def is_same(p1,p2):
            if not p1 and not p2:
                return True
            if (p1 and p2) and p1.val==p2.val:
                return is_same(p1.left,p2.right) and is_same(p1.right,p2.left)
            return False
        if not pRoot:
            return True
        if pRoot.left and not pRoot.right:
            return False
        if not pRoot.left and pRoot.right:
            return False
        return is_same(pRoot.left,pRoot.right)
```

#### 3.4  按之字形顺序打印二叉树

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

```python
    def Print(self, pRoot):
        # write code here
        if not pRoot:
            return []
        from collections import deque
        res,tmp=[],[]
        last=pRoot
        q=deque([pRoot])
        left_to_right=True
        while q:
            t=q.popleft()
            tmp.append(t.val)
            if t.left:
                q.append(t.left)
            if t.right:
                q.append(t.right)
            if t == last:           
                res.append(tmp if left_to_right else tmp[::-1])
                left_to_right= not left_to_right
                tmp=[]
                if q:last=q[-1]
        return res
```

#### 3.5 把二叉树 打印成多行

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

```python
    def Print(self, pRoot):
        # write code here
        if not pRoot:
            return []
        ans=[]
        root=[pRoot]
        while root:
            l=len(root)
            ls=[]
            for i in range(l):
                temp=root.pop(0)
                ls.append(temp.val)
                if temp.left:
                    root.append(temp.left)
                if temp.right:
                    root.append(temp.right)
            ans.append(ls)
        return ans
```

#### 3.6 序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树。序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。

```python 
class Solution:
    flag = -1
    def Serialize(self, root):
        # write code here
        if not root:
            return '#'
        return str(root.val) + ',' + self.Serialize(root.left) + ',' + self.Serialize(root.right)
     
    def Deserialize(self, s):
        # write code here
        self.flag += 1
         
        l = s.split(',')
        if self.flag >= len(s):
            return None
         
        root = None
        if l[self.flag] != '#':
            root = TreeNode(int(l[self.flag]))
            root.left = self.Deserialize(s)
            root.right = self.Deserialize(s)
        return root
```

#### 3.7  二叉搜索树的第k个结点

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

```python
class Solution:
    def KthNode(self, pRoot, k):
        global result
        result=[]
        self.midnode(pRoot)
        if  k<=0 or len(result)<k:
            return None
        else:
            return result[k-1]
    def midnode(self,root):
        if not root:
            return None
        self.midnode(root.left)
        result.append(root)
        self.midnode(root.right)
```

#### 3.8  树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

```python
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        if  pRoot1==None or  pRoot2==None:
            return False
        return self.helper(pRoot1, pRoot2) or self.HasSubtree(pRoot1.left, pRoot2) or self.HasSubtree(pRoot1.right, pRoot2)
        # write code here
    def helper(self,node1,node2):
        if node2==None:
            return True
        if node1 ==None or node1.val !=node2.val:
            return False
        return self.helper(node1.left,node2.left) and self.helper(node1.right,node2.right)
```



### 4 栈和队列

#### 4.1 用栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

```python
class Solution:
    def __init__(self):
        self.stack1 = []
        self.stack2 = []
    def push(self, node):
        # write code here
        self.stack1.append(node)
    def pop(self):
        # return xx
        if self.stack2 == []:
            while self.stack1:
                a=self.stack1.pop()
                self.stack2.append(a)
        return self.stack2.pop()
```

### 5 动态规划和贪婪

#### 5.1  剪绳子

给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

```python
class Solution:
    def cutRope(self,number):
        # write code here
        if number == 1:return 1
        if number == 2:return 1
        if number == 3:return 2
        dp = [1]
        for i in range(2,number+1):
            maxs = i
            for j in range(1,i-1):
                tmp = dp[i-j-1]*j
                if tmp>maxs:
                    maxs = tmp
            dp.append(maxs)
        return dp[-1]
```

#### 5.2 矩形覆盖

我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

比如n=3时，2*3的矩形块有3种覆盖方法：

```python
class Solution:
    def rectCover(self, number):
        # write code here
        dp=[i for i in range(number+1)]
        if number<3:
            return dp[-1]
        for j in range(3,number+1):
            dp[j]=dp[j-1]+dp[j-2]
        return dp[-1]
```



### 6 回溯

#### 6.1  机器人的运动范围

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

```python
class Solution:
    def movingCount(self, threshold, rows, cols):
        # write code here
        board = [[0 for _ in range(cols)] for _ in range(rows)]
        def block(r,c):
            s = sum(map(int,str(r)+str(c)))
            return s>threshold
        class Context:
            acc = 0
        def traverse(r,c):
            if not (0<=r<rows and 0<=c<cols): return
            if board[r][c]!=0: return
            if board[r][c]==-1 or block(r,c):
                board[r][c]=-1
                return
            board[r][c] = 1
            Context.acc+=1
            traverse(r+1,c)
            traverse(r-1,c)
            traverse(r,c+1)
            traverse(r,c-1)
        traverse(0,0)
        return Context.acc
```

#### 6.2 矩阵中的路径

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。

```python
class Solution:
    def hasPath(self, matrix, rows, cols, path):
        # write code here
        for i in range(rows):
            for j in range(cols):
                if matrix[i*cols+j]==path[0]:
                    if self.find(list(matrix),rows,cols,path[1:],i,j):
                        return True
        return False
    def find(self,matrix,rows,cols,path,i,j):
        if not path:
            return True
        matrix[i*cols+j]='0'
        if j+1<cols and matrix[i*cols+(j+1)]==path[0]:
            return self.find(matrix,rows,cols,path[1:],i,j+1)
        elif j-1>=0 and matrix[i*cols+(j-1)]==path[0]:
            return self.find(matrix,rows,cols,path[1:],i,j-1)
        elif i+1<rows and matrix[(i+1)*cols+j]==path[0]:
            return self.find(matrix,rows,cols,path[1:],i+1,j)
        elif i-1>=0 and matrix[(i-1)*cols+j]==path[0]:
            return self.find(matrix,rows,cols,path[1:],i-1,j)
        else:
            return False
```

#### 6.3  正则表达式匹配

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配。

```python
class Solution:
    def match(self, s, pattern):
        if (len(s) == 0 and len(pattern) == 0):
            return True
        if (len(s) > 0 and len(pattern) == 0):
            return False
        if (len(pattern) > 1 and pattern[1] == '*'):
            if (len(s) > 0 and (s[0] == pattern[0] or pattern[0] == '.')):
                return (self.match(s, pattern[2:]) or self.match(s[1:], pattern[2:]) or self.match(s[1:], pattern))
            else:
                return self.match(s, pattern[2:])
        if (len(s) > 0 and (pattern[0] == '.' or pattern[0] == s[0])):
            return self.match(s[1:], pattern[1:])
        return False
```





