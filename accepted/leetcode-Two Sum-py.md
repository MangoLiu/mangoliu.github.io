```
Two Sum
Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, 
where index1 must be less than index2. Please note that your returned answers (both index1 and index2) 
are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

example:
input:[3,2,4] target=6
Expected answer:[2,3]
```

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        rlist = []
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i]+nums[j] == target:
                    rlist.append(i+1)
                    rlist.append(j+1)
                    return rlist      
```
当input非常长时，Time Limit Exceeded。<br>
给的这个demo input都是严格递增的。<br>

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        rlist = []
        i,j = 0,len(nums)-1
        while 1:
            if nums[i]+nums[j] == target:
                rlist.append(i+1)
                rlist.append(j+1)
                return rlist
            elif nums[i]+nums[j] < target:
                i += 1
            else:
                j -= 1
```
结果input=[3,2,4]时报错。看来不是严格递增的，那么把list变为递增的。<br>
```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        nums.sort()#增加了这一行
        rlist = []
        i,j = 1,len(nums)
        while 1:
            if nums[i-1]+nums[j-1] == target:
                rlist.append(i)
                rlist.append(j)
                return rlist
            elif nums[i]+nums[j] < target:
                i += 1
            else:
                j -= 1
```
这里忽视了一个问题，就是list排序后，原来的索引关系变了。<br>
考虑速度和保存原来索引，使用字典。<br>
注意1：有可能有两个元素相同的情形，要分别保存。<br>
注意2：还有忽略自身值。例如[3,2,4]，target=6，计算时发现dict[6-3]，即dict[3]有值，这样获得的结果是不正确的。<br>

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        d = {}
        i,j = 0,0
        for i in range(len(nums)):
            if nums[i] not in d:
                d[nums[i]] = i#以数值为key，以索引为value
            else:
                d[str(nums[i])+" "] = i#相同值时，添加一个空格，用以区分key
                #当有多于2个的数值相同时，也无所谓了。可以考虑下为什么。

        for j in range(len(nums)):
            if target-nums[j] in d and j != d[target-nums[j]]:#后者是保证计算时不考虑自身
                return sorted([j+1,d[target-nums[j]]+1])
            elif str(target-nums[j])+" " in d and j != d[str(target-nums[j])+" "]:
                return sorted([j+1,d[str(target-nums[j])+" "]+1])
```                

