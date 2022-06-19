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
Remarks
```
We know every number is appears twice except a single number which appears only single time.

See, we already discuss a thing a that xor of a same number with itself is zero, i.e A ^ A = 0
Now, we will look some more property of xor-

1) xor of a same number with itself is zero, i.e A ^ A = 0
2) xor is commutative that means a ^ b = b ^ a.
3) xor of any number with zero is the number itself i.e A ^ 0 = A.

Suppose our array is arr[]: [5, 1, 3, 1, 3, 4, 5, 7, 4]
we will rearrange the array, and take all the numbers together, then our array looks like
                     arr[]: [1, 1, 3, 3, 4, 4, 5, 5, 7]
					 now, take xor of all numbers -
					 1 ^ 1 ^ 3 ^ 3 ^ 4 ^ 4 ^ 5 ^ 5 ^ 7   (rearrange the array)
					   0   ^   0   ^   0   ^   0   ^ 7   (see point number 1)
					               7                     (see point number 3) 
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
