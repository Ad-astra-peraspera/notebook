## python的输入

###  input()

`input()`函数

```python
line = input()
#input()的返回值是str类型
int = int(input())
```

`input()`函数常与[`map()`](#####map())函数一起用。

```python
line=input()
arr=list(map(int,line.split()))
```



### 处理不定行的输入

```python 
import sys
data = sys.stdin.read().splitlines()  # 每一行是一个字符串
```



## python常用函数

### map()

map()的语法是：

```python
map(function,iterable)
#function：一个函数或方法
#iterable：一个或多个序列（可迭代对象）
```

**map() 函数的作用是：对序列 iterable 中每一个元素调用 function 函数，返回一个map对象实例。这个map对象本质上来讲是一个迭代器**。

```python
def double_func(x):
    return x* 2

bonuses = [100, 200, 300]

iterator = map(double_func, bonuses)

```

`map()`函数与`lambda`表达式常常组合在一起用。

```python
#map与lambda结合在一起用
bonuses = [100, 200, 300]
iterator = map(lambda x: x*2, bonuses)
print(list(iterator))  # 输出：[200, 400, 600]

#map函数可以输入多个可迭代对象
b1 = [100, 200, 300]
b2 = [1, 2, 3]

iterator = map(lambda x,y : x*y, b1, b2)
print(list(iterator))  # 输出：[100, 400, 900]
```



### zip()

zip() 是 Python 的内置函数，用于**将多个可迭代对象“打包”在一起**，变成一个新的可迭代对象，每一项是由原可迭代对象中对应位置的元素组成的元组。

==zip() 返回的是一个迭代器（Python 3 中），需要用 list() 或 for 循环消费它。==

```python
a = [1, 2, 3]
b = ['a', 'b', 'c']

zipped = zip(a, b)
print(list(zipped))  # [(1, 'a'), (2, 'b'), (3, 'c')]

#构建字典
keys = ['name', 'age', 'city']
values = ['Alice', 25, 'Tokyo']

d = dict(zip(keys, values))
print(d)
# {'name': 'Alice', 'age': 25, 'city': 'Tokyo'}

#并行遍历多个列表
names = ['Tom', 'Jerry', 'Spike']
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(f"{name} scored {score}")
    
#反向解包
zipped = [(1, 'a'), (2, 'b'), (3, 'c')]
a, b = zip(*zipped)
print(a)  # (1, 2, 3)
print(b)  # ('a', 'b', 'c')
```



`__eq__()`方法

`__hash__()`





## 数组（列表）

> python中其实没有像c一样==固定长度==的数组。python中的数组通常指的是列表。

| **项目**     | **列表（Python list）**    | **真正的数组（比如C语言）** |
| ------------ | -------------------------- | --------------------------- |
| 内存存储     | 存放元素的**指针**（引用） | 连续存放元素本身            |
| 类型限制     | 任意类型都可以混着放       | 必须同一类型                |
| 是否动态扩展 | 是                         | 否                          |
| 底层特性     | 动态数组（Dynamic Array）  | 固定大小数组                |

- 通过索引访问元素，时间复杂度是 **O(1)**。

- 因为**初始内存连续 + 偏移寻址**，即使内部是指针，也能快速定位到元素。

	##### **关于列表的内存连续性**

	> Python的列表**初始是连续内存块**。

	> 如果只做**固定大小**的使用（比如 [0]*26），不会扩容，**可以近似看作连续内存**。

	> 当元素增加（比如 .append()超出容量）时，**Python会重新分配更大的内存块**，旧元素搬家。



### 一维列表

python的一维列表是一个有序、可变、可重复的**==元素==**[^注]集合。

[^注]:元素意味着里面的内容不一定要保持一样的类别。

```python
list=[1,2,3,4，'char']
list[0]#支持索引查找。列表下表从零开始
list[4]= 'changeto' #可以通过索引来修改其中的元素
list=[0]*n #创建大小为n的列表
```

###### 遍历

```python
for i , v in enumerate(arr):
```

`enumerate()`函数会生成一个迭代器，这个迭代器在每一步都会返回一个[<u>元组</u>](##元组)，元组的第一个值是当前元素的下标（index），第二个值是对应的元素（value）。

```python
nums = [2, 7, 11, 15]
for index, value in enumerate(nums):
    print("下标:", index, "值:", value)
```

```
下标: 0 值: 2  #元组(0,2)是enumerate的返回值
下标: 1 值: 7    
下标: 2 值: 11
下标: 3 值: 15
```

###### 切片

```python
s[start:end:step]#step可省略，省略时默认为1. start和end不写时默认为1，但第一个"："不可省略
arr = [10, 20, 30, 40, 50, 60]
```

==重要的是，切片保持左闭右开==

| **切片语法**  | **返回结果**             | **说明**                      |
| ------------- | ------------------------ | ----------------------------- |
| arr[1:4]      | [20, 30, 40]             | 从索引1取到索引3**（不含4）** |
| arr[:3]       | [10, 20, 30]             | 从开头开始取到索引2           |
| arr[3:]       | [40, 50, 60]             | 从索引3取到结尾               |
| arr[::2]      | [10, 30, 50]             | 每隔1个元素取（步长为2）      |
| arr[::==-1==] | [60, 50, 40, 30, 20, 10] | 倒序取整个数组                |
| arr[==-2==:]  | [50, 60]                 | 倒数第2个元素开始到最后       |
| arr[:-1]      | [10, 20, 30, 40, 50]     | 取到倒数第二个元素            |



###### 修改

```python
#修改
list[3]=5 #前面提到过的通过索引直接修改某个位置的值
list[1:4]=[1,1,1]#使用切片批量修改

#添加
list.append() #在列表末尾添加元素
list_new=list.copy()#复制并放入一个新列表
list.insert(1,4)#在位置 1 插入元素 4

#删除
list.remove(1)#删除元素 输入的参数是索引位置
list[1:4]=[]#使用切片批量删除
list.pop()#默认弹出列表末尾的元素
list.pop(2)#弹出指定位置的元素
list.clear()#删除整张表
```









### 二维列表

> python中其实没有内置二维数组，一般使用嵌套列表（list of lists)实现二维数组。另外Numpy提供了矩阵支持。



###### 创建二维列表

+ 直接定义

```python
# 定义一个 3x3 的二维数组
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
#等价于
martix=[[1,2,3],[4, 5, 6],[7, 8, 9]]

#或者可以这样：
matrix = [[0] * 4 for _ in range(3)]
```

> _是循环占位符，循环变量本身不需要被引用或使用，所以使用下划线 _ 来代替一个实际的变量名。
> 这是约定俗成的写法，这表明这个变量的值不重要。





+ 列表推导式创建

```python
rows, cols = 3, 4
matrix = [[0 for _ in range(cols)] for _ in range(rows)]
```

==注意==

> **不可以**使用类似于`matrix = [[0] * cols] * rows` 创建列表。这种方式所有行引用同一个内层列表，修改其中一个元素会影响所有的行。
>
> 而使用for循环,例如`matrix = [[0] * 4 for _ in range(3)]`，在每一次循环时都会创建一个新的[0]\*4列表，得到的三个列表对象相互独立。



###### 访问与修改

可以使用常规的索引来访问或修改二维数组中的元素。

```python
# 访问第二行第三列的元素（注意：Python 的索引从 0 开始）
value = matrix[1][2]

# 修改第一行第二列的元素
matrix[0][1] = 99

#整行赋值。如果想整列赋值，无法像NumPy那样直接用切片对整列赋值，需要遍历每一行，修改对应列的元素。
matrix[1] = [1, 2, 3, 4]

#使用enumerate()函数遍历嵌套数组
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
for index, row in enumerate(matrix):
    print("Index:", index, "Row:", row)
""""
Index: 0 Row: [1, 2, 3]
Index: 1 Row: [4, 5, 6]
Index: 2 Row: [7, 8, 9]
''''''
```







##  序列 series















## 元组





## 集合set



> - Python内置的数据结构。
> - 特点：**无序**、**不重复**、**元素唯一**。
> - 底层实现：**哈希表（Hash Table）**。每个元素通过 hash() 函数计算位置。只存储元素本身，哈希元素来决定存放位置。{}里存的就是元素本身。
> 	- 元素自己就是**哈希表里的“键”**。
> 	- 存进去的位置由元素自己的hash()值决定。
> 	- 没有单独的值（value）。
> - 主要用途：**快速判断元素是否存在**，以及**集合运算（交、并、差）**。
> - 查找/插入/删除的时间复杂度是**O(1)**（平均情况下）。
> - ==只支持**不可变（hashable）类型**作为元素==（比如整数、字符串、元组可以；列表、字典不可以）。

### 集合的创建

```python

# 直接用花括号
s = {1, 2, 3}

# 用 set() 函数
s = set([1, 2, 3, 2, 1])  # 自动去重，结果是 {1, 2, 3}

# 创建空集合，必须用 set()
empty_set = set()
```

==注意==

dic={} 是空字典，空集合一定要用set1= set()。

### 常用操作

| **方法**   | **说明**                      | **示例**               |
| ---------- | ----------------------------- | ---------------------- |
| add(x)     | 添加元素x                     | s.add(5)               |
| remove(x)  | 删除元素x（如果不存在会报错） | s.remove(2)            |
| discard(x) | 删除元素x（不存在也不会报错） | s.discard(2)           |
| clear()    | 清空集合                      | s.clear()              |
| in         | 判断元素是否存在              | 3 in s → True or False |

| **运算** | **说明**               | **示例**                               |
| -------- | ---------------------- | -------------------------------------- |
| 交集     | 共同的元素             | s1 & s2 或 s1.intersection(s2)         |
| 并集     | 所有元素，去重         | `s1                                    |
| 差集     | 在s1但不在s2           | s1 - s2 或 s1.difference(s2)           |
| 对称差集 | 两个集合中不重叠的元素 | s1 ^ s2 或 s1.symmetric_difference(s2) |











## 字典

字典是 Python 中的一种 **内置数据结构**，用于以 **键-值（key-value）对** 的形式存储数据。

| **特性**     | **描述**                                                     |
| ------------ | ------------------------------------------------------------ |
| **无序**     | Python 3.6 之前无序，3.7+ 保持插入顺序，但逻辑上仍然不依赖顺序。 |
| **可变**     | 可以修改、添加、删除元素。                                   |
| **键唯一**   | 键不能重复，重复的键会被后面的覆盖。                         |
| **键不可变** | 键必须是不可变类型（如字符串、数字、元组）。值可以是任何类型。 |

> - 用**key的哈希值**决定位置。
> - 每个位置上存的是一对（key, value）。
> - key是定位用的，value是存储的数据。

### 字典的基本操作

```python
#创建字典
d = {"a": 1, "b": 2}
empty_dict = {}

#查
print(d["a"])     # 输出 1
print(d.get("b")) # 推荐用 get，可以避免出错

#增删改
d["c"] = 3     # 添加
d["a"] = 10    # 更新
del d["b"]
value = d.pop("c")     # 删除并返回值

#for循环添加元素至字典

dic={}
key=[1,2,3]
val=['a',"b","c"]
#method1
for _ in range(len(key)):
  dic[key[_]]=val[_]
#method2  
for k, v in zip(key, val):
    dic[k] = v
#method3
dic = {k: v for k, v in zip(key, val)}

#遍历字典
for k, v in d.items():
    print(k, v)

for key in d: #遍历键
    print(key, d[key])

for val in d.values:
		print()
```



### 字典常用方法

| **方法**      | **说明**                   |
| ------------- | -------------------------- |
| dict.keys()   | 返回所有键                 |
| dict.values() | 返回所有值                 |
| dict.items()  | 返回所有键值对（元组形式） |
| dict.get(key) | 获取值，若不存在不会报错   |
| dict.pop(key) | 删除键并返回值             |
| dict.update() | 批量添加/更新键值对        |



##  set 和 dict 的具体细节



- **内部有一个数组（表格）**，称为哈希表（hash table）。
- 元素（set的元素或dict的键值对）通过 hash() 函数计算出哈希值。
- **哈希值**再经过取模运算，决定放在哪个槽位（slot）。
- 槽位之间不要求连续，所以整体看内存是**稀疏分布**的。
- 如果两个元素哈希到同一个位置（哈希冲突），就需要处理冲突（比如开放地址法、拉链法等）。





## 类

`__init()__`是用以在类中进行初始化的

![image-20250416235351388](../../Library/Application Support/typora-user-images/image-20250416235351388.png)



## 命名规范

| **作用**   | **推荐命名**                  | **命名风格**          | **说明**                   |
| ---------- | ----------------------------- | --------------------- | -------------------------- |
| **类名**   | MyLinkedList, TreeNode        | CapWords / PascalCase | 每个单词首字母大写         |
| **函数名** | add_at_index, delete_at_index | snake_case            | 全部小写，用下划线分隔单词 |
| **变量名** | dummy_head, size              | snake_case            | 与函数名相同               |
| **常量名** | MAX_SIZE, DEFAULT_CAPACITY    | UPPER_CASE            | 所有字母大写，用下划线分隔 |







## python里的栈与队列



### 基础

| 数据结构      | 特点             | 操作方式                               |
| ------------- | ---------------- | -------------------------------------- |
| 栈（Stack）   | 先进后出（LIFO） | 只能从“顶端”添加或删除元素             |
| 队列（Queue） | 先进先出（FIFO） | 从一端进入（入队），另一端出去（出队） |

### 栈的实现与操作

Python 没有专门的栈类，一般用 **`list`** 模拟：

```python
stack = []		 #新建栈

stack.append(1)  # 入栈
stack.append(2)

stack.pop()      # 出栈，返回2

stack[-1]        #查看栈顶

not stack        #判空
```

> `list` 的 `append()` 和 `pop()` 默认是对末尾操作，因此效率较高。

### 队列的实现与操作

方法1`collections.deque`

```python
from collections import deque

queue = deque()
queue.append(1)    # 入队
queue.append(2)
queue.popleft()    # 出队，返回1 fifo

not queue          #判空
```

`deque` 是双端队列，两端操作都是 O(1)。

方法2 模拟队列

```python
queue = []
queue.append(1)     # 入队
queue.append(2)
queue.pop(0)        # 出队，但效率低
```

### 双端队列

首先要了解双端队列与队列的区别

**一般的队列（Queue）：只能 FIFO**：First In, First Out（先进先出）

这类队列的典型行为是：

| **操作名** | **含义**         |
| ---------- | ---------------- |
| enqueue    | 入队，从队尾加入 |
| dequeue    | 出队，从队首弹出 |

```python
from queue import Queue

q = Queue()
q.put(1)    # 入队
q.put(2)
q.put(3)

print(q.get())  # 出队，输出 1
print(q.get())  # 输出 2
#这个 Queue 是线程安全的，但功能上就是标准队列（FIFO），不能从中间或尾部删元素，也不能倒序。
```

 **双端队列（Deque）：可以两头进出**

| **方法**      | **说明**           |
| ------------- | ------------------ |
| append(x)     | 从右边加入         |
| appendleft(x) | 从左边加入         |
| pop()         | 从右边删除（队尾） |
| popleft()     | 从左边删除（队首） |

```python
from collections import deque
d = deque()
d.append(1)
d.append(2)
d.pop()        # 可以从队尾删
d.appendleft(0)  # 可以从队首加
```

### 底层原理

### list 的底层原理

- Python 的 `list` 底层是**动态数组（Dynamic Array）**，可以自动扩容。
- `append()` 在末尾添加元素：摊销时间复杂度是 **O(1)**。
- `pop()` 末尾删除元素：**O(1)**。
- `pop(0)` 删除第一个元素，需要移动所有后续元素：**O(n)** ❌ 不推荐用于队列。

### deque 的底层原理

- `collections.deque` 使用的是**双端链表/块链表**。
- 两端添加、删除元素都是 **O(1)**，非常适合做队列或双端栈。
- 适合大量插入/删除场景，不适合随机访问。
