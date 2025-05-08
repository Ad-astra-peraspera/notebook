# leetcode-数组



| 题目                                                         | 难度/考察知识点       | 关键知识点                           |
| ------------------------------------------------------------ | --------------------- | ------------------------------------ |
| 704.[二分查找](##二分查找)<br />[url](https://leetcode.cn/problems/binary-search/description/) | 简单/ 二分查找        | 二分查找                             |
|                                                              |                       |                                      |
| 59.[螺旋矩阵II](##螺旋矩阵II)   <br>[url](https://leetcode.cn/problems/spiral-matrix-ii/description/) | 中/ 数组 矩阵         | [方向移动算法](#######方向移动算法)  |
| [区间和](##区间和)<br />[url](https://www.programmercarl.com/kamacoder/0058.区间和.html)（非leetcode） | 简单/ ACM输入输出模式 | [前缀和（Prefix Sum）](######前缀和) |
| [开发商购买土地](##开发商购买土地)<br />[url](https://kamacoder.com/problempage.php?pid=1044)(非leetcode) | 简单/ ACM输入输出模式 | 前缀和                               |



## 二分查找

> 第一次做的思路：
>
> 有一个最直观的思路：因为数组是升序的，那么只需要从第一个元素开始遍历，每次遍历都执行一次判断是否等于`target`，若遍历完不存在则返回-1。
>
> 由此，这种解法的时间复杂度为O(N)。但是结合数组==升序==，==无重复==，（题目也叫二分查找）那么很容易知道这道题考察的知识点为二分查找。

二分查找的思路很简单，即是从数组的中间开始判断大小：目标值大于中位数那么目标值在中位数的右边，反之则在左边。很自然地，接下来需要解决两个问题：

> + 二分操作如何实现？
> + 二分的边界（左闭右开、左闭右闭等）如何界定？



















## 螺旋矩阵II 



> 第一次做的思路：
> 容易发现：数据从1-n^2^有序依次填入正方形，而正方形由n*n构成，即是需要一些判断在何时改变输入的方向即可完成此题。但我在思考时有几点困惑：
> 1.c语言有二维数组，python是否有类似的结构？如果有，该怎么定义这个二维数组？
> 2.这些判断如何以一般性加以推广？

思考至此，我发现我对[python的数组](/Users/hemoqi/Desktop/coding/python语法.md)了解不够，需要加以学习。

经过学习与整理，我复习了python的一维与二维数组的基础知识，回到这道题。

经过在纸上对n=3、4、5的大致列出，我发现了一些规律：

> n=3:每次转向前赋值的个数（可以看作步长）3 2 2 1 1 右 下 左 上 右
>
> n=4 : 4 3 3 2 2 1 1  右 下 左 上 右 下 左 
>
> n=5 ：5 4 4 3 3 2 2 1 1 右 下 左 上右 下 左 上  右 

步长有很明显的规律。但是按照这个规律来写的话，应该会嵌套循环，会比较麻烦。

对于方向 我发现可以通过判断：前面是否有格子（数组空间 ）（是否越界）、该数组空间是否被赋值（是否等于0）来判断是否需要转向。如此，就不再需要计算步长。

该行为需要两步：判断下一个位置是哪里–>判断下一个位置需不需要转弯。



新的问题是：

> 如何判断下一个方向是什么？
>
> 如何让方向变化？在数组中，赋值是通过`matrix[][]`来进行的。



我发现我无法较为简单的方法实现，只能求助网络。找到如下方法：

###### ==方向移动算法==

```python
#用四个元组组成的列表表示方向
d=[(0,1),(1,0),(0,-1),(-1,0)]
#d[0]=(0,1)右 d[1]= 下 d[2]=左 d[3]=上

x = 0 #在x=0时，d00 d01 即是（0，1）
new_i = i + d[x][0] #i是横坐标 d[x][0]是横方向的移动量（列移动，所以是横方向）
new_j = j + d[x][1] #j是纵坐标 d[x][0]是纵方向的移动量

#接下来要做的是让x体现四个方向的轮换

x = ( x + 1 )%4 如此 x将在 1 2 3 0间轮换
```

==这也是leetcode计算方向移动的常见方法==

由此，得到了最终的代码：

```python
def generateMatrix(n: int) -> list[list[int]]:
    matrix = [[0]*n for _ in range(n)]
    dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # 右下左上
    i, j, d = 0, 0, 0  # 初始位置在左上角，方向向右

    for num in range(1, n*n + 1):
        matrix[i][j] = num
        ni, nj = i + dirs[d][0], j + dirs[d][1]
        if ni < 0 or ni >= n or nj < 0 or nj >= n or matrix[ni][nj] != 0:#判断下一个位置
            d = (d + 1) % 4  # 顺时针换方向
            ni, nj = i + dirs[d][0], j + dirs[d][1]
        i, j = ni, nj

    return matrix
```







## 区间和

>第一次做的思路：
>
>这道题与以往做的题的不同点在于数组题目本身没有直接给定，而是需要根据输入不断向列表里赋值。也就是说，如何按照题目的输入进行接收是解决这道题需要的一个步骤。如此，我发现了掌握不熟练的地方：c使用`scanf()`函数进行接收，python使用`input()函数`，除此之外还有什么方式？[python是怎么接收的](/Users/hemoqi/Desktop/coding/python语法.md)?以及什么是acm输入输出模式？

###### ACM模式

>ACM来源于[ACM国际大学生程序设计竞赛](https://so.csdn.net/so/search?q=ACM国际大学生程序设计竞赛&spm=1001.2101.3001.7020)。
>
>- 机试系统调用你的代码时，传入的都是字符串，**题目的输入描述**会说明字符串的组成规则，你需要根据这些规则从输入字符串中解析出需要的数据。
>- 当算法程序计算出结果后，也不能直接返回给机试系统，因为机试系统只接收字符串，我们还需要根据**题目的输出描述**，来生成要求格式的字符串返回。

这种情况下，python需要使用以下代码：

```python
import sys
input = sys.stdin.read
```

写出了第一版的内容：（暴力破解）

```python
def solution():
    import sys

    # 读取前 n+1 行：第一个是 n，后面 n 行是数组元素
    lines = sys.stdin.read().splitlines()
    
    n = int(lines[0])                    # 数组长度
    arr = list(map(int, lines[1:n+1]))   # 数组内容（n 个元素）

    # 查询从第 n+1 行开始
    for line in lines[n+1:]:
        if line.strip() == "":
            continue
        a, b = map(int, line.strip().split())
        print(sum(arr[a:b+1]))
```

但是这个方法的时间复杂度是n*m n为数组长度，m为查询次数，这样效率低下。

学习了前缀和（Prefix Sum）的思路。

###### ==前缀和==

> “前缀和”是算法中非常经典、非常重要的技巧，尤其适合处理**大量区间求和**的问题。

![image-20250412151626895](../../Library/Application Support/typora-user-images/image-20250412151626895.png)

`p[i]`表示下标0到1的累加之和。

要求下标1-5的区间之和的话，只需要`p[5]`-`p[0]`而不需要再去数组中进行切片操作。

![image-20250412152057101](../../Library/Application Support/typora-user-images/image-20250412152057101.png)



```python
def solution():
  import sys
  lines = sys.stdin.read().splitlines()
  
  n = int(lines.pop(0))#首先取出数组的长度
  real_arr=[]
  pre_sum=[]
  sum=0
  for i in range(n):
    real_arr.append(int(lines[i]))
    sum+=int(lines[i])
    pre_sum.append(sum)
  
  for line in lines[n:]:
        if line.strip() == "":
            continue
        a,b=map(int,line.strip().split())
        if a == 0:
          print(pre_sum[b])
        else:
        	print(pre_sum[b]-pre_sum[a-1])
    
```







## 开发商购买土地



> 可以先简化为更为简单的版本：例如：1 2 3 4 5 6这样一个1*n的数组，如何分，使得两边的差距最小
>
> 可以看出，这道题为上面的前缀和的进阶版。
>
> 只需分别求出最小的 横向的差与纵向的差，然后进行比较，取较小的那个，得解。



```python
def solution():
  import sys
  data=list(map(int, sys.stdin.read().split()))
  n = data.pop(0)
  m = data.pop(0)
  arr = []
  min_minus=float('inf')
  
  #把数组进行填充
  for i in range(n):
    row=data[i*m:(i+1)*m]
    arr.append(row)
 
  #计算横向的前缀和 在这里 提前给pre_row[0]=0,防止最后计算出错
  pre_row=[0]
  row_sum=0
  for i in range(n):
    row_sum+=sum(arr[i])
    pre_row.append(row_sum)
  
  #计算纵向的前缀和：
  col_sum=[0]*m
  for j in range(m):
    for i in range(n):
      col_sum[j]+=arr[i][j]
  pre_col=[0]
  p_col_sum=0
  for j in range(m):
    p_col_sum+=col_sum[j]
    pre_col.append(p_col_sum)
    
  #比较横向的最小的前缀和之差
  for index in range(1,n):
    minus = abs((pre_row[n]-pre_row[index])-pre_row[index])
    min_minus=min(minus,min_minus)
    
	#比较纵向的：
  for index in range(1,m):
    minus = abs((pre_col[m]-pre_col[index])-pre_col[index])
    min_minus=min(minus,min_minus)
	
  print(min_minus)

if __name__ == "__main__":
    solution()
```

下面这个是我没有想到的。即在对row进行整行赋值

>  for i in range(n):
>     row=data[i*m:(i+1)*m]
>     arr.append(row)

其次，发现另外一个问题：我在第一次写的时候：

```python
  #计算横向的前缀和
pre_row=[]
  for i in range(n):
    row_sum+=sum(arr[i])
    pre_row.append(row_sum)
```

在没有声明因为没有提前声明row_sum的时候就进行`row_sum+=sum(arr[i])`是错误的写法，**row_sum += sum(arr[i])**  **是“自增（+=）”操作：**不同于赋值操作，需要提前声明。



++++++



# leetcode-链表

| 题目                                                         | 难度/考察知识点             | 关键知识点                                                   |
| ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| 203.[移除链表元素](##移除链表元素) [url](https://leetcode.cn/problems/remove-linked-list-elements/description/) | 简单/ 单向链表 递归         | [虚拟头节点](######虚拟头节点：) 递归(没完全搞明白，需要复习) |
| 707.[设计链表](##设计链表) [url](https://leetcode.cn/problems/design-linked-list/) | 中等/ 链表的基础操作        | 类的定义 self                                                |
| 206.[反转链表](##反转链表) [url](https://leetcode.cn/problems/reverse-linked-list/description/) | 简单/ 链表 递归 迭代 双指针 |                                                              |
| 24.[两两交换链表中的节点](##两两交换链表中的节点)  [url](https://leetcode.cn/problems/swap-nodes-in-pairs/description/) | 中等/ 链表 递归             |                                                              |
| 19.[删除链表的倒数第N个结点](##删除链表的倒数第N个结点)      | 中等/ 链表 双指针           | 字典 双指针 快慢指针                                         |
| 面试题02.07[链表相交](##链表相交)                            | 简单/ 链表 双指针           |                                                              |
| 142.[环形链表II](##环形链表II) [url](https://leetcode.cn/problems/linked-list-cycle-ii/description/) | 中等/ 链表 哈希表 双指针    | 字典 双指针 快慢指针 Floyd判环                               |



++++



## 移除链表元素

> 在对链表有一个初步的了解后，这道题初步的思路是比较简单的。
>
> 有1个问题：
>
> + leetcode 题目示例`Optional[ListNode]`是什么意思

查阅资料后，了解到：optional[listnode]这个变量**可能是一个 ListNode 对象，也可能是 None**。



值得注意的是leetcode提前编写好的链表的结构类：

```python
class listnode:
  def __init__(self,val=0,next=None):
    self.val=val
    self.next=next
```

在这里，\__init \__()给了两个默认的输入：val=0与next=None,即是在未定义的时候，让节点值为0，默认为尾节点。

如何删除节点？在python中，不需要手动管理内存，只需要让上一个节点的指针指向下一个元素即可，无需手动管理内存。

```python
Node1,next=node2

#删除node2
Node1.next=node3

```

如何遍历节点？

```python
#需要通过指针来遍历；
pointer=head
pointer=pointer.next
```

###### 虚拟头节点：

```python
dummy_head=ListNode(next=head)
#通过虚拟头节点，可以有效避免在删除节点时需要考虑该节点是否为头节点的问题，在最后返回头节点时，只需要 
return dummy_head.next
```



以下是我第一次写的代码：

```python
class ListNode:
  def __init__(self,val=0,next=None):
    self.val=val
    self.next=next

class solution:
  def removeElements(self,head:Optinal[ListNode],val:int)->Optional[ListNode]:
    dummy=ListNode(next=head)#给一个新的虚拟头节点
    #指针从头节点开始遍历
    pointer=head
    while(pointer!=None):
      while(ListNode(pointer).val==val):
        ListNode(pointer-1).next=ListNode(pointer+1)  
    	pointer+=1
    return dummy.next
```

但其中有几个问题：
`while(ListNode(pointer).val==val):`

`pointer`本身已经是一个实例了，`ListNode(pointer)`会调用listnode（）==再次生成一个listnode对象==。因此，这里应该是`pointer.val==val`

同样的，pointer-1也是因为我对pointer的类型不明确导致的。在数组中指针常用下标来充当，但是在这里：pointer=head，就说明pointer本身是listnode里的对象类型（一个节点实例）。

但是如果不能使用pointer-1，**怎么表示指针的前一个节点？**

用pointer.next来进行判断，用pointer.next=pointer.next.next进行删除节点，这样就避免了单向链表中需要表示指针的前一个的麻烦。

另外，还有一个问题：

```python
    pointer=head
  会报错：
 # AttributeError: 'NoneType' object has no attribute 'next'
```

这是由于当传入的链表为空时，pointer=head=None，那么pointer.next就会报错。 如果从pointer=dummy(虚拟头节点)开始，那么则不会报错。

```python
class ListNode:
  def __init__(self,val=0,next=None):
    self.val=val
    self.next=next

class solution:
  def removeElements(self,head:Optinal[ListNode],val:int)->Optional[ListNode]:
    dummy=ListNode(next=head)#给一个新的虚拟头节点
    #指针从头节点开始遍历
    pointer=dummy
    while(pointer.next!=None):
      ifpointer.next.val==val:
        pointer.next=pointer.next.next  
      else:
    		pointer=pointer.next
    return dummy.next
```



另外，这道题还可以使用递归来做：

思路是使用函数自己调用自己，直到链表的结尾：node.next=none,开始从后往前判断是否需要删除该节点。

```python
class ListNode:
  def __init__(self,val=0,next=None):
    self.val=val
    self.next=next
    
class solution:
  def removeElements(self,head:Optinal[ListNode],val:int)->Optional[ListNode]:
    if head=None:
    	return None
    head.next=self.removeElements(head.next,val)
    return head.next if head.val ==val else head
    
```

==注意==

> ```python
> head.next=self.removeElements(head.next,val)
> ```
>
> 这里必须要加`self`**因为，在函数内部调用了 removeElements，没有加 self.的话， Python 以为在调用一个“全局函数”，结果找不到，会报错。**
>
> 在类 Solution 里的一个方法中，想要在**同一个类中调用别的方法**，必须用 self. 来引用。



++++



## 设计链表

> 实现 `MyLinkedList` 类：
>
> - `MyLinkedList()` 初始化 `MyLinkedList` 对象。
> - `int get(int index)` 获取链表中下标为 `index` 的节点的值。如果下标无效，则返回 `-1` 。
> - `void addAtHead(int val)` 将一个值为 `val` 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
> - `void addAtTail(int val)` 将一个值为 `val` 的节点追加到链表中作为链表的最后一个元素。
> - `void addAtIndex(int index, int val)` 将一个值为 `val` 的节点插入到链表中下标为 `index` 的节点之前。如果 `index` 等于链表的长度，那么该节点会被追加到链表的末尾。如果 `index` 比长度更大，该节点将 **不会插入** 到链表中。
> - `void deleteAtIndex(int index)` 如果下标有效，则删除链表中下标为 `index` 的节点。

-------

做到这道题，我的问题反映了我在链表与类方面的不足：

1.**如何初始化？**并且在leetcode给出的题目模版中,并没有看到MyLinkedList（），并且`__init__(self):`的括号里只有self没有向其中添加的节点。

```python
class MyLinkedList:
    def __init__(self):        
    def get(self, index: int) -> int:       
    def addAtHead(self, val: int) -> None:        
    def addAtTail(self, val: int) -> None:
    def addAtIndex(self, index: int, val: int) -> None:
    def deleteAtIndex(self, index: int) -> None:
```

看不到MyLinkedList（）是因为leetcode会自动创建类的实例并调用方法。

`__init__(self):`的括号里只有self没有向其中添加的节点，是因为题目实现的是链表类，而不是节点类，因此要额外定义一个ListNode类。

##### 1.单链表

```python
class LinkNode:
    def __init__(self,val=0,next=None):
        self.val=val
        self.next=next
class MyLinkedList:
    #初始化时，生成一个虚拟的头节点dunmmy_node
    def __init__(self):
        self.dummy_head=LinkNode()
        self.size=0#给一个size，用以计算链表的长度

    def get(self, index: int) -> int:
        if index<0 or index >= self.size :
            return -1
        cur=self.dummy_head
        for i in range(index+1):
            cur=cur.next
        return cur.val       

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next=LinkNode(val,next=self.dummy_head.next)
        self.size+=1

    def addAtTail(self, val: int) -> None:
        p_find_tail=self.dummy_head
        while p_find_tail.next:
            p_find_tail=p_find_tail.next
        p_find_tail.next=LinkNode(val)
        self.size+=1

    def addAtIndex(self, index: int, val: int) -> None:
        if index>self.size:
            return None
        else:
            cur=self.dummy_head
            for i in range(index):
                cur=cur.next
            cur.next=LinkNode(val,next=cur.next)
        self.size+=1

    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size or index <0:
            return None
        else:
            cur=self.dummy_head
            for i in range(index):
                cur=cur.next
            cur.next=cur.next.next
        self.size-=1
```

==在做这道题时巩固的知识点：==

```python
#1.self的使用：忘记使用会导致属性找不到、报错
self.dummy_head
self.size+=1
#2.下面两者看起来好像没有差别，但是一个是让指针移动，一个是删除cur.next即cur的下一个元素
cur=cur.next
cur.next=cur.next.next
#3.LinkNode() 表示调用构造方法，创建一个新的节点对象（实例）
self.dummy_head=LinkNode()
```

##### 2. 双链表

`linknode`不符合`Capwords`约定，标准的应该是`LinkNode`。

###### 补充：命名规范 

| **作用**   | **推荐命名**                  | **命名风格**          | **说明**                   |
| ---------- | ----------------------------- | --------------------- | -------------------------- |
| **类名**   | MyLinkedList, TreeNode        | CapWords / PascalCase | 每个单词首字母大写         |
| **函数名** | add_at_index, delete_at_index | snake_case            | 全部小写，用下划线分隔单词 |
| **变量名** | dummy_head, size              | snake_case            | 与函数名相同               |
| **常量名** | MAX_SIZE, DEFAULT_CAPACITY    | UPPER_CASE            | 所有字母大写，用下划线分隔 |

```python
class linknode:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev
class MyLinkedList:
    def __init__(self):
        # 初始化双向列表
        self.dummy_head = linknode()
        self.dummy_tail = linknode()
        self.dummy_head.next = self.dummy_tail
        self.dummy_tail.prev = self.dummy_head
        self.size = 0
    def get(self, index: int) -> int:
        if index >= self.size or index < 0:
            return -1
        pointer = self.dummy_head
        for i in range(index + 1):
            pointer = pointer.next
        return pointer.val
    def addAtHead(self, val: int) -> None:
        new_node = linknode(val)
        new_node.next = self.dummy_head.next
        self.dummy_head.next.prev = new_node
        self.dummy_head.next = new_node
        new_node.prev = self.dummy_head
        self.size += 1
    def addAtTail(self, val: int) -> None:
        new_node = linknode(val)
        self.dummy_tail.prev.next = new_node
        new_node.prev = self.dummy_tail.prev
        new_node.next = self.dummy_tail
        self.dummy_tail.prev = new_node
        self.size += 1
    def addAtIndex(self, index: int, val: int) -> None:
        if index > self.size or index < 0:
            return None
        elif index == self.size:
            self.addAtTail(val)
        else:
            new_node = linknode(val)
            pointer = self.dummy_head
            for i in range(index):
                pointer = pointer.next
            tmp = pointer.next
            pointer.next = new_node
            new_node.prev = pointer
            new_node.next = tmp
            tmp.prev = new_node
            self.size += 1
    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size or index < 0:
            return None
        else:
            pointer = self.dummy_head
            for _ in range(index):
                pointer = pointer.next
            to_delete = pointer.next
            pointer.next = to_delete.next
            to_delete.next.prev = pointer
        self.size -= 1
```

> 学到的比自己的更好的写法:一个to_delete作为临时指针指向需要删除的节点
>
> ```python
> for _ in range(index):
> 	pointer = pointer.next
> to_delete = pointer.next
> pointer.next = to_delete.next
> to_delete.next.prev = pointer
> ```

++++++



## 反转链表

> 在看到这道题的时候我首先想到了题目[移除链表元素](##移除链表元素)中的递归的做法：
>
> 可以递归到链表的尾节点，然后从链表的尾节点开始反转链表。
>
> 但是在尝试后发现尚无法完全理解实现的逻辑，因此决定先用其他的方法。可以发现这道题：让我们需要一边遍历这个链表，一边更改链表的方向。但是更改方向至少需要知道：
>
> + 当前节点
> + 上一个节点
>
> 因此，容易想到，使用双指针的方法来解决这个问题。

1. 双指针

	一个`pre`指针指向上一个节点，一个`cur`指针指向目前所在的节点。

但我发现如果仅用pre和cur还是没法记录所有的所需数据：例如，当使cur.next=pre,则会导致cur无法继续遍历。因此引入一个临时指针`tmp`。

**==注意== 下面这个地方我弄混过两次！**

```python
 #以下两条语句代表了截然不同的意思
 tmp=cur.next#暂存下一个节点
 cur.next=pre#修改链表方向
  #我的理解是：=是赋值，只能又右边赋值到左边
  因此 cur.next是把cur.next(即cur节点的下一个节点)暂存到tmp
  cur.next=pre 则是把pre“赋值给”cur所指向的下一个节点
```
在下面的代码中，我犯了一个错误：

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #定义两个指针
        #在这里，由于pre和cur都被初始化为head
        pre=head
        cur=head
        while cur.next:
           cur=cur.next#并且在这里直接使cur向前移动
           if cur.next:
               tmp = cur.next  # 暂存下一个节点
               cur.next = pre  # 修改链表方向
###################在这里，导致原链表的第一个元素和第二个元素相互指向，形成死循环
               pre = cur
               cur = tmp  # 通过临时指针实现cur指针的移动
           else:
               cur.next = pre
               return cur
```

如何解决这个问题呢？

让pre先初始化为None，使cur=head时指向pre，即可。

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # 定义两个指针
        pre = None
        cur = head
        while cur:
            tmp = cur.next  # 暂存下一个节点
            cur.next = pre  # 修改链表方向
            
            pre = cur
            cur = tmp  # 通过临时指针实现cur指针的移动
        #当cur==None时，while cur判断为否，停止循环，此时pre指向原链表的最后一个节点，即新链表的头节点
        return pre
```



2. 递归

递归是以自己调用自己来实现到尾节点的。

要实现反转列表，我认为有三点需要实现：

> + 让dummy节点指向原链表的尾节点
> + 让原链表中转向
> + 让原头节点指向None

（1）让dummy节点指向原链表的尾节点

这个应该很好实现，只需在递归的步骤中，加入`dummy_head.next=head`（head在这里是递归的每个节点）就可以了

（2）让原链表转向

在递归的时候，使`head=head.next`应该可以实现



## 两两交换链表中的节点

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #首先定义一个虚拟头节点
        dummy_head=ListNode(next=head)
        #定义指针
        cur=dummy_head
        #判断：当后面有两个节点再开始交换
        while cur.next is not None and cur.next.next is not None:
            fir_to_swap=cur.next.next
            sec_to_swap=cur.next                
            #当这组节点后面为None
            if fir_to_swap.next is None:
                cur.next=fir_to_swap
                fir_to_swap.next=sec_to_swap
                sec_to_swap.next=None
            else:
                cur.next=fir_to_swap
                tmp=fir_to_swap.next #暂时储存这一组下个节点的位置
                fir_to_swap.next=sec_to_swap
                sec_to_swap.next=tmp
            cur=sec_to_swap
        return dummy_head.next
```



+++++

## 删除链表的倒数第N个结点

> 第一次做的思路：
>
> 要删除链表的倒数第n个结点，我看到的第一眼的思路：
>
> 遍历链表，并用字典储存链表中的结点信息{索引}：{结点}。
>
> 通过网络搜索资料，发现通过指针把链表结点储存进字典是非常常见且实用的做法

##### 1.使用字典

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head=ListNode(next=head)
        lis_dic = {}
        lis_dic[0]= dummy_head
        cur = dummy_head
        i = 1
        #使用指针把链表的索引和结点储存至字典
        
        while cur.next:#判断下一个节点存在，指针遍历至该结点，并储存该结点
            cur = cur.next
            lis_dic[i]=cur
            i += 1
        #储存完毕，开始删除节点
        #现在i-n代表要删除的结点
        lis_dic[i-n-1].next = lis_dic[i-n].next
        return dummy_head.next
```

使用这个方法有一点需要注意：

```python
 while cur.next:
            cur = cur.next
            lis_dic[i]=cur
            i += 1	
 #在这里，由于是在把当前结点储存至字典后i再递增1，所以在储存了最后一个节点后i还会递增1，在最后i会比链表的长度长1
```

此外，在字典储存的是直接指向该结点，因此可以使用`lis_dic[i-n-1].next`



##### 2.双指针法

人与人的智商是有差距的，这是我看到题解时的想法。

双指针的经典应用，如果要删除倒数第n个节点，让fast移动n步，然后让fast和slow同时移动，直到fast指向链表末尾。删掉slow所指向的节点就可以了。

思路想清楚代码很简单，但是能想到我觉得不简单==

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        #双指针法
        dummy_head=ListNode(next=head)#虚拟头节点
        fast=dummy_head
        slow=dummy_head#定义快慢指针
        for _ in range(n):
            fast=fast.next
        while fast.next:
            fast=fast.next
            slow=slow.next
        #在结束时，slow在要删除节点的前一个
        tmp=slow.next.next
        slow.next=tmp
        return dummy_head.next

```



++++

## 链表相交

> 在最开始我以为和KMP算法相关，但发现其实不一样，因为只要开始相交，最后的结尾一定是一样的。因此只需要把两个链表尾部对齐就好了。

尾部对齐后开始开始比对。

==注意==

> **两个链表指针“相等”通常是指它们指向同一个节点（即引用地址相同）——这可以用 is 判断。**
>
> 即使两个节点的 val 相同，它们不一定是“相等”的。
>
> 在判断结点是否是同一个时最好用==is==，而不是==“\==”==.因为假如使用了\__eq__方法，则会判断结点的值是否相等。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        pa=headA
        pb=headB#定义两个指针
        la=0
        lb=0
        if pa is None or pb is None:
            return None
        else:
            while pa.next:
                pa=pa.next
                la+=1
            while pb.next:
                pb=pb.next
                lb+=1
            if pa is not pb:
                return None
            else:
                pa=headA
                pb=headB 
                if la>=lb:
                    for _ in range(la-lb):
                        pa=pa.next
                    while pa is not pb:
                        pa=pa.next
                        pb=pb.next
                else:
                    for _ in range(lb-la):
                        pb=pb.next
                    while pb is not pa:
                        pb=pb.next
                        pa=pa.next
                return pa
     
```

在题解中，大致思路相等，但是使用了一些方法来简化。

```python
#复用cur
lenA, lenB = 0, 0
        cur = headA
        while cur:         # 求链表A的长度
            cur = cur.next 
            lenA += 1
        cur = headB 
        while cur:         # 求链表B的长度
            cur = cur.next 
            lenB += 1
        curA, curB = headA, headB
 # 让curB为最长链表的头，lenB为其长度。在后续就不需要再根据不同的长度来判断。        
				if lenA > lenB:    
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA 
      
#以及一行代码来赋值多个
curA, curB = headA, headB
lenA, lenB = 0, 0
```



+++

## 环形链表II



> 只需要知道两个问题就可以解答这道题：
>
> 1.如何知道有没有环—>结点有没有重复
>
> 2.如何找到这个环的入口？

==1.哈希表==

那么非常直观的思路就是使用哈希表（字典）

只需要让哈希表的键储存结点，值储存索引即可。

```python	
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #使用字典的键不可重复的特点来检查是否有环
        visited={}
        dummy_head=ListNode(next=head)
        per=dummy_head
        i=-1
        while per.next and per.next  not in visited :
            per=per.next
            i += 1
            visited[per] = i
        if per.next is None:
            return None
        else:
            return per.next
```

==2.快慢指针==

Floyd判环在代码随想录讲的非常清楚，可以直接在代码随想录看。

![image-20250425105229543](../../Library/Application Support/typora-user-images/image-20250425105229543.png)

![image-20250425105245400](../../Library/Application Support/typora-user-images/image-20250425105245400.png)

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        #使用快慢指针来解决这道题
        fast=head
        slow=head
        #开始寻找快慢指针第一次重合的地方
        if fast is None or fast.next is None or fast.next.next is None:
            return None
        else:
            fast=fast.next.next
            slow=slow.next
            while fast is not slow:
                if fast.next is not None and fast.next.next is not None:
                    fast=fast.next.next
                    slow=slow.next
                else:
                    return None
            p1=head
            p2=fast
            while p1 is not p2:
                p1=p1.next
                p2=p2.next
            return p1
        
```

但我发现我的代码写的十分冗余。

```python
#更简洁的版本
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = fast = head
        # 第一阶段：判断是否有环
        #只要 fast.next 存在，就可以放心执行 fast = fast.next.next，即使 fast.next.next 是 None，也不会报错，只是意味着 fast 变成了 None，下一轮循环自动终止。
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                # 第二阶段：找到环的入口
                p1 = head
                p2 = slow
                while p1 is not p2:
                    p1 = p1.next
                    p2 = p2.next
                return p1
        return None  # 无环
```

