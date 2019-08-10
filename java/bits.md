# Bits

+ [Single Number](#single-number)
+ [Number of 1 Bits](#number-of-1-bits)

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

## Number of 1 Bits

https://leetcode.com/problems/number-of-1-bits/

```java
public int hammingWeight(int n) {
    int count = 0;
    for (int i = 0; i < 32; i++) {
        count += (n & (1 << i)) != 0 ? 1 : 0;
    }
    return count;
}
```

Или такое решение

```java
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        count += (n & 1);
        n >>>= 1;
    }
    return count;
}
```
