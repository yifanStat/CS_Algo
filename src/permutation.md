# <center>Permutation</center>

Problem: Given a list, return all possible permutations of such list.

Leetcode Problems: [LC46](https://leetcode.com/problems/permutations/), [LC47](https://leetcode.com/problems/permutations-ii/)

### Case 1: no duplicates in the list
*Approach 1: Recursion* - reduce f(n) problem to f(n-1).
```python
def perm(arr):
    res = []
    if len(arr) == 0: return []
    elif len(arr) == 1: return [arr]
    for i in range(len(arr)):
        res += [[arr[i]] + x for x in perm(arr[:i] + arr[i+1:])]
    return res
```
Complexity: time O(n!)

*Approach 2: Iterative* - Add each element to the result one at a time. For the i-th element, place that in each position of each subset in the existing result.
```python
def perm(arr):
    res = [[]]
    for x in arr:
        curr = [[]]
        for l in res:
            for i in range(len(l) + 1):
                curr += l[:i] + [x] + l[i:]
        res = curr
    return res
```
Complexity: time O(n!), space O(n*n!)

### Case 2: there are duplicates in the input list, but require not duplicated permutations in the output.

*Approach 1: Iterative* - Based on the iterative approach in Case 1, when adding a duplicated value to a list, we should only consider positions before that existing value since the two are exchangeable. For example, when adding 1 to [2,1,3], we can only add to before 2, or between 2 and 1 but not after 1. This is because there must be an existing list [2,3,1] where when processing this one, we can add 1 in between 2 and 3 which produces the same list [2,1,3,1] as if we added 1 after 3 in the first list. 
```python
def perm_with_dup(arr):
    res = [[]]
    for x in arr:
        curr = [[]]
        for l in res:
            for i in range((l+[x]).index(x) + 1):
                curr += l[:i] + [x] + l[i:]
        res = curr
    return res
```

