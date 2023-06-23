# Design

+ [Insert Delete GetRandom O(1)](#insert-delete-getrandom-o1)
+ [Binary Search Tree Iterator](#binary-search-tree-iterator)
+ [Design an ATM Machine](#design-an-atm-machine)

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

## Design an ATM Machine

https://leetcode.com/problems/design-an-atm-machine/

1. Учесть, что deposit выполняется много раз, что приведет к переполнению при сложении, поэтому long[]
2. Цикл должен продолжаться только до тех пор, пока remainder > 0, иначе нет смысла, когда уже нечего разменивать
3. Может получиться так, что у нас банкнот 2, но при делении получаем, что нужно 3 банкноты данного номинала, поэтому берем минимум, то есть в любом случае берем 2 банкноты
4. Обязательно формируем сразу ответ, потом только в самом конце вычитаем из банкнот

```java
class ATM {

    private long[] banknotes;
    private long[] nominal;

    public ATM() {
        this.nominal = new long[]{20, 50, 100, 200, 500};
        this.banknotes = new long[nominal.length];
    }

    public void deposit(int[] banknotesCount) {
        for (int i = 0; i < banknotesCount.length; i++) {
            banknotes[i] += banknotesCount[i];
        }
    }

    public int[] withdraw(int amount) {
        int remainder = amount;
        long count = 0;
        int[] result = new int[banknotes.length];
        for (int i = banknotes.length - 1; i >= 0 && remainder > 0; i--) {          
            count = Math.min(remainder / nominal[i], banknotes[i]);
            result[i] = (int) count;
            remainder -= count * nominal[i];
        }
        if (remainder != 0) {
            return new int[]{-1};
        }
        for (int i = 0; i < banknotes.length; i++) {
            banknotes[i] -= result[i];
        }
        return result;
    }

}
```
