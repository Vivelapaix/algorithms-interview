# Bits

+ [Single Number](#single-number)
+ [Sum of Two Integers](#sum-of-two-integers)
+ [Number of 1 Bits](#number-of-1-bits)
+ [Counting Bits](#counting-bits)
+ [Missing Number](#missing-number)
+ [Reverse Bits](#reverse-bits)

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

## Sum of Two Integers

https://leetcode.com/problems/sum-of-two-integers/

```java
public int getSum(int a, int b) {
    int carry;
    while (b != 0) {
        carry = a & b;
        a = a ^ b;
        b = carry << 1;
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

## Counting Bits

https://leetcode.com/problems/counting-bits/

```java
public int[] countBits(int num) {
    int[] res = new int[num + 1];
    for (int i = 1; i <= num; i++) {
        if (i % 2 == 0) {
            res[i] = res[i >> 1];
        } else {                
            res[i] = res[i >> 1] + 1;
        }
    }
    return res;
}
```

## Missing Number

https://leetcode.com/problems/missing-number/

```java
public int missingNumber(int[] nums) {
    int missing = nums.length;
    for (int i = 0; i < nums.length; i++) {
        missing ^= i ^ nums[i];
    }
    return missing;
}
```

## Reverse Bits

https://leetcode.com/problems/reverse-bits/

```java
public int reverseBits(int n) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        result = (result << 1) | (n & 1);
        n >>= 1;
    }
    return result;
}
```
