| 题目                                                         | 难度/ 相关知识点 | 关键知识点                                   |
| ------------------------------------------------------------ | ---------------- | -------------------------------------------- |
| [344.反转字符串](##反转字符串) [url](https://leetcode.cn/problems/reverse-string/description/) | 简单 字符串      |                                              |
| [541.反转字符串II](##反转字符串II) [url](https://leetcode.cn/problems/reverse-string-ii/description/) | 简单 字符串      | 运算符优先级 字符串不可变 range() reversed() |
|                                                              |                  |                                              |
|                                                              |                  |                                              |

## 反转字符串

> 题目的要求是不得新建数组，必须原地修改数组。观察可以发现，题目是使用列表来输入字符，因此可以使用下标进行快速的查找操作

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        #定义两个指针，进行两两交换
        left = 0
        right = len(s) - 1
        while left < right:
            tmp = s[left] 
            s[left] = s[right]
            s[right] = tmp
            left += 1
            right -= 1
```

这里可以用更简单的交换值的方法：

```python
s[left],s[right] = s[right],s[left]
```

这是python的语法糖，当进行`a, b = b, a`的时候，Python 会在**赋值前先同时计算右边的值并打包成一个临时元组**，然后再解包赋值给左边

```python
# 等价于：
tmp = (b, a)   # 先构造右侧元组
a = tmp[0]
b = tmp[1]
```

不再显式地需要tmp变量。

+++



## 反转字符串II

> 大题思路与昨天一致，只是增加了判断区间的操作。

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        #1.先判断字符串中有几个2k
        length = len(s)
        i = length // (2*k)
        #余数存起来
        rest = length % (2*k)
        s_L = list(s)  # 转为列表
        #2.开始反转字符串。用双指针，双指针的索引通过2k、i来计算得出。
        for index in range(i) :
            left = 0 + 2*k*index
            right = (k-1) +2*k*index
            while left < right :
                s_L[left],s_L[right] = s_L[right],s_L[left]
                left += 1
                right -= 1
        #3.判断剩余的字符的反转情况
        if rest >= k :
            left = (2*k-1)+2*k*(i-1)+1
            right = left + k - 1
            while left < right :
                s_L[left],s_L[right] = s_L[right],s_L[left]
                left += 1
                right -= 1
        else:
            left = (2*k-1)+2*k*(i-1)+1
            right = length - 1
            while left < right :
                s_L[left],s_L[right] = s_L[right],s_L[left]
                left += 1
                right -= 1
        return ''.join(s_L)
```

==在做的时候，发现了一些掌握不熟练的地方==

> 1.  字符串是不可变的，因此要先把字符串转化成列表，最后再把列表转化为字符串输出。
>
>    ```python
>     s_L = list(s)  # 转为列表
>    return ''.join(s_L)  
>    ```
>
> 2. 注意运算符的优先级
>
>    ```python
>    i = length // (2*k)
>    i = length // 2*k
>    ```
>
>    这两行代码代表了截然不同的意思

另一种方法：使用range()函数。

### range()

```python
range(start, stop, step)
'''
start=0：从第一个字符开始；
stop=len(s_list)：一直遍历到字符串末尾；
step=2*k：每次跳过 2k 个字符（这是题目中定义的分块长度）。
'''
```

那么这样，就只需要再使用切片就可以解决这个问题了。

```python
s_l[i:i+k]#切片从i到i*k-1（左闭右开）
```

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        s_l = list(s)
        for i in range(0,len(s)-1,2*k):
            s_l[i:i+k] = reversed(s_l[i:i+k])
        return ''.join(s_l)
```

这两种方法的时空复杂度都是O(n)



