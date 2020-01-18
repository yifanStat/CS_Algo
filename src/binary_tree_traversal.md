# <center> Binary Tree Traversal </center>

Leetcode Problems: [LC94](https://leetcode.com/problems/binary-tree-inorder-traversal/), [LC144](https://leetcode.com/problems/binary-tree-preorder-traversal/), [LC145](https://leetcode.com/problems/binary-tree-postorder-traversal/) [LC102](https://leetcode.com/problems/binary-tree-level-order-traversal/), [LC107](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/) 

Different order of tree traversal:  
* pre-order: root -> left -> right  
* in-order: left -> root -> right  
* post-order: left -> right -> root  
* level-order: by level

To define a binary tree node class:
```python
class Node(object):
    def __init__(self, value):
        self.val = value
        self.left = None
        self.right = None
```
When comes to binary tree problems, one useful tool is recursion. Using __in-order__ traversal as an example, 
```python
def traversal(root):
    res = []
    if root:
        traversal(root.left) # (1)
        res.append(root.val) # (2)
        traversal(root.right) # (3)
    return res
```
In the case of pre-order or post-order, we should rearrange the lines (1) - (3).   
An alternative class of approaches is using stack. For example, still with the __in-order__ example,  
```python
def traversal(root):
    res = []
    stack = [root]
    while stack:
        if root:
            stack.append(root)
            root = root.left
        else:
        root = stack.pop()
        res.append(root.val) # (1)
        root = root.right
```
For other ordering, just change the position of line (1) accordingly. For example, to print pre-order, line (1) should be moved to before stack.append(root). For __post-order__, it is a bit more complex.
```python
def postOrder(root):
    stack = []
    res = []
    pre = None # point to the last popped node
    while stack or root:
        if root:
            stack.append(root)
            root = root.left
        else:
            if stack[-1].right != pre:
                root = stack[-1].right:
                pre = None
            else:
                pre = stack.pop()
                res.append(pre.val)
```
The idea to generate the left -> right -> root sequence is after finding and popping the left child, check whether the next-to-pop node has its right child already popped. To do so, use a pointer (i.e., pre) to note the last popped node. If it has a right node and it's just popped, pop this root node as well.

__Level print__ uses different schemes and the idea is to remember the number of nodes in the next layer, and then use a queue to track all the visited nodes.
```python
from collections import deque
def levelPrint(root):
    if not root: return []
    res = []
    q = deque([root])
    ct = 1
    while q:
        next_n = 0
        tmp = []
        for i in range(ct):
            node = q.popleft()
            tmp.append(node.val)
            if node.left:
                next_n += 1
                q.append(node.left)
            if node.right:
                next_n += 1
                q.append(node.right)
        res.append(tmp)
        ct = next_n
    return res
```

__Reconstruct Binary Tree__ from some traversal order: [LC106](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/), [LC1028](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/)  
Problem: Given inorder and postorder traversal of a tree, construct the binary tree.  
Recursion is very effective in solving this kind of problems. Actually, when comes to binary tree problems, thinking about recursion first would usually be useful. In each recursion, the last element of the postorder traversal is the root. Now, we just need to identify the left branch and right branch. When there is no duplicates in the tree, we just need to find the index of root in the inorder traversal. See the code below:
```python
class Solution(object):
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        if len(inorder) == 0: return None
        root_val = postorder.pop()
        root = TreeNode(root_val)
        idx = inorder.index(root_val)
        root.left = self.buildTree(inorder[:idx], postorder[:idx])
        root.right = self.buildTree(inorder[idx+1:], postorder[idx:])
        return root
```
Complexity: time O(n)
In order to reduce memory, we can utilize a help function that indicates the first and last index of the current subtree in the inorder traversal and do a inplace pop up in the postorder array. 


