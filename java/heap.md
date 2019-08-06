# Heap

+ [Top K Frequent Elements](#top-k-frequent-elements)

## Top K Frequent Elements

https://leetcode.com/problems/top-k-frequent-elements/

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
