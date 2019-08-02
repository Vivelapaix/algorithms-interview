# Dynamic Programming

+ [Climbing Stairs](#climbing-stairs)

## Climbing Stairs

https://leetcode.com/problems/climbing-stairs/

```java
public int climbStairs(int n) {
    if (n == 1) {
        return 1;
    }
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```
