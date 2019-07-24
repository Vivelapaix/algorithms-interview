# Tree

+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
+ [Validate Binary Search Tree](#validate-binary-search-tree)
+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)


## Lowest Common Ancestor of a Binary Tree

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

https://www.youtube.com/watch?v=13m9ZCB8gjw

```python
def lowestCommonAncestor(self, root, p, q):
    def recurse_tree(node):
        if not node: return None
        if node == p or node == q: return node

        left = recurse_tree(node.left)
        right = recurse_tree(node.right)

        if left and right: return node
        if not left and not right: return None

        return left if not right else right
    return recurse_tree(root)
```

## Validate Binary Search Tree

Используем минимальное и максимальное значения в левом и в правом поддеревьях, чтобы проверить.

https://leetcode.com/problems/validate-binary-search-tree/

```python
def isValidBST(self, root):
    def is_valid_subtree(node, lower = float('-inf'), upper = float('+inf')):
        if not node: return True

        if node.val <= lower or node.val >= upper:
            return False

        if not is_valid_subtree(node.left, lower, node.val):
            return False
        if not is_valid_subtree(node.right, node.val, upper):
            return False

        return True
```

## Binary Tree Inorder Traversal

https://leetcode.com/problems/binary-tree-inorder-traversal/

```python
def inorderTraversal(self, root):
    """
    :type root: TreeNode
    :rtype: List[int]
    """
    res = []
    def inorder_collect(node, res):
        if node:
            inorder_collect(node.left, res)
            res.append(node.val)
            inorder_collect(node.right, res)

    inorder_collect(root, res)        
    return res
```

## Symmetric Tree

https://leetcode.com/problems/symmetric-tree/

```python
def isSymmetric(self, root):
    """
    :type root: TreeNode
    :rtype: bool
    """
    def is_mirror(t1, t2):
        if not t1 and not t2: return True
        if not t1 or not t2: return False

        return t1.val == t2.val \
               and is_mirror(t1.right, t2.left)\
               and is_mirror(t1.left, t2.right)

    return is_mirror(root.left, root.right) if root else True
```
