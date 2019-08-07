# Arrays

+ [Find the Duplicate Number](#find-the-duplicate-number)
+ [Longest Consecutive Sequence](#longest-consecutive-sequence)

## Find the Duplicate Number

https://leetcode.com/problems/find-the-duplicate-number/

```java
public int findDuplicate(int[] nums) {    
    for (int num : nums) {
        if (nums[Math.abs(num)] < 0) 
            return Math.abs(num);
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
