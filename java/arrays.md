# Arrays

+ [Find the Duplicate Number](#find-the-duplicate-number)
+ [Longest Consecutive Sequence](#longest-consecutive-sequence)
+ [Product of Array Except Self](#product-of-array-except-self)
+ [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
+ [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)
+ [3Sum](#3sum)

## Find the Duplicate Number

https://leetcode.com/problems/find-the-duplicate-number/

```java
public int findDuplicate(int[] nums) {    
    for (int num : nums) {
        if (nums[Math.abs(num)] < 0) {
            return Math.abs(num);
        }
        nums[Math.abs(num)] *= -1;
    }
    return 0;
}
```

## Longest Consecutive Sequence

https://leetcode.com/problems/longest-consecutive-sequence/

https://leetcode.com/problems/longest-consecutive-sequence/discuss/351902/java.-clearly-and-easy-understand.

```java
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<Integer>();
    int currentNum;
    int currentLen;
    int maxLen = 0;

    for (int num : nums) set.add(num);

    for (int num : set) {
        if (!set.contains(num - 1)) {
            currentNum = num;
            currentLen = 1;
            while (set.contains(currentNum + 1)) {
                currentNum += 1;
                currentLen += 1;
            }
            maxLen = Math.max(maxLen, currentLen);
        }
    }
    return maxLen;
}
```

## Product of Array Except Self

https://leetcode.com/problems/product-of-array-except-self/

```java
public int[] productExceptSelf(int[] nums) {
    int[] res = new int[nums.length];
    if (res.length == 0) return res;

    res[0] = 1;
    for (int i = 1; i < nums.length; i++) {
        res[i] = res[i - 1] * nums[i - 1];
    }

    int rightProduct = 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        res[i] = res[i] * rightProduct;
        rightProduct *= nums[i];
    }
    return res;
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

## 3Sum

https://leetcode.com/problems/3sum/

https://fizzbuzzed.com/top-interview-questions-1/

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length < 3) return res;
    Arrays.sort(nums);
    List<Integer> triplet;
    for (int i = 0; i < nums.length; i++) {

        if (i != 0 && nums[i] == nums[i - 1]) continue;
        int j = i + 1;
        int k = nums.length - 1;

        while (j < k) {
            if (nums[i] + nums[j] + nums[k] == 0) {
                triplet = new ArrayList<>();
                triplet.add(nums[i]);
                triplet.add(nums[j]);
                triplet.add(nums[k]);
                res.add(triplet);
                j++;

                while (j < k && nums[j] == nums[j - 1]) j++;
            } else if (nums[i] + nums[j] + nums[k] > 0) {
                k--;
            } else {
                j++;
            }
        }
    }
    return res;
}
```
