## 三数之和

>  一开始，我的思路是这样的：
>   #题目要求：数组中每个元素只能使用一次。要求元组不能重复（即元素相同、顺序不同的元组视作一个答案）
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

