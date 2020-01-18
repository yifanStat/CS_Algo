# <center>Binary Search</center>

Binary search is essential in many advanced algorithms. [Bisect package](https://docs.python.org/3/library/bisect.html) in python provides good solutions to this class of problems. This document analyze the core of the search function in details and discuss various extension of that. First of all, to apply binary search, it is required that the array is __sorted__. A usual set up is as follows:  
> Find the index of the largest value which is no larger than a given target T.

There are other variations of the problem setup. For example, find whether a given value exist in an array. Or, find the smallest value that is larger or equal to a given T. Now, here provides a template to solve these problems.
The time complexity is O(log n).

```python {.line-numbers}
def binary_search(arr, target):
    if arr[-1] <= target:
        return len(arr) - 1
    lo, hi = 0, len(arr) - 1
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] > target:
            hi = mid
        else:
            lo = mid + 1
    return lo - 1
```
Now, we explain each line of the code above. For example, if given `arr = [0,1,2,2,3,5], taget = 2`, we want to find the right most index that is no larger than the target 2, which is 3 here.  
* The while loop will stop only when `lo == hi`. Then in each step, to avoid infinite loop, at least `lo` or `hi` needs to move closer to each other. The way we define the `mid` determines that when `hi == lo + 1`, `mid` stays in `lo`. So we should make `lo = mid + 1` and `hi = mid` to make sure there is indeed a move.
* Now, here the request is to output the rightmost index which is no larger than the given target, which means even if we found a match, we still need to move towards the right to find the first value greater than the target. This determines the if-else condition. When `arr[mid] > target`, we move `hi` to `mid`. This means `arr[hi]` is always larger than the target. When `lo` meets `hi`, we return `lo - 1`. On the other hand, if we want to find the leftmost index, we will move the equal condition to the line above.
* Don't forget that we need to check edge case where the index happens at the end. 
* Many problems do not appear directly as a binary search problem. Usually, we need to substitute the if-else block by other conditions `f(...)` which are case dependent. See [LC1283](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)