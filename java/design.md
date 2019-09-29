# Design

+ [Insert Delete GetRandom O(1)](#insert-delete-getrandom-o1)
+ [Binary Search Tree Iterator](#binary-search-tree-iterator)

## Insert Delete GetRandom O(1)

https://leetcode.com/problems/insert-delete-getrandom-o1/

```java
class RandomizedSet {

    private Map<Integer, Integer> map;
    private List<Integer> list;
    private java.util.Random random;
    
    /** Initialize your data structure here. */
    public RandomizedSet() {
        map = new HashMap<>();
        list = new ArrayList<>();
        random = new java.util.Random();
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.containsKey(val)) return false;
        
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) return false;
        
        int index = map.remove(val);
        if (index != list.size() - 1) {
            int lastValue = list.get(list.size() - 1);
            list.set(index, lastValue);
            map.put(lastValue, index);
        }
        list.remove(list.size() - 1);
        return true;
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        return list.get(random.nextInt(list.size()));
    }
}
```

## Binary Search Tree Iterator

https://leetcode.com/problems/binary-search-tree-iterator/

```java
class BSTIterator {

    Deque<TreeNode> stack;
    
    public BSTIterator(TreeNode root) {
        this.stack = new ArrayDeque<>();
        this.leftmostInorder(root);
    }
    
    private void leftmostInorder(TreeNode root) {
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode topNode = stack.pop();
        if (topNode.right != null) {
            leftmostInorder(topNode.right);
        }
        return topNode.val;    
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return stack.size() > 0;
    }
}
```
