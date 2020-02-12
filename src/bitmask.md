# <center>Bit Mask & Operations</center>

Leetcode Problems: [LC51](https://leetcode.com/problems/n-queens/), [LC52](https://leetcode.com/problems/n-queens-ii/), [LC1349](https://leetcode.com/problems/maximum-students-taking-exam/)

<p align='center'>
    <img src="../fig/ghibli01.jpg", height=300>
    <img src="../fig/ghibli02.jpg" height=300>
</p>

Problem: This is usually a backtracking problem but with high runtime requirement. Normal backtracking runs O(n!) time. With some optimization using bitmask, it shrinks the search space much faster. __N-Queens__ problem is a typical application of that: *place n queens in the board without attacking each other. Return the eligible placement.*  
__Solution:__ The idea is track the available slots to be filled/tried using bitwise operations. 
```python
import math
class Solution(object):
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """
        self.res = []
        def solve(curr, path, col, ld, rd):
            if curr == n:
                self.res.append(path[:])
                return
            slots = (1 << n) - 1
            slots &= ~col
            slots &= ~ld 
            slots &= ~rd
            while slots:
                i = slots & (-slots)
                c = int(math.log(i, 2)) + 1
                line = "."*(n-c) + 'Q' + '.'*(c-1)
                path.append(line)
                solve(curr+1, path, col|i, (ld|i)<<1, (rd|i)>>1)
                path.pop()
                slots &= ~i
        solve(0, [], 0, 0, 0)
        return self.res
```
Notice that we used several bitwise operations in the code above. [This tutorial](https://wiki.python.org/moin/BitwiseOperators) gives a good overview of such operators: <<, >>, &, |, ~, ^. For example,  
* << i is shift the bit towards left by i places. e.g., 11 << 2 -> 1100; >> is the other way around.
* &, |, ~, ^ are and (intersect), or (union), complement, and exclusive or.   

Negative numbers in bitwise operation are a bit confusing. In python, the negative sign means <font color='orange'>Two's Complement</font>. Specifically, -x = ~(x-1). For example, -10 = ~ (10-1) = ~9. One useful trick with this is x & (-x) equal to 10..0 where 1 sits in the first non-zero digit from the right. 

