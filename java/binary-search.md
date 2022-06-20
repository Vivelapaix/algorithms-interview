# Binary Search

+ [Binary Search](#binary-search)

## Binary Search

https://leetcode.com/problems/binary-search/

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length;

    while (left <= right) {
        int middle = left + (right - left) / 2;
        if (nums[middle] == target) {
            return middle;
        } else if (nums[middle] < target) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }

    return -1;
}
```
