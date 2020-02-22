# <center>DP Problem: Knapsack Problem</center>

Problem: This is combinatorial optimization problem. The common set up is given a set of items with different weight and value, determine which ones to include so we can achieve the max value while not exceeding the total weight limit. This is what we call 0-1 knapsack problem. Other variations can be seen [here](https://blog.csdn.net/m0_37809890/article/details/83153974). The textbook solution is using DP.

<p align='center'>
    <img src="../fig/totoro06.jpg" height=300 width=200>
    <img src="../fig/totoro02.jpg" height=300>
</p>

Leetcode Problem: [LC322](https://leetcode.com/problems/coin-change/), [LC518](https://leetcode.com/problems/coin-change-2/), [LC656](https://leetcode.com/problems/coin-path/)

### Unbounded Knapsack Problem   

We explain the DP solution using the coin change example. _Given a set of coins and a target amount, find the fewest number of coins that you need to make up that amount. Assume infinite number of each coins._  
__Solution:__ We construct a 1-d DP array of length (1+target). dp[i] means the fewest number of coins needed to make up $i. The transition function is dp[i] = min(dp[i], 1+dp[i-v[j]]) where v[j] is the value of j-th coin. We should also pay attention to how to initiate the dp and the order of looping.  
```python
def coinChange(coins, amount):
    dp = [float("inf")] * (1 + amount)
    dp[0] = 0
    for coin in coins:
        for x in range(coin, amount+1):
            dp[x] = min(dp[x], 1+dp[x-coin])
    return dp[-1] if dp[-1] != float("inf") else -1
```
In _coin change II_, the goal is to get the number of combinations. The only thing that needs to be changed is the transition function. See below.
```python
def coinChange(coins, amount):
    dp = [0] * (1 + amount)
    dp[0] = 1
    for coin in coins:
        for x in range(coin, amount+1):
            dp[x] += dp[x-coin] # transition function
    return dp[-1]
```
Complexity Analysis: runtime O(S*m), S = amount, m = #coins
### 0-1 Knapsack Problem


