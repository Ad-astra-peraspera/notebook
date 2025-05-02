| [344.反转字符串](##反转字符串) [url](https://leetcode.cn/problems/reverse-string/description/) | 简单 字符串 |      |
| ------------------------------------------------------------ | ----------- | ---- |
|                                                              |             |      |
|                                                              |             |      |
|                                                              |             |      |

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

不再显式地需要tmp变量