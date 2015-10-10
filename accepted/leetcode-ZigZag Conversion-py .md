```
ZigZag Conversion
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: 
(you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

说明：
Zigzag:即循环对角线结构（

0               8               16           
1           7   9           15  17           
2       6       10      14      18           
3   5           11  13          19           
4               12              20           
）
```

```python
class Solution(object):
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """

        line = [[] for i in xrange(numRows)]
        
        i = 0 #index 
        while i<len(s):
            t = 0
            while t<numRows and i<len(s):
                line[t].append(s[i])
                i += 1
                t += 1
            t = numRows -2

            while t>0 and i<len(s):
                line[t].append(s[i])
                i += 1 
                t -= 1

        result = ""   
        for i in xrange(numRows):
            result += "".join(line[i])

        return result            
```
主要思路就是：忽略那些空格的影响，那些空格的存在是为了让人脑更好了解。在实际中并没有这些空格。相当于构造numRows个list。然后string在这些list之间来回的分配字符。