| 题目                                                         | 难度/ 相关知识点   | 关键知识点                                   |
| ------------------------------------------------------------ | ------------------ | -------------------------------------------- |
| [344.反转字符串](##反转字符串) [url](https://leetcode.cn/problems/reverse-string/description/) | 简单 字符串        |                                              |
| [541.反转字符串II](##反转字符串II) [url](https://leetcode.cn/problems/reverse-string-ii/description/) | 简单 字符串        | 运算符优先级 字符串不可变 range() reversed() |
| [替换数字](##替换数字) [url](https://kamacoder.com/problempage.php?pid=1064) | 简单 字符串 双指针 |                                              |
| [151.反转字符串里的单词](##反转字符串里的单词)  [url](https://leetcode.cn/problems/reverse-words-in-a-string/description/) | 中等               | 反转字符串 “ ”.join                          |
| [右旋转字符串](##右旋转字符串) [url](https://programmercarl.com/kamacoder/0055.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF) | 简单               |                                              |
| [28.找出字符串中第一个匹配项的下标](##找出字符串中第一个匹配项的下标) [url](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/) | 中等（？）         | [KMP](###KMP)                                |
| [459.重复的子字符串](##重复的子字符串)                       | 简单               |                                              |

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

+++

## 右旋转字符串

> 一种很简单容易想到的解法就是先把字符串转化为数组，在字符串中使用切片操作实现快速倒置。

```python
class solution(object):
    def right_change(self,s:str,k:int)->str:
        s = list(s)
        right = len(s) -1
        left = right - k + 1

        s[0:0] = s[left:right+1]
        del s[len(s)-k:]

        return "".join(s)

if __name__ == "__main__":
    try:
        k = int(input())
        s = input()
        print(solution().right_change(s,k))
    except Exception as e:
        print("ERROR:",e)
```

==注意==

在做这道题出错的地方：

```python
#1.
if __name__ == "__main__":

#2. solution需要先实例化 可以用下面这种 right_change 是实例方法
print(solution().right_change(s,k))
#也可以用另外一种
sol = solution()
print(sol.right_change(s,k))

#3.s[0:0]是空切片，即从索引0-0的元素，是[]
s[0:0] = s[left:right+1]

#4.
"".join(s) #意思是把s用“”里的东西连接起来。“”里没有内容那么意思就是直接连接，中间什么也不要加。
```

有非常简洁的方法可以实现：

```python
k = int(input())
s = input()

print(s[-k:] + s[:-k])
```



另外，题解的思想十分有趣，就是沿用[151.反转字符串里的单词](##反转字符串里的单词)的思想，进行两次翻转，这样顺序就成了题目所要求的。

```python
class solution(object):
    def right_change(self,s:str,k:int)->str:
        s = s[::-1]
        s = s[:k][::-1]+s[k:][::-1]

        return s

if __name__ == "__main__":
    try:
        k = int(input())
        s = input()
        print(solution().right_change(s,k))
    except Exception as e:
        print("ERROR:",e)  
```

+++

## 找出字符串中第一个匹配项的下标

这道题可以用暴力法破解，当然很简单，但是用的最多的还是KMP算法

### ==KMP==

我觉得这个版本是讲的最好的：[url]([【搬运】油管阿三哥讲KMP查找算法，中英文字幕，人工翻译，简单易懂_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV18k4y1m7Ar/?spm_id_from=333.337.search-card.all.click&vd_source=5cb240326e24e6e5b879b832de7eb079))

在整个方法中，最重要的是对next数组的计算。

我觉得在这道题里，所谓的前后缀对帮助理解并不直观，也并没有帮助理解。这道题应该多回顾，属于在我目前刷到的题里最难的。

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        #构造模式串的next数组
        next = [0]*len(needle)
        #1.初始化j、i指针.j指向前缀.i指向后缀 
        j = 0
        i = 1
        while i < len(needle):
            while j > 0 and needle[i] != needle[j]:
                j = next[j-1]
            if needle[i] == needle[j]:
                j += 1
                next[i] = j
            i += 1
        #开始用next数组匹配
        
        j = 0 #用来指向next数组
        for i_h in range(len(haystack)): #haystack的指针，不会回退
            while j > 0 and  haystack[i_h] != needle[j]:
                j = next[j-1]
            if haystack[i_h] == needle[j]:
                j += 1
            if j == len(needle):
                return i_h - len(needle) + 1
        return -1

```

+++



## 重复的子字符串

> 第一次做的思路：只需要沿用昨天学到的KMP算法。如果字符串是由重复的子字符串组成的，那么s字符串的next的最后一位数字一定不是0

双倍字符串的解法：

![image-20250508195645669](../../Library/Application Support/typora-user-images/image-20250508195645669.png)

![image-20250508195731845](../../Library/Application Support/typora-user-images/image-20250508195731845.png)
