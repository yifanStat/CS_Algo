# <center>Union Find Algorithm</center>

Leetcode Problems: [LC261](https://leetcode.com/problems/graph-valid-tree/), [LC1319](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)  

This algorithm is effective in detecting cycles. The usual set up is the follows: 
```python
parents = [i for i in range(N)]

def find(x):
    if parents[x] != x:
        parents[x] = find(parents[x])
    return parents[x]

def union(x, y):
    px, py = find(x), find(y)
    if px != py:
        parents[px] = py
```

One typical application of this algorithm is to find disjoint subgraphs given a set of edges. With a bit of trick, we can detect whether a tree is valid (i.e., no cycle).
```python
from collections import defaultdict
class Solution(object):
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        parents = range(n)
        self.partition = n
        def find_parents(i):
            if parents[i] != i:
                parents[i] = find_parents(parents[i])
            return parents[i]
        def get_union(i, j):
            pi, pj = find_parents(i), find_parents(j)
            if pi != pj:
                parents[pi] = pj
                self.partition -= 1
                return True
            else:
                return False

        for edge in edges:
            if not get_union(edge[0], edge[1]):
                return False
        return self.partition == 1
```