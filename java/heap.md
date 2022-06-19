# Heap

+ [Top K Frequent Elements](#top-k-frequent-elements)
+ [Kth Largest Element in an Array](#kth-largest-element-in-an-array)
+ [Top K Frequent Words](#top-k-frequent-words)

## Top K Frequent Elements

https://leetcode.com/problems/top-k-frequent-elements/

Solution 1:
```java
public List<Integer> topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> count = new HashMap<>();
    for (int num : nums) {
        count.put(num, count.getOrDefault(num, 0) + 1);
    }

    PriorityQueue<Integer> heap = new PriorityQueue<>(
        (n1, n2) -> count.get(n1) - count.get(n2)
    );

    for (int cnt : count.keySet()) {
        heap.add(cnt);
        if (heap.size() > k) {
            heap.poll();
        }
    }

    List<Integer> topK = new ArrayList<>();
    while (!heap.isEmpty()) {
        topK.add(heap.poll());
    }
    Collections.reverse(topK);
    return topK;
}
```

Solution (simple) 2:
```java
public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    Queue<Integer> pq = new PriorityQueue<>(
        (a, b) -> map.get(b) - map.get(a)
    );
    int[] result = new int[k];

    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }

    for (int num : map.keySet()) {
        pq.add(num);
    } 

    for (int i = 0; i < k; i++) {
        result[i] = pq.poll();
    }

    return result;
}
```

## Kth Largest Element in an Array

https://leetcode.com/problems/kth-largest-element-in-an-array/

```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<>();

    for (int num : nums) {
        heap.add(num);
        if (heap.size() > k) {
            heap.poll();
        }
    }

    return heap.poll();
}
```

## Top K Frequent Words

https://leetcode.com/problems/top-k-frequent-words/

```java
public List<String> topKFrequent(String[] words, int k) {
    Map<String, Integer> map = new HashMap<>();
    Queue<String> pq = new PriorityQueue<>(
        (a, b) -> map.get(a) - map.get(b) == 0 ? a.compareTo(b) : map.get(b) - map.get(a)
    );
    List<String> result = new ArrayList<>();

    for (String word : words) {
        map.put(word, map.getOrDefault(word, 0) + 1);
    }

    for (String word : map.keySet()) {
        pq.add(word);
    }

    while (k-- > 0) {
        result.add(pq.poll());
    }

    return result;
}
```
