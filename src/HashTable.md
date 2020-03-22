# <center>Hash Table</center>

Leetcode Problems: [LC705](https://leetcode.com/problems/design-hashset/), [LC706](https://leetcode.com/problems/design-hashmap/)

<p align="center">
    <img src="../fig/ghibli03.jpg">
</p>

hash function:  
* division method: A prime not too close to an exact power of 2 is often a good choice for m.
* multiplication method:  
* universal hashing: (don't fully understand)


collision:  
* chaining: use linked list (doubly linked list for fast deletion) - average search time O(1+a), where a = n/m.  
* open addressing: double hashing

Template of those in dealing with search, insert and delete operation.