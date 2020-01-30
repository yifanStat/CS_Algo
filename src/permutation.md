# <center>Permutation, Subset & Combination</center>

## <font color="grey">Table of Contents</font>
1. [Permutation](#perm)
2. [Subset](#subsest)
3. [Combination](#comb)

## PART I: Permutation<a name="perm"></a>

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

## PART II: Subset<a name="subset"></a>
Problem: Given a set, return all possible subsets.  
Leetcode Problems: [LC78](https://leetcode.com/problems/subsets/), [LC90](https://leetcode.com/problems/subsets-ii/)   

The solution is very similar to generating permutations in the iterative fashion, with the difference being adding the new element to the existing subsets and append the new subsets to the list rather than replacing the existing ones. To see this, 
```python
def subsets(S):
    res = [[]]
    for i in S:
        res += [l + [i] for l in res]
    return res
```

If there exists duplicates in the set but require no duplicated subsets in the output. To avoid duplicates, we just need to sort the set first and skip in the loop when S[i] == S[i-1].


## PART III: Combination<a name="comb"></a>
Problem: Get all combinations of K numbers out of 1, ..., n.  
Leetcode Problems: [LC77](https://leetcode.com/problems/combinations/), [LC39](https://leetcode.com/problems/combination-sum/), [LC40](https://leetcode.com/problems/combination-sum-ii/)  
*There could be another variation which is to get the number of different combinations of K numbers.  
This is a typical DFS + backtracking problem. The solution is as follows:  
```python
def getComb(k, n):
    res = []
    def recur(curr, start):
        if len(curr) == k:
            res.append(curr)
        elif start > n:
            return
        else:
            for i in range(start, n + 1):
                curr.append(i)
                recur(curr, i + 1)
                curr.pop()
    recur([], 1)
    return res
```
Complexity: time O(n^K)  
__Combination Sum__: given a set of numbers, find all combinations with sum being a target value.   
The solution is similar to above, with the termination criterion being the sum of the curr is each to the target. If allowing repeated use of each value, we just need to call recur() twice in the loop. For example,
```python
recur(curr+[i], i)
recur(curr, i+1)
```
The complete solution to LC39 is as follows:
```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        candidates.sort()
        candidates = [x for x in candidates if x <= target ]
        res = []
        def dfs(i, curr_path):
            if i >= len(candidates): return
            if candidates[i] + sum(curr_path) == target:
                res.append(curr_path + [candidates[i]])
            elif candidates[i] + sum(curr_path) > target:
                return
            else:
                dfs(i, curr_path+[candidates[i]])
                dfs(i+1, curr_path)
        dfs(0, [])
        return res
```
One variation is to just the number of such combinations. This can be solved by DP.
```python
class Solution(object):
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if target < min(nums):
            return 0
        dp = [0] * (target + 1)
        dp[0] = 1
        for x in nums:
            for i in range(x, target + 1):
                dp[i] += dp[i - x]
        return dp[-1]
```
Complexity: O(n*target)
