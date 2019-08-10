# Arrays

+ [Find the Duplicate Number](#find-the-duplicate-number)
+ [Longest Consecutive Sequence](#longest-consecutive-sequence)
+ [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
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
