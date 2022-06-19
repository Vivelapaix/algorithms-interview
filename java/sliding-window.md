# Sliding Window

+ [Sliding Window Maximum](#sliding-window-maximum)

## Sliding Window Maximum

https://leetcode.com/problems/sliding-window-maximum/

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> dq = new ArrayDeque();
    int[] ans = new int[nums.length - k + 1];

    for (int i = 0; i < nums.length; i++) {
        if (i < k - 1) {
            addNewElement(nums, i, dq);
        } else {
            addNewElement(nums, i, dq); 
            while (!dq.isEmpty() && dq.peek() <= i - k) {
                dq.removeFirst();
            }
            ans[i - k + 1] = nums[dq.peek()];
        }
    }

    return ans;
}

private void addNewElement(int[] nums, int curIndex, Deque<Integer> dq) {
    while (!dq.isEmpty() && nums[curIndex] > nums[dq.peekLast()]) {
        dq.removeLast();
    }
    dq.addLast(curIndex);
}
```

Пояснения:
```
Набираем окно так, чтобы в нем первым элементом был всегда максимум окна. Добавляем новый элемент из расширенного окна, выбрасываем с конца 
все ненужное (что не является максимумом), удаляем из начала очереди все индексы, которые не относятся к текущему окну, вычисляем максимум. Идея в том,
что в начале очереди всегда хранится максимум расширяемого окна

Time Complexity: O(n). 
It seems more than O(n) at first look. It can be observed that every element of array is added and removed at most once. So there are total 2n operations.
Auxiliary Space: O(k). 
Elements stored in the dequeue take O(k) space.
```
