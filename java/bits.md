# Bits

+ [Single Number](#single-number)

## Single Number

https://leetcode.com/problems/single-number/

```java
public int singleNumber(int[] nums) {
    int a = 0;
    for (int num : nums) {
        a ^= num;
    }
    return a;
}
```
