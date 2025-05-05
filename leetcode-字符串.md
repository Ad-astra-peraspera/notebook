| 题目                                                         | 难度/ 相关知识点   | 关键知识点                                   |
| ------------------------------------------------------------ | ------------------ | -------------------------------------------- |
| [344.反转字符串](##反转字符串) [url](https://leetcode.cn/problems/reverse-string/description/) | 简单 字符串        |                                              |
| [541.反转字符串II](##反转字符串II) [url](https://leetcode.cn/problems/reverse-string-ii/description/) | 简单 字符串        | 运算符优先级 字符串不可变 range() reversed() |
| [替换数字](##替换数字) [url](https://kamacoder.com/problempage.php?pid=1064) | 简单 字符串 双指针 |                                              |
| [151.反转字符串里的单词](##反转字符串里的单词)  [url](https://leetcode.cn/problems/reverse-words-in-a-string/description/) | 中等               | 反转字符串 “ ”.join                          |

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

+++

## 替换数字

> 看到这道题目的第一反应即是：遍历字符串，遇到数字，就删除这个数字，插入一个“number”。于是接下来的问题转化成：
>
> 1.如何判断数字、如何删除数字
>
> 2.如何在这个地方插入一个number
>
> 想到昨天的题目：先把这个字符串转化成数组，开始遍历。由于python数组可以储存不同类型的元素，因此直接使用替换就可以做出来了。

```python
class Solution(object):
    def changenum(self,s:str)->str:
        l_s = list(s)
        num_set={"1","2","3","4","5","6","7","8","9","0"}
        for i in range(len(l_s)):
            if l_s[i] in num_set:
                l_s[i] = "number"
        return "".join(l_s)

if __name__ == "__main__":
    solution = Solution()

    while True:
        try:
            s = input()
            result = solution.changenum(s)
            print(result)
        except EOFError:
            break
```

==在做这道题发现的问题==

1. range()里是应该是一个数字，因此要加上len，这点老是忘记
2. 把字符串转列表后，里面的数字其实是被看作字符类型而不是整型，因此集合里应该是“1”等

看了题解，有另外的方法

+ 构造一个新字符串，一边遍历原字符串一边往新字符串中添加元素
+ 代码随想录所使用的方法：扩充字符串，长度是原来的长度加数字个数*（6-1）

```python
class Solution(object):
    def changenum(self,s:str)->str:
        #统计字符串里的数字个数
        num_count = sum(1 for char in s if char.isdigit())
        new_s_len = len(s) + (num_count*5)
        new_s = ['']*new_s_len
        oldindex = len(s) - 1
        newindex = new_s_len - 1

        while oldindex >= 0 :

            if  s[oldindex].isdigit() :
                new_s[newindex-5:newindex+1] = "number"
                newindex -= 6
            else :
                new_s[newindex] = s[oldindex]
                newindex -= 1
            oldindex -= 1

        return "".join(new_s)

if __name__ == "__main__":
    solution = Solution()

    while True:
        try:
            s = input()
            result = solution.changenum(s)
            print(result)
        except EOFError:
            break
```

==学到了新的函数`isdigit()`==

注意，要使用这个方法的话一定要加()，比如下面这句

```python
if  s[oldindex].isdigit:
```

`()` —— `isdigit` 是一个函数，**需要调用它**，不然只是检查函数对象是否存在（永远是 `True`），而不是判断字符是不是数字。

另外，下面这行代码也值得学习。切片操作如果使用熟练的话可以提高代码的简洁性。

```python
new_s[newindex-5:newindex+1] = "number"
```

+++

## 反转字符串中的单词

> 这道题反映出对split()函数、切片操作、 “ ” .join 的掌握不足

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s[::-1]
        return " ".join(word[::-1] for word in s.split())
        '''
        split()自动忽略多余的空格
        " ".join 把后面的用单个空格连接起来
        '''
        
```

另外还有其他的方法，如双指针判断是否是多余的空格等等，我这里用了用来一种方法，新建一个数组来储存单词，最后使用reverse反转这个数组。

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = []
        word = ''
        s += " "

        for ch in s :
            if ch == " " :
                if word != '':
                    words.append(word)
                    word = ''
                continue
                
            word += ch

        words.reverse()
        return " ".join(words)        
```

