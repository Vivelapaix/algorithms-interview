# Tree

+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)

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
