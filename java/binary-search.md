# Binary Search

+ [Binary Search](#binary-search)
+ [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
+ [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)

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

## Search in Rotated Sorted Array

https://leetcode.com/problems/search-in-rotated-sorted-array/

```java
public int search(int[] nums, int target) {
    if (nums == null || nums.length == 0){
        return -1;
    }
    int left = 0, right = nums.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] == target) {
            return mid;
        }

        if (nums[left] <= nums[mid]) {
            if (target >= nums[left] && target <= nums[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        } else {
            if (target > nums[mid] && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
    }
    return -1;
}
```

Solution 2:

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
        int middle = left + (right - left) / 2;

        if (nums[middle] == target) {
            return middle;
        } else if (nums[middle] < target) {
            if (nums[left] > nums[middle] && nums[left] <= target) {
                right = middle - 1;
            } else {
                left = middle + 1;
            }
        } else {
            if (nums[right] < nums[middle] && nums[right] >= target) {
                left = middle + 1;
            } else {
                right = middle - 1;
            }
        }
    }
    return -1;
}
```

## Find Minimum in Rotated Sorted Array

https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

```java
public int findMin(int[] nums) {
    if (nums.length == 1) return nums[0];

    int left = 0, right = nums.length - 1;
    if (nums[0] < nums[right]) return nums[0];

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (mid + 1 < nums.length && nums[mid] > nums[mid + 1]) {
            return nums[mid + 1];
        }

        if (mid - 1 >= 0 && nums[mid - 1] > nums[mid]) {
            return nums[mid];
        }

        if (nums[mid] > nums[0]) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```
