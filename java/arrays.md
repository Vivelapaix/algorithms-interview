# Arrays

+ [Find the Duplicate Number](#find-the-duplicate-number)

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
