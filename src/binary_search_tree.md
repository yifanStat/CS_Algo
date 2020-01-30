# <center>Binary Search Tree</center>

What is a BST?  
It's a special binary tree, with left node < root node < right node.  
 
We summarize some classic problems of BST.  

__1. Validate BST:__ [LC98](https://leetcode.com/problems/validate-binary-search-tree/) Given a binary tree, determine if it is a valid binary search tree (BST).  
```python
def validateBST(root, lo, hi):
    if not root: return True
    if root.val > hi or root.val < lo:
        return False
    return validate(root.left, lo, root.val) and validate(root.right, root.val, hi)
```

__2. Recover BST__ [LC99](https://leetcode.com/problems/recover-binary-search-tree/) Two elements of a binary search tree (BST) are swapped by mistake. Recover the tree without changing its structure.  

