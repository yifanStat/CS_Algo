# <center>Topological Sort</center>

Leetcode Problems: [LC 269](https://leetcode.com/problems/alien-dictionary/submissions/)

Problem: Given a **directed** graph, find an ordering of its nodes such that for any edge u->v, u comes before v.
<p align='center'>
    <img src="../fig/totoro03.jpg">
</p>

The basic algorithm to perform topological sorting is described as Khan's algorithm in the [wikipedia](https://en.wikipedia.org/wiki/Topological_sorting) page. The idea is to have a hash map recording the number of in-degree edges. Use a queue to push in all the nodes with zero in-degree. For any node popped from the queue, reduce the indegree by 1 from the destination node which has an edge with this popped node. If its indegree happens to be zero, push to the queue. The python implementation is as follows:  
```python
# G: adjList
# inDegree: dict()

q = deque()
for k, v in inDegree.items():
    if v == 0:
        q.append(k)

res = []
while q:
    node = q.popleft()
    res.append(node)
    for dst in G[node]:
        inDegree[dst] -= 1
        if inDegree[dst] == 0:
            q.append(dst)
return res
```