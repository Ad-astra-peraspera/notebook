## 哈希表

| 题目                                                         | 难度/考察知识点                   | 关键知识点            |
| ------------------------------------------------------------ | --------------------------------- | --------------------- |
| 242.[有效的字母异位词](##有效的字母异位词) [url](https://leetcode.cn/problems/valid-anagram/description/) | 简单/ 哈希表                      | defaultdic ord() 数组 |
| 349.[两个数组的交集](##两个数组的交集) [url](https://leetcode.cn/problems/intersection-of-two-arrays/description/) | 简单/ 哈希表 二分查找 双指针 排序 | 集合set               |
| 202.[快乐数](##快乐数)                                       | 简单/哈希表 双指针                |                       |
| 1.[两数之和](##两数之和)                                     | 简单/                             |                       |
| [四数相加II](##四数相加II)[url](https://leetcode.cn/problems/4sum-ii/submissions/661928452/) | 中等                              | 推导式                |
| [赎金信](##赎金信)[url](https://leetcode.cn/problems/ransom-note/) | 简单                              |                       |
| [15. 三数之和](##三数之和) [url](https://leetcode.cn/problems/3sum/) | 中等/双指针/排序                  | sort()方法            |
| [18.四数之和](##四数之和)  [url](https://leetcode.cn/problems/4sum/description/) | 中等/                             |                       |

### 有效的字母异位词

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        #要满足是字母异位词：1.字符数相等 2.所使用的字母完全相等
        #观察例题发现，可能一个字母会被使用不止一次
        ls = len(s)
        lt = len(t)
        if ls != lt:
            return False
        else:
            if self.checkCh(s) == self.checkCh(t):
                return True
            else:
                return False
                #遍历字符串，把字符串的字母作为键，字母出现的次数作为值加入字典
    
    def checkCh(self,string:str)->dict:
        result={}
        for ch in string:
            if ch not in result:
                result[ch]=1
            else:
                result[ch]+=1
        return result
      
#可优化：
        else:
            return self.checkCh(s) == self.checkCh(t)
```

这个解法时间复杂度O(n),空间复杂度==O(1)==。

> - 空间复杂度只看“新增了多少空间”，不是操作了多少次。在这里，由于全是小写字母，不论字符串多长，字典里最多是26个字母，因此是O(1)

####  collections.defaultdict

这个写法还可以继续优化：

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import defaultdict
        
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for x in s:
            s_dict[x] += 1
        
        for x in t:
            t_dict[x] += 1
        return s_dict == t_dict
```

`defaultdict(int)`表示：**遇到不存在的键，自动创建并赋初值0**。

int其实是int()函数，在遇到一个不存在的键的时候，如‘z’,就会自动：

`dic['z']=int()`而int()返回0。

`defaultdict(工厂函数)`括号里不一定是int，可以是任意==工厂函数==。工厂函数是一个**可以调用返回初值的函数**，比如：int、list、set等。也可以是自己编写的函数

| **写法**          | **默认值** | **适用场景**             |
| ----------------- | ---------- | ------------------------ |
| defaultdict(int)  | 0          | 统计次数（计数器）       |
| defaultdict(list) | []         | 分组归类（一对多关系）   |
| defaultdict(set)  | set()      | 分组去重（去除重复元素） |

++++



### 使用数组

在这道题里，由于只有26个小写字母，那么使用数组会更加简单。因为字典涉及到维护哈希值等操作，而数组只需要直接通过索引访问。

而使用数组需要解决的问题是，如何把字符映射到索引0-25中。

#### Ord()函数

`Ord()`函数 会返回字符 的 Unicode 编码（整数）。ord('a') 是 'a' 的编码（97）。所以 'a' - 'a' = 0，‘b’ - ‘a’ = 1，…，‘z’ - ‘a’ = 25。

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        arr = [0]*26
        for ch in s:
            arr[ord(ch)-ord('a')] += 1
        for ch in t:
            arr[ord(ch)-ord('a')] -= 1
        for _ in arr:
            if _ !=0:
                return False
        return True

```

==注意==

```python
for _ in arr:
    if _ !=0:
```

这里我第一次做的时候返了一个错误：我写成了arr[_] ，\_是数组里的**元素值**，**不是索引**！因此直接判断__的值是否为0就好了。

+++



### 其他解法：collections.Counter

Counter 是 collections 模块中的一个**子类**，专门用来做**元素计数**。它本质上就是一个**带有默认值的字典**，默认值是0。使用起来非常简单，**可以直接统计字符串中每个字符出现的次数**。

```python
class Solution(object):
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter
        a_count = Counter(s)
        b_count = Counter(t)
        return a_count == b_count
```



+++

## 两个数组的交集



> 第一次做的思路：
>
> 数组里的数的大小不能确定，因此上一题使用的数组的解法在此不太适用
>
> 这道题即是要输出两个数组里相同的元素，可以考虑字典，字典里的键如果在另一个字典可以找到，即把这个键存入一个数组返回。

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        from collections import defaultdict
        dic1=defaultdict(int)
        dic2=defaultdict(int)
        for i in nums1:
            dic1[i] += 1
        for i in nums2:
            dic2[i] += 1
        result=[]
        for i in dic1:
            if i in dic2:
                result.append(i)
        return result 
```

时空复杂度都是O(n)

### 使用集合

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1)&set(nums2))
```

时空复杂度也是O(n)，但由于不需要存value的值，因此空间占用要稍小一些。若只建一个集合去查另外一个集合空间占用会更小。



##  快乐数

> 在第一次做的时候，思路出现了一些问题：
>
> 这道题要做的其实是判断一个数，每位的平方和能否是1（10、100、1000.。。。）  那么只要知道10、100、1000、这些是由哪些平方和的数相加得到，并且这个数只能由n个（1-9）的平方相加而成。但是会不会有数字不断增大，那怎么判断是否终止循环呢？

这个思路错了，因为虽然在最开始会快速增加，但是接近1000，这个数的每一位的平方和会比自己小，因此一定会在这个范围循环。

那么只需要一个集合，判断是否出现在这个集合里，出现了说明是陷入了无限循环。

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        seen=set() #seen 里不能有1
        while n not in seen :#如果n没在seen里，就继续算
            if n == 1:
                return True
            seen.add(n)
            n = self.sum_square(n)
            
        #如果发现n在seen里，说明循环了，return false
        return False
        
    def sum_square(self,n:int)->int:
        if n == 0:
            return 0
        return (n%10)**2+self.sum_square(n//10)
```

这里的平方和有更简单的方法：

```python
n = sum(int(i)**2 for i in str(n))
```





## 两数之和

> 以前做过，被搞得自闭，但现在来看很简单啦。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #建立一个字典，储存数组中的数字及其下标。数字为键，下标为值
        dic = {}
        for index,value in enumerate(nums):
            if (target-value) in dic:
                return [dic[target-value],index]
            else:
                dic[value]=index

        return -1
          
```

但是在又一次做的时候，还是发现了诸多没有完全搞明白的地方。

`enumerate()`函数的返回值是(下标，值)的元组。

要先看target-value是否在dic里，这样可以避免 当前元素是3，target是6，把元素储存进去，看6-3=3，返回两个3的坐标的情况。



+++

## 四数之和II

> 最开始的思路其实是对的，把四个数字分为两两只和，让两部分之和为0即可。但是后面没有想清楚这样这样怎么知道出现和一样（如A = [1, 2] B = [-2, -1]，a和b的和有两种情况 = 0）怎么统计精确。但是后面看了题解发现可以使用字典对这种和进行统计。

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        import collections
        sums_of_1_2 = collections.Counter( a+b for a in nums1 for b in nums2)
        ans = 0
        for c in nums3:
            for d in nums4:
                if -c - d in sums_of_1_2:
                    ans += sums_of_1_2[-c-d]
        return ans
```

值得注意的是`sums_of_1_2 = collections.Counter( a+b for a in nums1 for b in nums2)`这一句

使用了生成器表达式**(generator expression)**，语法上和 **列表推导式 (list comprehension)** 类似。



## 赎金信

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        import collections
##        rans_dict = collections.Counter(for ch in list(ransomNote))
        mag_dict = collections.Counter(ch for ch in list(magazine))
        for ch in ransomNote:
            if ch not in mag_dict or mag_dict[ch] == 0:
                return False
            else:
                mag_dict[ch] -= 1
        return True
```

mag_dict = collections.Counter(ch for ch in list(magazine)) 这一句使用了上一道题学到的生成器表达式，但是其实不用。字符串本身是一个==可迭代对象==，会一个个字符迭代出来。collections.Counter 接收任何 iterable，所以字符串完全没问题。

所以有这几种写法

```python
rans_dict = collections.Counter(ransomNote)#直接传字符串

rans_dict = collections.Counter(list(ransomNote))#传列表

rans_dict = collections.Counter(ch for ch in ransomNote)#生成器表达式
```



## 三数之和

>  一开始，我的思路是这样的：
>  #题目要求：数组中每个元素只能使用一次。要求元组不能重复（即元素相同、顺序不同的元组视作一个答案）
>  \#观察示例发现，输出的三元组里的元素是值不是索引。  \#思路：两个指针遍历数组，储存两两数之和，并用字典储存，和为键，值为下标的集合
>  \#然后再：满足下标不在这个和所对应的集合里，即是满足题意条件的答案。

```python
        from collections import defaultdict
        dic=defaultdict(set)
        result=[]
        for slow in range(len(nums)):
            fast = slow+1
            if fast < len(nums):
                dic[nums[slow]+nums[fast]].append(slow,fast)
            slow +=1
        for i in range(len（nums):
            if (0-nums[i]) in dic:
                if i not in dic[0-nums[i]]:
                    result.append([nums[i],nums[dic[0-nums]     ]])
```

但是很快我发现我写不下去了，因为去重非常麻烦。例如一个数组[0,0,0,0,0]，那么我最后需要for很多次（并且也没有考虑到在集合里有很多下标的可能性）

这道题应该使用双指针，这样可以有效去避免字典的去重问题。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        #题目要求：数组中每个元素只能使用一次。要求元组不能重复（即元素相同、顺序不同的元组视作一个答案）
        #观察示例发现，输出的三元组里的元素是值不是索引。
        nums.sort()
        result=[]
        i = 0

        if nums[i] > 0:
            return result

        for i in range(len(nums)):
            if i>0 and nums[i]==nums[i-1]:
                i += 1
                continue
            left = i+1
            right = len(nums)-1

            while left<right:

                sums = nums[i] + nums[left] + nums[right]

                if sums>0:
                    right -= 1
                elif sums < 0:
                    left += 1
                else:
                    result.append([nums[i],nums[left],nums[right]])
                #以下的代码是为了在已经有答案的时候避免答案重复：注意缩进！
                    while left<right and nums[left]==nums[left+1]:
                        left += 1
                    while left<right and nums[right]==nums[right-1]:
                        right -= 1
                    left += 1
                    right -= 1
        return result
            
```

这道题的指针移动的目的需要搞清楚，例如两个while的目的在sums成功等于零之后，避免指针移动到同样大小的数上。

这道题可以用`nums.sort()`是因为这道题的返回结果是元素，而不是题目给定数组的下标，因此可以修改给定的数组。

+++

## 四数之和

> 这道题的思路和上面的三数之和差不多。唯一是需要再增加一个指针。

`````PYTHON
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        result = []
        length = len(nums) 
        i = 0

        for i in range(length) :
            if i >0 and nums[i] == nums[i-1]:
                continue
            i_2 = i + 1

            while i_2 < length- 2 :
                left = i_2 + 1
                right = length - 1
  
                while left <right :

                    target_of_lr = target - (nums[i] + nums[i_2])
                    sums_of_lr = nums[left] + nums[right]

                    if sums_of_lr < target_of_lr:
                        left += 1
                    elif sums_of_lr > target_of_lr:
                        right -= 1
                    else:
                        result.append([nums[i],nums[i_2],nums[left],nums[right]])

                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                
                i_2 += 1
                while i_2 < (length - 1) and nums[i_2] == nums[i_2-1]:
                    i_2 += 1
        return result            

`````

在做这道题的时候，主要的问题有两个：

1.在写`while`循环的时候，==要更新变量的值==

2.我尝试进行剪枝操作，但是：

+ 这道题的target值不确定，有可能数组的第一个值大于target但是通过其余的数相加也可以等于target
+ 剪枝操作也要注意更新变量的值，如我在while 里剪枝也出现了未更新值，死循环的问题。

