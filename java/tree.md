# Tree

+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)
+ [Invert Binary Tree](#invert-binary-tree)
+ [Path Sum](#path-sum)
+ [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
+ [Subtree of Another Tree](#subtree-of-another-tree)
+ [Kth Smallest Element in a BST](#kth-smallest-element-in-a-bst)
+ [Implement Trie (Prefix Tree)](#implement-trie-prefix-tree)
+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)

## Maximum Depth of Binary Tree

https://leetcode.com/problems/maximum-depth-of-binary-tree/

```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

## Same Tree

https://leetcode.com/problems/same-tree/

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    if (p.val != q.val) return false;
    return isSameTree(p.left, q.left) &&
           isSameTree(p.right, q.right);
}
```

## Invert Binary Tree

https://leetcode.com/problems/invert-binary-tree/

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    TreeNode left = invertTree(root.left);
    TreeNode right = invertTree(root.right);
    root.left = right;
    root.right = left;
    return root;
}
```

Или итеративно

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);

    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;

        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```

## Path Sum

https://leetcode.com/problems/path-sum/

```java
public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null) return false;
    if (root.left == null && root.right == null) return root.val == sum; 
    return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
}
```

## Binary Tree Level Order Traversal

https://leetcode.com/problems/binary-tree-level-order-traversal/

`ArrayDeque` не добавляет `null` объекты, кидает исключение.

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    if (root == null) return new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<>();
    List<List<Integer>> res = new ArrayList<>();
    queue.add(root);
    int level_cnt = 1, current_level_cnt = 1;

    while(!queue.isEmpty()) {
        current_level_cnt = level_cnt;

        List<Integer> newLevel = new ArrayList<>();
        level_cnt = 0;
        while (current_level_cnt > 0) {
            TreeNode current = queue.poll();
            if (current != null) {
                newLevel.add(current.val);
                queue.add(current.left);
                queue.add(current.right);
                level_cnt += 2;
            }
            current_level_cnt--;
        }
        if (!newLevel.isEmpty()) res.add(newLevel);
    }
    return res;
}
```

## Subtree of Another Tree

https://leetcode.com/problems/subtree-of-another-tree/

```java
public boolean isSubtree(TreeNode s, TreeNode t) {
    return traverse(s, t);
}

public boolean equalsTree(TreeNode x, TreeNode y) {
    if (x == null && y == null) return true;
    if (x == null || y == null) return false;
    if (x.val != y.val) return false;
    return equalsTree(x.left, y.left) && equalsTree(x.right, y.right);
}

public boolean traverse(TreeNode s, TreeNode t) {
    return s != null && 
        (equalsTree(s, t) || traverse(s.left, t) || traverse(s.right, t));
}
```

## Kth Smallest Element in a BST

https://leetcode.com/problems/kth-smallest-element-in-a-bst/

```java
public void inOrder(TreeNode root, List<Integer> lst) {
    if (root == null) return;
    inOrder(root.left, lst);
    lst.add(root.val);
    inOrder(root.right, lst);
}

public int kthSmallest(TreeNode root, int k) {
    List<Integer> nums = new ArrayList<Integer>();
    inOrder(root, nums);
    return nums.get(k - 1);
}
```

Либо итеративно

```java
public int kthSmallest(TreeNode root, int k) {
  Deque<TreeNode> stack = new ArrayDeque<>();

  while (true) {
      while (root != null) {
          stack.push(root);
          root = root.left;
      }
      root = stack.pop();
      if (--k == 0) return root.val;
      root = root.right;
  }
}
```

## Lowest Common Ancestor of a Binary Tree

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) return null;
    if (root == p || root == q) return root;

    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);

    if (left != null && right != null) return root;
    if (left == null && right == null) return null;

    return left == null ? right : left; 
}
```

## Implement Trie (Prefix Tree)

https://leetcode.com/problems/implement-trie-prefix-tree/

```java
class Trie {
    class TrieNode {
        private TrieNode[] childrens;

        private final int R = 26;

        private boolean isEnd;

        public TrieNode() {
            childrens = new TrieNode[R];
        }

        public boolean containsKey(char ch) {
            return childrens[ch - 'a'] != null;
        }

        public TrieNode get(char ch) {
            return childrens[ch -'a'];
        }

        public void put(char ch, TrieNode node) {
            childrens[ch - 'a'] = node;
        }

        public void setEnd() {
            isEnd = true;
        }

        public boolean isEnd() {
            return isEnd;
        }
    }
    
    private TrieNode root;
    
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode());
            }
            node = node.get(c);
        }
        node.setEnd();
    }
    
    private TrieNode searchPrefix(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (node.containsKey(c)) {
                node = node.get(c);
            } else {
                return null;
            }
        }
        return node;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEnd();
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode node = searchPrefix(prefix);
        return node != null;
    }
}
```
