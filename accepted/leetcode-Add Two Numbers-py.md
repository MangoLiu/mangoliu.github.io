```
Add Two Numbers*
You are given two linked lists representing two non-negative numbers. 
The digits are stored in reverse order and each of their nodes contain a single digit. 
Add the two numbers and return it as a linked list.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

解释题意：
相当于用两个list分别表示两个数字，342+465=807
注意，开始的表示个位，最后结果以list的形式进行返回。
```

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if not l1:
            return l2
        if not l2:
            return l1
            
        rlist = []
        flag = 0
        for i in xrange(0,max(len(l1),len(l2))):
            if i<len(l1) and i<len(l2):
                rlist.append((l1[i]+l2[i]+flag)%10)
                if l1[i]+l2[i]+flag>9:
                    flag = 1
                else:
                    flag = 0
            elif i > len(l1)-1:
                rlist.append((l2[i]+flag)%10)
                if l2[i]+flag>9:
                    flag = 1
                else:
                    flag = 0 
            else:
                rlist.append((l1[i]+flag)%10)
                if l1[i]+flag>9:
                    flag = 1
                else:
                    flag = 0
        if flag != 0:
            rlist.append(flag)
        return rlist  
```
error:<br>
input:[0]<br>
[0]<br>
answer:Line 16: TypeError: object of type 'ListNode' has no len()<br>
Expected answer:[0]<br>